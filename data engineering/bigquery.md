# Google BigQuery

BigQuery is Google Cloud's fully managed, serverless data warehouse. It is designed for analytical queries (OLAP) — fast reads over billions of rows with no infrastructure to manage. You pay per query by bytes scanned (or use flat-rate slots).

## Table of contents

- [Architecture](#architecture)
  - [Columnar Storage — Dremel](#columnar-storage--dremel)
  - [Serverless Execution](#serverless-execution)
  - [BigQuery vs Bigtable](#bigquery-vs-bigtable)
- [Tables and Schemas](#tables-and-schemas)
  - [Creating Tables](#creating-tables)
  - [Partitioned Tables](#partitioned-tables)
  - [Clustered Tables](#clustered-tables)
- [BigQuery SQL — Key Features](#bigquery-sql--key-features)
  - [Window Functions](#window-functions)
  - [Arrays and Structs](#arrays-and-structs)
  - [UNNEST](#unnest)
  - [Approximate Aggregations](#approximate-aggregations)
  - [Date and Time Functions](#date-and-time-functions)
  - [Recursive CTEs](#recursive-ctes)
- [Query Optimisation](#query-optimisation)
- [Loading Data into BigQuery](#loading-data-into-bigquery)
  - [From GCS with Python Client](#from-gcs-with-python-client)
  - [From a DataFrame](#from-a-dataframe)
  - [From Spark](#from-spark)
  - [Streaming Inserts](#streaming-inserts)
- [Exporting Data](#exporting-data)
- [Common Gotchas](#common-gotchas)
- [Resources](#resources)

---

## Architecture

### Columnar Storage — Dremel

BigQuery uses **Dremel**, a columnar storage format. Data is stored column-by-column rather than row-by-row:

```
Row-based (traditional RDBMS):
  Row 1: [user1, 500.0, 2024-01-01, USD]
  Row 2: [user2, 200.0, 2024-01-02, GBP]
  Row 3: [user3,  75.0, 2024-01-03, USD]

Columnar (BigQuery / Parquet):
  user_id column:   [user1, user2, user3]
  amount column:    [500.0, 200.0,  75.0]
  date column:      [2024-01-01, 2024-01-02, 2024-01-03]
  currency column:  [USD, GBP, USD]
```

When you run `SELECT SUM(amount) FROM transactions`, BigQuery reads **only** the `amount` column — skipping `user_id`, `date`, and `currency` entirely. On a petabyte table this reduces scan cost by orders of magnitude.

### Serverless Execution

BigQuery separates compute from storage:
- **Storage**: Colossus (Google's distributed file system) — always on, billed per GB/month
- **Compute**: Dremel workers — spun up on demand per query, billed per TB scanned (or per slot-hour on reservations)

There is no cluster to provision, scale, or maintain.

### BigQuery vs Bigtable

| | BigQuery | Bigtable |
|---|---|---|
| **Type** | Data warehouse (OLAP) | Wide-column NoSQL store (OLTP) |
| **Read pattern** | Full-table analytical scans | Single-row point lookups |
| **Write latency** | Seconds (batch load) / milliseconds (streaming inserts) | Sub-millisecond |
| **Query language** | Standard SQL | No SQL — key-based GET/PUT |
| **Typical latency** | Seconds to minutes | < 10 ms |
| **Use cases** | Analytics reports, ML feature history, dashboards | Real-time feature serving, session data, fraud score lookup |

---

## Tables and Schemas

### Creating Tables

```sql
-- Standard table with explicit schema
CREATE TABLE IF NOT EXISTS `project.dataset.transactions` (
    transaction_id  STRING      NOT NULL,
    user_id         STRING,
    amount          FLOAT64,
    currency        STRING,
    transaction_date TIMESTAMP,
    status          STRING
)
OPTIONS (
    description = "Cleaned transaction records",
    labels = [("team", "data-engineering"), ("env", "production")]
);

-- Create table from query result
CREATE OR REPLACE TABLE `project.dataset.monthly_summary` AS
SELECT
    DATE_TRUNC(transaction_date, MONTH) AS month,
    currency,
    COUNT(*) AS num_transactions,
    SUM(amount) AS total_volume
FROM `project.dataset.transactions`
GROUP BY 1, 2;
```

### Partitioned Tables

Partitioning physically splits table data into segments by a column value. Queries that filter on the partition column only scan the matching partitions — BigQuery shows "Bytes processed" before running if you hover in the UI.

```sql
-- Partition by date (most common — time-series data)
CREATE TABLE `project.dataset.transactions`
PARTITION BY DATE(transaction_date)
AS SELECT * FROM `project.dataset.raw_transactions`;

-- Query uses partition pruning automatically — only January scanned:
SELECT SUM(amount)
FROM `project.dataset.transactions`
WHERE transaction_date BETWEEN '2024-01-01' AND '2024-01-31';

-- Partition by integer range (e.g. user_id ranges)
CREATE TABLE `project.dataset.users`
PARTITION BY RANGE_BUCKET(user_id, GENERATE_ARRAY(0, 10000000, 100000))
AS SELECT * FROM `project.dataset.raw_users`;

-- Partition expiry: auto-delete old partitions (useful for raw/staging tables)
CREATE TABLE `project.dataset.raw_events`
PARTITION BY DATE(event_time)
OPTIONS (partition_expiration_days = 90)   -- delete partitions older than 90 days
AS SELECT * FROM `project.dataset.raw_events_source`;
```

### Clustered Tables

Clustering sorts data within each partition by one or more columns. Queries filtering on clustered columns scan less data within each partition.

```sql
-- Combined partitioning + clustering (the recommended pattern)
CREATE TABLE `project.dataset.transactions`
PARTITION BY DATE(transaction_date)
CLUSTER BY user_id, currency
AS SELECT * FROM `project.dataset.raw_transactions`;

-- This query benefits from BOTH partition pruning (date filter) AND clustering (user_id filter):
SELECT SUM(amount)
FROM `project.dataset.transactions`
WHERE transaction_date BETWEEN '2024-01-01' AND '2024-01-31'
  AND user_id = 'u_12345';
```

> **When to cluster:** columns frequently used in `WHERE`, `JOIN ON`, or `GROUP BY`. Best candidates: user_id, region, status, category.

---

## BigQuery SQL — Key Features

### Window Functions

```sql
-- Running total per user over time
SELECT
    user_id,
    transaction_date,
    amount,
    SUM(amount) OVER (
        PARTITION BY user_id
        ORDER BY transaction_date
    ) AS running_total

-- Rolling 7-day spend (inclusive of current row)
SELECT
    user_id,
    transaction_date,
    amount,
    SUM(amount) OVER (
        PARTITION BY user_id
        ORDER BY transaction_date
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
    ) AS rolling_7day_spend

-- Rank users by total spend within each country
FROM (
    SELECT user_id, country, SUM(amount) AS total_spend
    FROM `project.dataset.transactions`
    JOIN `project.dataset.users` USING (user_id)
    GROUP BY 1, 2
)
SELECT
    user_id, country, total_spend,
    RANK()       OVER (PARTITION BY country ORDER BY total_spend DESC) AS rank_in_country,
    DENSE_RANK() OVER (PARTITION BY country ORDER BY total_spend DESC) AS dense_rank,
    PERCENT_RANK() OVER (PARTITION BY country ORDER BY total_spend DESC) AS pct_rank

-- Compare to previous row (lag) and next row (lead)
SELECT
    user_id, transaction_date, amount,
    LAG(amount,  1) OVER (PARTITION BY user_id ORDER BY transaction_date) AS prev_amount,
    LEAD(amount, 1) OVER (PARTITION BY user_id ORDER BY transaction_date) AS next_amount,
    amount - LAG(amount, 1) OVER (PARTITION BY user_id ORDER BY transaction_date) AS delta
FROM `project.dataset.transactions`;
```

### Arrays and Structs

```sql
-- ARRAY_AGG: collect multiple rows into an array column
SELECT
    user_id,
    ARRAY_AGG(
        STRUCT(transaction_id, amount, transaction_date)
        ORDER BY transaction_date DESC
        LIMIT 5
    ) AS last_5_transactions
FROM `project.dataset.transactions`
GROUP BY user_id;

-- Result: one row per user with a nested array of the 5 most recent transactions

-- STRUCT: a named record type (like a row within a row)
SELECT
    user_id,
    STRUCT(
        SUM(amount) AS total,
        COUNT(*) AS count,
        AVG(amount) AS average
    ) AS spend_summary
FROM `project.dataset.transactions`
GROUP BY user_id;
```

### UNNEST

`UNNEST` explodes an array back into individual rows — the inverse of `ARRAY_AGG`.

```sql
-- Flatten the nested array created by ARRAY_AGG above
SELECT
    u.user_id,
    txn.transaction_id,
    txn.amount,
    txn.transaction_date
FROM `project.dataset.user_transactions` u,
UNNEST(u.last_5_transactions) AS txn;

-- Unnest a repeated field (e.g. tags array)
SELECT product_id, tag
FROM `project.dataset.products`,
UNNEST(tags) AS tag;
```

### Approximate Aggregations

For very large tables where exact answers aren't needed:

```sql
-- APPROX_COUNT_DISTINCT: HyperLogLog++ algorithm, < 1% error, much faster
SELECT APPROX_COUNT_DISTINCT(user_id) AS approx_unique_users
FROM `project.dataset.transactions`;
-- vs COUNT(DISTINCT user_id) which is exact but may be 10-100× slower on huge tables

-- APPROX_TOP_COUNT: top N values by frequency
SELECT APPROX_TOP_COUNT(currency, 5) AS top_currencies
FROM `project.dataset.transactions`;

-- APPROX_QUANTILES: percentile estimates
SELECT APPROX_QUANTILES(amount, 100)[OFFSET(50)] AS median_amount,
       APPROX_QUANTILES(amount, 100)[OFFSET(95)] AS p95_amount
FROM `project.dataset.transactions`;
```

### Date and Time Functions

```sql
-- Truncate to granularity
SELECT DATE_TRUNC(transaction_date, MONTH)   AS month
SELECT DATE_TRUNC(transaction_date, WEEK)    AS week
SELECT DATE_TRUNC(transaction_date, QUARTER) AS quarter

-- Extract parts
SELECT EXTRACT(YEAR  FROM transaction_date) AS year
SELECT EXTRACT(MONTH FROM transaction_date) AS month
SELECT EXTRACT(HOUR  FROM transaction_timestamp) AS hour

-- Arithmetic
SELECT DATE_ADD(transaction_date, INTERVAL 7 DAY)   AS one_week_later
SELECT DATE_DIFF(end_date, start_date, DAY)         AS days_between
SELECT TIMESTAMP_DIFF(end_ts, start_ts, SECOND)     AS duration_seconds

-- Format
SELECT FORMAT_DATE('%Y-%m', transaction_date) AS year_month
SELECT FORMAT_TIMESTAMP('%Y-%m-%dT%H:%M:%SZ', event_time) AS iso_timestamp
```

### Recursive CTEs

```sql
-- Traverse a referral/hierarchy tree up to 10 levels deep
WITH RECURSIVE referral_chain AS (
    -- Anchor: root users (no referrer)
    SELECT user_id, referred_by, 1 AS depth, CAST(user_id AS STRING) AS path
    FROM `project.dataset.referrals`
    WHERE referred_by IS NULL

    UNION ALL

    -- Recursive: join each user to their referrer chain
    SELECT r.user_id, r.referred_by, rc.depth + 1, CONCAT(rc.path, ' → ', r.user_id)
    FROM `project.dataset.referrals` r
    JOIN referral_chain rc ON r.referred_by = rc.user_id
    WHERE rc.depth < 10   -- prevent infinite loops
)
SELECT * FROM referral_chain ORDER BY depth, path;
```

---

## Query Optimisation

Run this checklist when a BigQuery query is slow or expensive:

```
1. PARTITION PRUNING
   Always filter on the partition column in WHERE.
   BAD:  WHERE EXTRACT(YEAR FROM txn_date) = 2024
   GOOD: WHERE txn_date BETWEEN '2024-01-01' AND '2024-12-31'
   (BigQuery can't prune on function-wrapped partition columns)

2. COLUMN PRUNING
   SELECT only columns you need. Never SELECT *.
   Columnar storage means unused columns cost nothing to skip — but SELECT * still bills you.

3. CLUSTERING
   Add CLUSTER BY on columns in WHERE / JOIN predicates.

4. AVOID CROSS JOINS
   CROSS JOIN of 1M × 1M rows = 1 trillion rows. Almost never intentional.

5. MATERIALISE HOT CTEs
   A CTE used multiple times in one query is re-executed each time.
   For expensive CTEs, consider a temp table:
   CREATE TEMP TABLE my_cte AS SELECT ... FROM big_table ...;
   Then reference my_cte instead.

6. USE APPROX FUNCTIONS
   Replace COUNT(DISTINCT col) with APPROX_COUNT_DISTINCT(col) when exact is not required.

7. LIMIT DURING DEVELOPMENT
   Always add a date filter or LIMIT while testing to avoid scanning the full table.

8. USE DML SPARINGLY
   UPDATE, DELETE, MERGE are expensive (they rewrite entire partitions).
   Prefer INSERT OVERWRITE a partition or partition expiry for deletes.
```

---

## Loading Data into BigQuery

### From GCS with Python Client

```python
from google.cloud import bigquery

client = bigquery.Client(project="my-project")

job_config = bigquery.LoadJobConfig(
    source_format=bigquery.SourceFormat.PARQUET,
    write_disposition=bigquery.WriteDisposition.WRITE_APPEND,
    # For CSV:
    # source_format=bigquery.SourceFormat.CSV,
    # skip_leading_rows=1,
    # autodetect=True,
)

load_job = client.load_table_from_uri(
    "gs://my-bucket/data/2024-01-15/*.parquet",
    "my-project.dataset.transactions",
    job_config=job_config
)
load_job.result()   # wait for completion
print(f"Loaded {load_job.output_rows} rows")
```

### From a DataFrame

```python
from google.cloud import bigquery
import pandas as pd

client = bigquery.Client()
df = pd.DataFrame({"user_id": ["u1", "u2"], "amount": [100.0, 200.0]})

job_config = bigquery.LoadJobConfig(
    write_disposition="WRITE_APPEND",
    schema=[
        bigquery.SchemaField("user_id", "STRING"),
        bigquery.SchemaField("amount",  "FLOAT64"),
    ]
)

job = client.load_table_from_dataframe(df, "project.dataset.table", job_config=job_config)
job.result()
```

### From Spark

```python
# Requires: spark-bigquery-with-dependencies JAR on the classpath
df.write \
  .format("bigquery") \
  .option("table", "my-project.dataset.transactions") \
  .option("temporaryGcsBucket", "my-temp-bucket") \
  .option("writeMethod", "indirect")   # indirect = stage in GCS first, then load
  .mode("overwrite") \
  .save()
```

### Streaming Inserts

For low-latency ingestion (seconds), use the streaming API — but note it is more expensive and has no deduplication guarantee by default:

```python
from google.cloud import bigquery

client = bigquery.Client()
table_id = "my-project.dataset.events"

rows = [
    {"event_id": "e001", "user_id": "u123", "amount": 55.0, "ts": "2024-01-15T10:00:00"},
    {"event_id": "e002", "user_id": "u456", "amount": 30.0, "ts": "2024-01-15T10:00:01"},
]

errors = client.insert_rows_json(table_id, rows)
if errors:
    print(f"Streaming insert errors: {errors}")
else:
    print("Rows streamed successfully")
```

---

## Exporting Data

```python
from google.cloud import bigquery

client = bigquery.Client()

extract_job = client.extract_table(
    "my-project.dataset.transactions",
    "gs://my-bucket/exports/transactions_*.parquet",  # wildcard for multiple files
    job_config=bigquery.ExtractJobConfig(
        destination_format=bigquery.DestinationFormat.PARQUET,
        compression=bigquery.Compression.SNAPPY
    )
)
extract_job.result()
```

```sql
-- Export via SQL (to GCS) — can filter before exporting
EXPORT DATA OPTIONS (
    uri="gs://my-bucket/exports/jan2024/*.parquet",
    format="PARQUET",
    overwrite=true
) AS
SELECT * FROM `project.dataset.transactions`
WHERE transaction_date BETWEEN '2024-01-01' AND '2024-01-31';
```

---

## Common Gotchas

| Gotcha | Explanation | Fix |
|---|---|---|
| Wrapping partition column in a function | `WHERE YEAR(date) = 2024` disables partition pruning | Use `WHERE date BETWEEN '2024-01-01' AND '2024-12-31'` |
| `SELECT *` billing | You are billed for all columns scanned even if you only read one | Always select specific columns |
| Streaming insert cost | ~$0.01 per 200 MB — much more expensive than batch loads | Use batch loads (GCS → BigQuery) unless you need sub-minute latency |
| CTE re-execution | A CTE used twice is executed twice (not cached) | Materialise as a temp table if it's expensive |
| DML write costs | UPDATE/DELETE rewrite full partitions | Use partition overwrites or INSERT patterns instead |
| `NUMERIC` vs `FLOAT64` | `FLOAT64` has floating-point precision issues for money | Use `NUMERIC` (exact, up to 29 digits) for financial amounts |
| Dataset and table location | Dataset region can't be changed after creation | Choose region carefully — matches where data is (e.g. `europe-west1` for EU data) |

---

## Resources

- [BigQuery Documentation](https://cloud.google.com/bigquery/docs)
- [BigQuery SQL Reference](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax)
- [BigQuery Pricing](https://cloud.google.com/bigquery/pricing)
- **Related notes:**
  - [Data Engineering Index](data_engineer.md)
  - [Apache Spark — Writing to BigQuery](spark.md)
  - [Apache Airflow — BigQuery Operators](airflow.md)
  - [Data Modelling](data_modelling.md)
