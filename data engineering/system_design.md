# Data Engineering System Design

System design in data engineering covers how to architect data pipelines that are reliable, scalable, and maintainable at large scale. This includes choosing the right tools for each layer, handling failures, ensuring data quality, and making trade-off decisions.

## Table of contents

- [Framework for Answering DE System Design Questions](#framework-for-answering-de-system-design-questions)
- [Batch vs Streaming](#batch-vs-streaming)
- [Real-time Streaming Pipeline](#real-time-streaming-pipeline)
  - [Architecture Pattern](#architecture-pattern)
  - [Component Selection Guide](#component-selection-guide)
- [Batch ETL Pipeline](#batch-etl-pipeline)
  - [Architecture Pattern](#architecture-pattern-1)
  - [Idempotency and Reprocessing](#idempotency-and-reprocessing)
- [Lambda Architecture](#lambda-architecture)
- [Kappa Architecture](#kappa-architecture)
- [Data Quality and Observability](#data-quality-and-observability)
  - [Validation Layers](#validation-layers)
  - [Alerting on Anomalies](#alerting-on-anomalies)
- [Scalability Patterns](#scalability-patterns)
  - [Horizontal Partitioning](#horizontal-partitioning)
  - [Backpressure Handling](#backpressure-handling)
  - [Circuit Breaker](#circuit-breaker)
- [Resilience Patterns](#resilience-patterns)
  - [Dead Letter Queue](#dead-letter-queue)
  - [Exponential Backoff and Retry](#exponential-backoff-and-retry)
  - [Idempotent Processing](#idempotent-processing)
- [Storage Layer Trade-offs](#storage-layer-trade-offs)
- [Worked Example — Real-time Fraud Detection Pipeline](#worked-example--real-time-fraud-detection-pipeline)
- [Resources](#resources)

---

## Framework for Answering DE System Design Questions

Use this structure when asked to design a data pipeline in an interview or planning session:

```
Step 1 — Clarify requirements (2–3 min)
  - What is the data source? (API, database CDC, event stream, file upload)
  - What is the throughput? (events/sec, GB/day)
  - What latency is required? (real-time <1s, near-real-time <1min, batch hourly/daily)
  - Who are the consumers? (dashboard, ML model, downstream service, analyst)
  - What are the SLA and data quality requirements?

Step 2 — High-level design (5 min)
  - Draw the data flow: Source → Ingestion → Processing → Storage → Serving
  - Choose tools for each layer

Step 3 — Deep dive (10 min)
  - Partitioning strategy (how data is split for parallelism)
  - Fault tolerance (what happens when a component fails)
  - Data quality checks (where validation happens)
  - Scaling strategy (what happens when volume 10×)

Step 4 — Trade-offs (3 min)
  - Explicitly state what you chose and what you gave up
  - "I chose X over Y because of Z — the trade-off is ..."
```

---

## Batch vs Streaming

| Dimension | Batch | Streaming |
|---|---|---|
| **Latency** | Minutes to hours | Milliseconds to seconds |
| **Throughput** | Very high (terabytes per run) | High (millions of events/sec) |
| **Complexity** | Lower — simpler to debug and replay | Higher — windowing, watermarks, late data |
| **Cost** | Pay per job run | Pay for always-on infrastructure |
| **Use cases** | Nightly reports, ML training, data warehouse loads | Fraud detection, live dashboards, alerting |
| **Tools** | Spark, BigQuery, dbt, Dataproc | Kafka, Pub/Sub, Dataflow, Flink, Spark Streaming |
| **Replay** | Easy — re-run the job for any date range | Harder — requires offset replay or re-read from source |

> **Rule of thumb:** if the business decision is made in minutes or hours, use batch. If it must be acted on in seconds, use streaming.

---

## Real-time Streaming Pipeline

### Architecture Pattern

```
Data Sources (mobile app, web, IoT devices)
        ↓
[ Ingestion Layer ]
  Kafka / GCP Pub/Sub
  - Decouples producers from consumers
  - Durable buffer absorbs traffic spikes
        ↓
[ Stream Processing Layer ]
  Apache Flink / Apache Beam (Dataflow) / Spark Structured Streaming
  - Parse and validate events
  - Enrich with reference data (joins to dimension tables)
  - Aggregate with time windows (tumbling, sliding, session)
  - Apply business logic
        ↓
[ Serving Layer ]  (write to multiple sinks)
  ├── Bigtable / Redis        ← real-time feature store (<10ms lookups)
  ├── Kafka topic             ← downstream services consume decisions
  ├── BigQuery (streaming)   ← analytics and monitoring dashboards
  └── Pub/Sub topic          ← trigger notifications or Lambda functions
        ↓
[ Orchestration & Monitoring ]
  Airflow (batch jobs) + Cloud Monitoring (streaming metrics)
```

### Component Selection Guide

| Requirement | Choose |
|---|---|
| Exactly-once semantics, long retention, replay | Apache Kafka |
| GCP-native, serverless, event-driven | GCP Pub/Sub |
| Stateful aggregations, complex windowing, SQL-friendly | Apache Flink |
| GCP-native stream processing with Beam API | Cloud Dataflow |
| Already have Spark, want streaming too | Spark Structured Streaming |
| Sub-10ms point lookups from serving layer | Cloud Bigtable |
| Millisecond cache, ephemeral signals | Redis |

---

## Batch ETL Pipeline

### Architecture Pattern

```
Source Systems (databases, APIs, SaaS tools)
        ↓
[ Extraction ]
  Pull snapshots or CDC (Change Data Capture) deltas
  Land to object storage: GCS / S3 / ADLS (bronze layer)
        ↓
[ Orchestration ]
  Apache Airflow (DAG scheduling, dependency management, retries)
        ↓
[ Transformation ]
  Spark on Dataproc / dbt on BigQuery / Pandas for small data
  Apply: cleaning, deduplication, type casting, business rules
        ↓
[ Loading ]
  Write to data warehouse (BigQuery / Snowflake / Redshift)
  Partitioned and clustered for query performance
        ↓
[ Serving ]
  BI tools (Looker, Tableau, Metabase)
  ML feature store
  Downstream analytics APIs
```

### Idempotency and Reprocessing

Design every batch job to be **idempotent** — running it twice for the same date produces the same result:

```python
# Pattern 1: WRITE_TRUNCATE on the target partition
# If the job runs again for 2024-01-15, it overwrites only that partition
df.write \
  .format("bigquery") \
  .option("table", "project.dataset.transactions") \
  .option("writeDisposition", "WRITE_TRUNCATE") \
  .option("datePartition", "20240115") \
  .save()

# Pattern 2: Overwrite a date partition in BigQuery
bq_client.query("""
    DELETE FROM `project.dataset.transactions`
    WHERE transaction_date = '2024-01-15'
""").result()
# Then load fresh data for that date

# Pattern 3: Stage → merge (upsert)
# Write to a staging table, then MERGE into the target
bq_client.query("""
    MERGE `project.clean.transactions` AS target
    USING `project.staging.transactions_2024_01_15` AS source
    ON target.transaction_id = source.transaction_id
    WHEN MATCHED THEN UPDATE SET ...
    WHEN NOT MATCHED THEN INSERT ROW
""").result()
```

---

## Lambda Architecture

Combines a **batch layer** (accurate, slow) with a **speed layer** (fast, approximate) to serve both real-time and historical queries.

```
Raw Data Stream
    ├── Batch Layer  (Spark batch, nightly)    → Historical accurate view
    │                                              (BigQuery, data warehouse)
    └── Speed Layer (Kafka + Flink, real-time) → Recent approximate view
                                                   (Redis, Bigtable)

Serving Layer merges results:
    Query = Batch view (up to yesterday) + Speed view (last 24h)
```

**Pros:** high accuracy for historical; low latency for recent.
**Cons:** two codebases to maintain (batch logic and stream logic) — they can drift.

---

## Kappa Architecture

A simplification of Lambda — **everything is a stream**. Historical reprocessing is done by replaying the event log from Kafka.

```
All Data → Kafka (long retention) → Stream Processor (Flink/Dataflow) → Serving Layer

Reprocessing: reset consumer offset to beginning → re-run stream job → overwrite output
```

**Pros:** single codebase, simpler operations.
**Cons:** requires long retention in Kafka (can be expensive), more complex windowing logic.

**Choose Lambda when:** you have existing batch jobs and need to add real-time on top.
**Choose Kappa when:** building from scratch, data volume fits Kafka retention, team is stream-first.

---

## Data Quality and Observability

### Validation Layers

Apply validation at multiple stages — not just at the end:

```python
import pandas as pd
from typing import List, Tuple

class DataQualityCheck:
    def __init__(self, df: pd.DataFrame):
        self.df = df
        self.failures: List[Tuple[str, int]] = []

    def not_null(self, columns: list) -> "DataQualityCheck":
        for col in columns:
            count = self.df[col].isna().sum()
            if count > 0:
                self.failures.append((f"null_{col}", int(count)))
        return self

    def no_duplicates(self, key: str) -> "DataQualityCheck":
        count = len(self.df) - self.df[key].nunique()
        if count > 0:
            self.failures.append((f"duplicate_{key}", count))
        return self

    def value_range(self, col: str, min_val=None, max_val=None) -> "DataQualityCheck":
        mask = pd.Series([False] * len(self.df))
        if min_val is not None:
            mask |= self.df[col] < min_val
        if max_val is not None:
            mask |= self.df[col] > max_val
        count = mask.sum()
        if count > 0:
            self.failures.append((f"out_of_range_{col}", int(count)))
        return self

    def assert_pass(self) -> None:
        if self.failures:
            summary = "; ".join(f"{name}: {count} rows" for name, count in self.failures)
            raise ValueError(f"Data quality checks failed: {summary}")
        print("All data quality checks passed.")

# Usage in a pipeline
df = pd.read_parquet("gs://bucket/data/2024-01-15.parquet")
DataQualityCheck(df) \
    .not_null(["transaction_id", "user_id", "amount"]) \
    .no_duplicates("transaction_id") \
    .value_range("amount", min_val=0) \
    .assert_pass()
```

### Alerting on Anomalies

```python
from google.cloud import bigquery
import smtplib

def check_daily_volume_anomaly(client: bigquery.Client, table: str, alert_email: str):
    """Alert if today's row count is < 50% of the 7-day average."""
    result = client.query(f"""
        SELECT
            today_count,
            avg_7day,
            today_count / NULLIF(avg_7day, 0) AS ratio
        FROM (
            SELECT
                COUNTIF(DATE(created_at) = CURRENT_DATE()) AS today_count,
                AVG(daily_count) AS avg_7day
            FROM (
                SELECT DATE(created_at) AS d, COUNT(*) AS daily_count
                FROM `{table}`
                WHERE created_at >= DATE_SUB(CURRENT_DATE(), INTERVAL 8 DAY)
                  AND DATE(created_at) < CURRENT_DATE()
                GROUP BY 1
            )
        )
    """).result()

    row = list(result)[0]
    if row.ratio is not None and row.ratio < 0.5:
        message = (
            f"⚠️ Volume anomaly in {table}: "
            f"today={row.today_count}, 7-day avg={row.avg_7day:.0f}, "
            f"ratio={row.ratio:.1%}"
        )
        print(message)
        # send_alert(alert_email, message)  # integrate with email/Slack
```

---

## Scalability Patterns

### Horizontal Partitioning

Split data across workers by a key so each worker handles an independent shard:

```python
from concurrent.futures import ProcessPoolExecutor
from typing import List

def process_partition(date_partition: str) -> dict:
    """Process one day's data independently."""
    df = pd.read_parquet(f"gs://bucket/raw/{date_partition}/")
    df = df.dropna(subset=["user_id"]).drop_duplicates("transaction_id")
    df.to_parquet(f"gs://bucket/clean/{date_partition}/", index=False)
    return {"partition": date_partition, "rows": len(df)}

def process_all_partitions(partitions: List[str], max_workers: int = 8) -> List[dict]:
    with ProcessPoolExecutor(max_workers=max_workers) as executor:
        results = list(executor.map(process_partition, partitions))
    return results

# Each partition is independent — safe to process in parallel
date_partitions = ["2024-01-01", "2024-01-02", "2024-01-03"]
results = process_all_partitions(date_partitions)
```

### Backpressure Handling

When a consumer can't keep up with a producer, apply backpressure to prevent memory overflow:

```python
import asyncio
from asyncio import Queue

async def producer(queue: Queue, items: list):
    for item in items:
        await queue.put(item)         # blocks if queue is full — natural backpressure
        print(f"Produced: {item}")
    await queue.put(None)             # sentinel to signal completion

async def consumer(queue: Queue, semaphore: asyncio.Semaphore):
    while True:
        item = await queue.get()
        if item is None:
            break
        async with semaphore:         # limit concurrent processing
            await process(item)
        queue.task_done()

async def run_pipeline(items: list, max_concurrent: int = 10):
    queue: Queue = asyncio.Queue(maxsize=max_concurrent * 2)  # bounded queue
    semaphore = asyncio.Semaphore(max_concurrent)
    await asyncio.gather(
        producer(queue, items),
        consumer(queue, semaphore)
    )

async def process(item):
    await asyncio.sleep(0.1)   # simulate async I/O work
    print(f"Processed: {item}")
```

### Circuit Breaker

Prevent cascading failures by stopping calls to a failing service and failing fast:

```python
import time
from enum import Enum

class CircuitState(Enum):
    CLOSED   = "closed"    # normal operation — requests pass through
    OPEN     = "open"      # failing — requests blocked immediately
    HALF_OPEN = "half_open" # testing recovery — one request allowed through

class CircuitBreaker:
    def __init__(self, failure_threshold: int = 5, recovery_timeout: int = 60):
        self.failure_threshold = failure_threshold
        self.recovery_timeout  = recovery_timeout
        self.failure_count     = 0
        self.last_failure_time = None
        self.state             = CircuitState.CLOSED

    def call(self, func, *args, **kwargs):
        if self.state == CircuitState.OPEN:
            if time.time() - self.last_failure_time > self.recovery_timeout:
                self.state = CircuitState.HALF_OPEN
            else:
                raise RuntimeError("Circuit breaker OPEN — service unavailable")

        try:
            result = func(*args, **kwargs)
            self._on_success()
            return result
        except Exception as e:
            self._on_failure()
            raise

    def _on_success(self):
        self.failure_count = 0
        self.state = CircuitState.CLOSED

    def _on_failure(self):
        self.failure_count += 1
        self.last_failure_time = time.time()
        if self.failure_count >= self.failure_threshold:
            self.state = CircuitState.OPEN
```

---

## Resilience Patterns

### Dead Letter Queue

Route messages that fail processing to a separate queue for inspection and later replay:

```
Main Queue: "transactions"
      ↓ (consumer reads)
  [Processing Attempt]
      ↓ FAILS (max retries exceeded)
DLQ: "transactions_dlq"
      ↓ (ops team reviews, fixes, replays)
Main Queue: "transactions" (re-injected after fix)
```

### Exponential Backoff and Retry

```python
import time
import random
from typing import Callable, TypeVar

T = TypeVar("T")

def with_exponential_backoff(
    func: Callable[..., T],
    max_retries: int = 5,
    base_delay: float = 1.0,
    max_delay: float = 60.0,
    jitter: bool = True
) -> T:
    """Retry func with exponential backoff and optional jitter."""
    for attempt in range(max_retries):
        try:
            return func()
        except Exception as e:
            if attempt == max_retries - 1:
                raise   # re-raise on final attempt

            delay = min(base_delay * (2 ** attempt), max_delay)
            if jitter:
                delay *= (0.5 + random.random() * 0.5)  # ±50% jitter

            print(f"Attempt {attempt + 1} failed: {e}. Retrying in {delay:.1f}s...")
            time.sleep(delay)

# Usage
def call_external_api():
    # ...API call that might fail transiently...
    pass

result = with_exponential_backoff(call_external_api, max_retries=5, base_delay=2.0)
```

### Idempotent Processing

Ensure that processing the same message twice produces the same result:

```python
from google.cloud import bigquery
import hashlib, json

def process_idempotently(client: bigquery.Client, event: dict, table: str):
    """Use a deterministic event_hash to deduplicate on write."""
    # Create a stable hash of the event's identity fields
    identity = {"user_id": event["user_id"], "timestamp": event["timestamp"], "amount": event["amount"]}
    event["event_hash"] = hashlib.sha256(json.dumps(identity, sort_keys=True).encode()).hexdigest()

    # MERGE: insert only if this hash hasn't been seen before
    client.query(f"""
        MERGE `{table}` AS target
        USING (SELECT @event_hash AS event_hash) AS source
        ON target.event_hash = source.event_hash
        WHEN NOT MATCHED THEN INSERT ROW
    """, job_config=bigquery.QueryJobConfig(
        query_parameters=[bigquery.ScalarQueryParameter("event_hash", "STRING", event["event_hash"])]
    )).result()
```

---

## Storage Layer Trade-offs

| Layer | Technology | Access Pattern | Latency | Cost |
|---|---|---|---|---|
| **Hot cache** | Redis | Key-value GET | < 1 ms | High |
| **Operational store** | Bigtable | Row key lookup | 5–10 ms | Medium |
| **Object storage** | GCS / S3 | File GET | 50–200 ms | Very low |
| **Data warehouse** | BigQuery | Full-table SQL scan | 1–60 s | Low (pay-per-query) |
| **Message queue** | Kafka / Pub/Sub | Sequential read | 5–50 ms | Medium |

**Choosing storage for a feature store:**

```
Real-time inference (< 50ms budget):
  → Redis (in-memory, sub-ms) for hot features
  → Bigtable (SSD-backed, 10ms) for cold features

Batch model training:
  → BigQuery (scan full feature history)
  → GCS Parquet (cost-effective for very large feature tables)

Stream-computed features:
  → Flink/Dataflow computes features from Kafka
  → Writes to Bigtable (serving) and BigQuery (monitoring)
```

---

## Worked Example — Real-time Fraud Detection Pipeline

**Requirements:**
- Input: payment transactions at ~500 events/sec
- Latency: fraud flag must be available within 200ms of the transaction
- Signals: velocity (transactions in last 5 min), amount, merchant category, user's historical pattern
- Output: fraud decision visible to payment service + stored for analytics

**Design:**

```
Payment Service
      ↓ publish event
[ GCP Pub/Sub: "raw_transactions" ]
      ↓ streaming pull
[ Cloud Dataflow (Apache Beam) ]
  ├── Parse + validate JSON
  ├── Enrich: lookup user features from Bigtable (user history, risk score)
  ├── Compute velocity: count transactions in sliding 5-min window
  ├── Score: call fraud ML model (Vertex AI endpoint or embedded model)
  └── Route:
        ├── High risk  → Pub/Sub "flagged_transactions" → Payment Service (block)
        └── All events → BigQuery streaming insert  → Analytics dashboard
                       → Bigtable upsert           → Update user feature store

[ Cloud Bigtable ]  — real-time feature serving
  Row key: user_id
  Columns: last_txn_amount, txn_count_5min, risk_score, country

[ BigQuery ]  — analytics + model monitoring
  Table: fraud_events (partitioned by date, clustered by user_id)

[ Airflow (nightly batch) ]
  BigQuery raw transactions → Spark/Dataproc
  → Aggregate features (last 90 days per user)
  → Retrain fraud model → deploy to Vertex AI
  → Refresh Bigtable feature store
```

**Key trade-off decisions:**

| Decision | Choice | Rationale |
|---|---|---|
| Feature store | Bigtable | < 10ms latency; BigQuery is too slow for real-time lookup |
| Stream processor | Dataflow | GCP-native, auto-scales, handles late events with watermarks |
| Message queue | Pub/Sub | GCP-native, serverless; Kafka would require cluster management |
| Model retraining | Nightly batch | Real-time scoring uses yesterday's model; retraining in the hot path is too risky |
| Exactly-once | Dataflow + Bigtable upsert | Prevents double-counting fraud signals |

---

## Resources

- [Google Cloud Data Engineering Architecture](https://cloud.google.com/architecture/data-engineering)
- [Designing Data-Intensive Applications (Kleppmann)](https://dataintensive.net/)
- [Streaming Systems (Akidau et al.)](https://www.oreilly.com/library/view/streaming-systems/9781491983867/)
- **Related notes:**
  - [Data Engineering Index](data_engineer.md)
  - [Apache Kafka & GCP Pub/Sub](kafka.md)
  - [Apache Spark](spark.md)
  - [Apache Airflow](airflow.md)
  - [BigQuery](bigquery.md)
  - [Data Modelling](data_modelling.md)
