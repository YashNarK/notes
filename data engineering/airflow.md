# Apache Airflow

Airflow is a platform for authoring, scheduling, and monitoring data pipelines. Instead of running scripts with cron and hoping they succeed, Airflow gives you:

- A **visual UI** showing which tasks ran, failed, or are pending
- **Automatic retries** with configurable delay
- **Dependency management** — Task B only starts after Task A succeeds
- **Backfill** — re-run historical pipeline runs that were missed
- **Alerting** — email or Slack notifications on failure

Airflow is not a data processing engine — it is an **orchestrator**. It tells Spark, BigQuery, Python, or Bash what to run and when.

## Table of contents

- [Core Concepts](#core-concepts)
  - [DAG](#dag)
  - [Tasks and Operators](#tasks-and-operators)
  - [Sensors](#sensors)
  - [Execution Date and Scheduling](#execution-date-and-scheduling)
- [Complete DAG Example](#complete-dag-example)
- [Common Operators](#common-operators)
  - [PythonOperator](#pythonoperator)
  - [BashOperator](#bashoperator)
  - [BigQueryInsertJobOperator](#bigqueryinsertjoboperator)
  - [GCSObjectExistenceSensor](#gcsobjectexistencesensor)
  - [SparkSubmitOperator](#sparksubmitoperator)
- [XComs — Passing Data Between Tasks](#xcoms--passing-data-between-tasks)
- [Trigger Rules](#trigger-rules)
- [Task Dependencies](#task-dependencies)
- [Variables and Connections](#variables-and-connections)
- [Backfill and Catchup](#backfill-and-catchup)
- [Best Practices](#best-practices)
- [Common Gotchas](#common-gotchas)
- [Resources](#resources)

---

## Core Concepts

### DAG

A **DAG** (Directed Acyclic Graph) is one pipeline definition. It describes:
- Which tasks exist
- The order and dependencies between them
- When to run (schedule)

```
DAG: "daily_transactions_etl"   (runs at 2 AM every day)
│
├── [Task 1] wait_for_source_file  (Sensor)
│       ↓ (on success)
├── [Task 2] run_python_etl  (PythonOperator)
│       ↓ (on success)
└── [Task 3] run_aggregation  (BigQueryOperator)
```

### Tasks and Operators

A **Task** is one node in the DAG — one unit of work. An **Operator** defines the type of work:

| Operator | Does what |
|---|---|
| `PythonOperator` | Runs a Python function |
| `BashOperator` | Runs a bash command |
| `BigQueryInsertJobOperator` | Runs a SQL query in BigQuery |
| `SparkSubmitOperator` | Submits a Spark job to a cluster |
| `GCSObjectExistenceSensor` | Waits until a file exists in GCS |
| `HttpSensor` | Waits until an HTTP endpoint returns 200 |
| `EmailOperator` | Sends an email |

### Sensors

A **Sensor** is a special task that *waits* for an external condition before proceeding:

```python
from airflow.providers.google.cloud.sensors.gcs import GCSObjectExistenceSensor

wait_for_file = GCSObjectExistenceSensor(
    task_id="wait_for_source_file",
    bucket="my-data-bucket",
    object="input/{{ ds }}/data.csv",  # {{ ds }} = execution date (YYYY-MM-DD)
    timeout=3600,          # fail if not found after 1 hour
    poke_interval=60,      # check every 60 seconds
    mode="poke"            # "poke" (blocks a worker slot) or "reschedule" (releases slot between checks)
)
```

### Execution Date and Scheduling

Airflow schedules DAG runs based on a **logical execution date** (not wall-clock time).

```python
# schedule_interval examples
schedule_interval="@daily"        # once a day at midnight
schedule_interval="0 2 * * *"    # cron: 2 AM every day
schedule_interval="@hourly"       # every hour
schedule_interval=None            # manual trigger only

# Execution date vs run date
# A daily DAG with start_date=2024-01-01 and schedule="@daily":
# - First run executes on 2024-01-02, but execution_date = 2024-01-01
# - The run processes data FOR the execution date, not the run date
# This is by design: "process yesterday's data" pipelines use {{ ds }} = execution_date
```

---

## Complete DAG Example

```python
from airflow import DAG
from airflow.operators.python import PythonOperator
from airflow.providers.google.cloud.operators.bigquery import BigQueryInsertJobOperator
from airflow.providers.google.cloud.sensors.gcs import GCSObjectExistenceSensor
from datetime import datetime, timedelta
import pandas as pd

default_args = {
    "owner": "data-engineering",
    "retries": 3,
    "retry_delay": timedelta(minutes=5),
    "email_on_failure": True,
    "email": ["de-alerts@company.com"],
    "email_on_retry": False
}

with DAG(
    dag_id="daily_transactions_etl",
    description="Daily ETL: GCS raw data → BigQuery clean table",
    start_date=datetime(2024, 1, 1),
    schedule_interval="0 2 * * *",   # 2 AM every day
    catchup=False,                    # do not backfill missed runs automatically
    default_args=default_args,
    tags=["transactions", "etl", "daily"]
) as dag:

    # Task 1: Sensor — wait until the source file is available
    wait_for_file = GCSObjectExistenceSensor(
        task_id="wait_for_source_file",
        bucket="company-raw-data",
        object="transactions/{{ ds }}/data.csv",
        timeout=3600,
        poke_interval=60,
        mode="reschedule"   # releases worker slot while waiting
    )

    # Task 2: Python ETL
    def run_etl(**context):
        execution_date = context["ds"]    # "2024-01-15"
        print(f"Running ETL for: {execution_date}")

        # Read from GCS, clean, and load to BigQuery
        from google.cloud import bigquery, storage

        gcs_client = storage.Client()
        bucket = gcs_client.bucket("company-raw-data")
        blob = bucket.blob(f"transactions/{execution_date}/data.csv")
        content = blob.download_as_text()

        import io
        df = pd.read_csv(io.StringIO(content))
        df = df.dropna(subset=["user_id", "amount"])
        df = df[df["amount"] > 0]
        df["etl_date"] = execution_date

        bq_client = bigquery.Client()
        job_config = bigquery.LoadJobConfig(write_disposition="WRITE_APPEND")
        job = bq_client.load_table_from_dataframe(
            df, "project.dataset.transactions", job_config=job_config
        )
        job.result()

        # Push row count to XCom for downstream tasks to use
        context["ti"].xcom_push(key="rows_loaded", value=job.output_rows)
        print(f"Loaded {job.output_rows} rows")

    etl_task = PythonOperator(
        task_id="run_python_etl",
        python_callable=run_etl,
        provide_context=True
    )

    # Task 3: BigQuery aggregation after load
    run_aggregation = BigQueryInsertJobOperator(
        task_id="run_daily_aggregation",
        configuration={
            "query": {
                "query": """
                    INSERT INTO `project.dataset.daily_summary`
                    SELECT
                        DATE(transaction_date)  AS txn_date,
                        COUNT(*)                AS total_transactions,
                        SUM(amount)             AS total_volume,
                        COUNT(DISTINCT user_id) AS unique_users
                    FROM `project.dataset.transactions`
                    WHERE DATE(transaction_date) = '{{ ds }}'
                    GROUP BY 1
                """,
                "useLegacySql": False
            }
        }
    )

    # Define execution order using bitshift operators
    wait_for_file >> etl_task >> run_aggregation
```

---

## Common Operators

### PythonOperator

```python
from airflow.operators.python import PythonOperator

def my_function(param1, param2, **context):
    execution_date = context["ds"]
    print(f"Running for {execution_date} with {param1}, {param2}")
    return "result_value"   # stored in XCom automatically

task = PythonOperator(
    task_id="my_python_task",
    python_callable=my_function,
    op_kwargs={"param1": "hello", "param2": 42},  # pass extra arguments
    provide_context=True
)
```

### BashOperator

```python
from airflow.operators.bash import BashOperator

run_script = BashOperator(
    task_id="run_shell_script",
    bash_command="python /opt/scripts/process.py --date {{ ds }}",
    env={"ENV": "production"}
)

# Execution date available as {{ ds }}, {{ ds_nodash }}, {{ ts }}, etc.
```

### BigQueryInsertJobOperator

```python
from airflow.providers.google.cloud.operators.bigquery import BigQueryInsertJobOperator

run_sql = BigQueryInsertJobOperator(
    task_id="run_bigquery_sql",
    configuration={
        "query": {
            "query": "SELECT COUNT(*) FROM `project.dataset.table` WHERE date = '{{ ds }}'",
            "useLegacySql": False,
            "destinationTable": {
                "projectId": "my-project",
                "datasetId": "results",
                "tableId": "daily_counts"
            },
            "writeDisposition": "WRITE_APPEND"
        }
    },
    location="US"
)
```

### GCSObjectExistenceSensor

```python
from airflow.providers.google.cloud.sensors.gcs import GCSObjectExistenceSensor

sensor = GCSObjectExistenceSensor(
    task_id="wait_for_file",
    bucket="my-bucket",
    object="data/{{ ds }}/input.parquet",
    timeout=7200,           # 2 hours max wait
    poke_interval=300,      # check every 5 minutes
    mode="reschedule"       # preferred — does not hold a worker slot while waiting
)
```

### SparkSubmitOperator

```python
from airflow.providers.apache.spark.operators.spark_submit import SparkSubmitOperator

run_spark = SparkSubmitOperator(
    task_id="run_spark_etl",
    application="/opt/spark/jobs/etl_job.py",
    name="daily-etl-{{ ds }}",
    conn_id="spark_default",
    application_args=["--date", "{{ ds }}"],
    executor_memory="4g",
    executor_cores=2,
    num_executors=10,
    conf={"spark.sql.shuffle.partitions": "200"}
)
```

---

## XComs — Passing Data Between Tasks

XComs (cross-communications) let tasks share small values (row counts, file paths, status flags). Not for large data — use GCS or BigQuery for that.

```python
# Push a value from one task
def extract(**context):
    row_count = fetch_and_count_rows()
    context["ti"].xcom_push(key="row_count", value=row_count)
    # Alternatively: return value is auto-pushed with key="return_value"
    return row_count

# Pull the value in another task
def validate(**context):
    # Pull from a specific task and key
    count = context["ti"].xcom_pull(task_ids="extract_task", key="row_count")
    if count == 0:
        raise ValueError("No rows extracted — pipeline halted")
    print(f"Validating {count} rows")

extract_task = PythonOperator(task_id="extract_task", python_callable=extract, provide_context=True)
validate_task = PythonOperator(task_id="validate_task", python_callable=validate, provide_context=True)

extract_task >> validate_task
```

---

## Trigger Rules

By default, a task runs only when **all** upstream tasks succeed. You can override this:

```python
from airflow.utils.trigger_rule import TriggerRule

# Run this cleanup task even if upstream tasks failed
cleanup = PythonOperator(
    task_id="cleanup_temp_files",
    python_callable=do_cleanup,
    trigger_rule=TriggerRule.ALL_DONE  # runs regardless of upstream outcome
)

# Other useful trigger rules:
# TriggerRule.ALL_SUCCESS    (default) — all upstream must succeed
# TriggerRule.ALL_FAILED     — run only if all upstream failed
# TriggerRule.ONE_SUCCESS    — run if at least one upstream succeeded
# TriggerRule.ONE_FAILED     — run if at least one upstream failed
# TriggerRule.NONE_FAILED    — run if no upstream failed (skipped is OK)
```

---

## Task Dependencies

```python
# Sequential: A → B → C
task_a >> task_b >> task_c

# Parallel then converge: A and B run in parallel, both must finish before C
[task_a, task_b] >> task_c

# Fan-out: A completes, then B and C run in parallel
task_a >> [task_b, task_c]

# Diamond pattern: A → (B, C) → D
task_a >> [task_b, task_c]
[task_b, task_c] >> task_d

# Explicit set_upstream / set_downstream (equivalent to >> and <<)
task_b.set_upstream(task_a)
task_b.set_downstream(task_c)
```

---

## Variables and Connections

**Variables** store configuration values outside the code:

```python
from airflow.models import Variable

# Set in Airflow UI: Admin → Variables
bucket_name = Variable.get("gcs_data_bucket")
config = Variable.get("etl_config", deserialize_json=True)  # JSON variable
```

**Connections** store credentials for external systems (BigQuery, Spark, S3, databases):

```python
from airflow.hooks.base import BaseHook

# Connections set in UI: Admin → Connections
conn = BaseHook.get_connection("my_postgres_conn")
print(conn.host, conn.login, conn.password)

# Operators use conn_id to look up the connection:
BigQueryInsertJobOperator(task_id="...", gcp_conn_id="google_cloud_default", ...)
```

---

## Backfill and Catchup

**Catchup** (`catchup=True`): when a DAG is unpaused or `start_date` is changed, Airflow automatically schedules all missed runs between `start_date` and today.

**Backfill** (CLI): manually trigger runs for a historical date range:

```bash
# Backfill for January 2024
airflow dags backfill daily_transactions_etl \
    --start-date 2024-01-01 \
    --end-date 2024-01-31

# Dry run: see which runs would be triggered without executing
airflow dags backfill daily_transactions_etl \
    --start-date 2024-01-01 \
    --end-date 2024-01-31 \
    --dry-run
```

> **Tip:** always set `catchup=False` in production DAGs unless you specifically need historical backfill. Otherwise, enabling a new DAG can immediately trigger hundreds of historical runs.

---

## Best Practices

| Practice | Why |
|---|---|
| Keep tasks small and focused | Easier to retry just the failed step; better visibility |
| Make tasks idempotent | Running a task twice should produce the same result — use `WRITE_TRUNCATE` or `INSERT OVERWRITE` |
| Use `mode="reschedule"` on Sensors | Releases worker slots while the sensor waits — avoids starving other tasks |
| Never put credentials in DAG code | Use Connections and Variables — credentials stay out of source control |
| Set meaningful `email_on_failure` | Operators should alert on failure; you shouldn't check the UI manually |
| Use `catchup=False` by default | Prevents accidental mass-scheduling of historical runs |
| Pin `start_date` to a fixed datetime | Never use `datetime.now()` as start_date — it changes every deploy |
| Template everything time-dependent | Use `{{ ds }}` and `{{ ts }}` — never hardcode dates in SQL or file paths |

---

## Common Gotchas

| Gotcha | Explanation | Fix |
|---|---|---|
| Task not appearing | DAG file has a syntax error | Check the Airflow UI → DAGs → Import Errors |
| `start_date=datetime.now()` | Changes every deployment — causes missed/duplicate runs | Use a fixed date: `datetime(2024, 1, 1)` |
| XCom with large data | XCom stores values in the metadata DB — not meant for DataFrames | Write data to GCS/BigQuery; push only the file path via XCom |
| Sensor starving workers | `mode="poke"` sensors hold a worker slot while waiting | Switch to `mode="reschedule"` |
| Zombie tasks | Worker crashes mid-task — Airflow marks it stuck | Configure `zombie_threshold_secs` and heartbeat monitoring |
| `provide_context=True` | Required in Airflow 1.x to get `**context`; deprecated in Airflow 2.x | In Airflow 2.x, `**context` is passed automatically — `provide_context` is ignored but harmless |

---

## Resources

- [Apache Airflow Documentation](https://airflow.apache.org/docs/)
- [Airflow Providers (GCP, AWS, Spark)](https://airflow.apache.org/docs/apache-airflow-providers/index.html)
- **Related notes:**
  - [Data Engineering Index](data_engineer.md)
  - [Apache Spark — Running Jobs via Airflow](spark.md)
  - [BigQuery — Operators and SQL](bigquery.md)
  - [Data Engineering System Design](system_design.md)
