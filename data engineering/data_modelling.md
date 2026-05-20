# Data Modelling

Data modelling defines how data is organised in a warehouse — the schema, relationships, and granularity of tables. Good modelling makes queries fast, intuitive, and maintainable.

## Table of contents

- [OLTP vs OLAP](#oltp-vs-olap)
- [Star Schema](#star-schema)
  - [Fact Tables](#fact-tables)
  - [Dimension Tables](#dimension-tables)
  - [Example — Transactions Warehouse](#example--transactions-warehouse)
- [Snowflake Schema](#snowflake-schema)
- [Slowly Changing Dimensions — SCD](#slowly-changing-dimensions--scd)
  - [SCD Type 1 — Overwrite](#scd-type-1--overwrite)
  - [SCD Type 2 — History with Validity Dates](#scd-type-2--history-with-validity-dates)
  - [SCD Type 3 — Previous Value Column](#scd-type-3--previous-value-column)
- [Medallion Architecture](#medallion-architecture)
  - [Bronze Layer](#bronze-layer)
  - [Silver Layer](#silver-layer)
  - [Gold Layer](#gold-layer)
- [Data Vault](#data-vault)
- [Surrogate Keys vs Natural Keys](#surrogate-keys-vs-natural-keys)
- [Grain — the Most Important Concept](#grain--the-most-important-concept)
- [Normalisation vs Denormalisation](#normalisation-vs-denormalisation)
- [Common Gotchas](#common-gotchas)
- [Resources](#resources)

---

## OLTP vs OLAP

| | OLTP (Transactional) | OLAP (Analytical) |
|---|---|---|
| **Purpose** | Record individual transactions | Analyse patterns across many records |
| **Operations** | Many small INSERT/UPDATE/DELETE | Few large SELECT queries |
| **Schema** | Normalised (3NF) — minimise redundancy | Denormalised — minimise joins |
| **Row count** | Thousands to millions | Billions to trillions |
| **Examples** | MySQL, PostgreSQL, SQL Server | BigQuery, Snowflake, Redshift |
| **Query latency** | Milliseconds | Seconds to minutes |

---

## Star Schema

The star schema is the standard data warehouse pattern. One central **fact table** surrounded by multiple **dimension tables** — the visual layout resembles a star.

```
                        ┌──────────────┐
                        │  DIM_DATE    │
                        │ date_key PK  │
                        │ year         │
                        │ quarter      │
                        │ month        │
                        │ day_of_week  │
                        └──────┬───────┘
                               │
┌──────────────┐     ┌─────────┴────────┐     ┌──────────────┐
│  DIM_USER    │     │  FACT_TRANSACTIONS│     │ DIM_MERCHANT │
│ user_key PK  ├─────┤ transaction_id PK│─────┤ merchant_key │
│ user_id      │     │ user_key FK      │     │ merchant_id  │
│ full_name    │     │ merchant_key FK  │     │ name         │
│ country      │     │ date_key FK      │     │ category     │
│ tier         │     │ currency_key FK  │     │ country      │
└──────────────┘     │ amount           │     └──────────────┘
                     │ transaction_type │
                     └────────┬─────────┘
                              │
                     ┌────────┴─────────┐
                     │  DIM_CURRENCY    │
                     │ currency_key PK  │
                     │ code             │
                     │ name             │
                     │ symbol           │
                     └──────────────────┘
```

### Fact Tables

A fact table records **what happened** — one row per event (a transaction, a click, a login). It contains:

- **Measures** — numeric values you want to analyse (`amount`, `duration`, `quantity`)
- **Foreign keys** — links to dimension tables (`user_key`, `date_key`, `product_key`)
- **Degenerate dimensions** — identifiers with no dimension table (`transaction_id`, `order_number`)

```sql
CREATE TABLE `warehouse.fact_transactions` (
    transaction_id   STRING,          -- degenerate dimension (no dim table needed)
    user_key         INT64,           -- FK → dim_user
    merchant_key     INT64,           -- FK → dim_merchant
    date_key         INT64,           -- FK → dim_date (format: YYYYMMDD as integer)
    currency_key     INT64,           -- FK → dim_currency
    amount           NUMERIC,         -- measure
    fee_amount       NUMERIC,         -- measure
    transaction_type STRING           -- low-cardinality attribute (can stay in fact)
);
```

**Fact table types:**
| Type | Row represents | Example |
|---|---|---|
| **Transaction fact** | One event | One payment |
| **Periodic snapshot** | State at a point in time | Daily account balance |
| **Accumulating snapshot** | Lifecycle of a process | Order from placed → shipped → delivered |

### Dimension Tables

A dimension table records **who, what, where, when** — descriptive context for the facts.

```sql
CREATE TABLE `warehouse.dim_user` (
    user_key      INT64,       -- surrogate key (PK)
    user_id       STRING,      -- natural/business key
    full_name     STRING,
    email         STRING,
    country       STRING,
    account_tier  STRING,      -- "Gold", "Silver", "Basic"
    created_date  DATE
);

CREATE TABLE `warehouse.dim_date` (
    date_key      INT64,       -- surrogate key: YYYYMMDD as integer (e.g. 20240115)
    full_date     DATE,
    year          INT64,
    quarter       INT64,
    month         INT64,
    month_name    STRING,      -- "January"
    day_of_week   INT64,
    day_name      STRING,      -- "Monday"
    is_weekend    BOOLEAN,
    is_holiday    BOOLEAN
);
```

> **Always pre-build a dim_date table** covering your full data range. Never compute date parts on the fly in queries — join to dim_date instead. This makes time-based analysis much faster and more consistent.

### Example — Transactions Warehouse

```sql
-- Query: monthly revenue by country for 2024
SELECT
    d.year,
    d.month_name,
    u.country,
    SUM(f.amount)  AS total_revenue,
    COUNT(*)       AS num_transactions,
    COUNT(DISTINCT f.user_key) AS unique_users
FROM `warehouse.fact_transactions` f
JOIN `warehouse.dim_date`     d ON f.date_key     = d.date_key
JOIN `warehouse.dim_user`     u ON f.user_key     = u.user_key
JOIN `warehouse.dim_currency` c ON f.currency_key = c.currency_key
WHERE d.year = 2024
  AND c.code = 'USD'
GROUP BY 1, 2, 3
ORDER BY 1, 2, 3;
```

---

## Snowflake Schema

A snowflake schema normalises dimension tables into sub-dimensions, removing redundancy at the cost of more joins.

```
DIM_USER  →  DIM_COUNTRY  →  DIM_REGION
            (country normalised out of dim_user)
```

**Use snowflake when:** storage is a constraint, or dimension tables are very large and highly normalised source data makes denormalisation expensive.

**Prefer star schema when:** query performance matters more than storage efficiency (usually the case in cloud warehouses where storage is cheap).

---

## Slowly Changing Dimensions — SCD

Dimensions change over time. A user changes their country; a product changes its category. SCD patterns decide how to handle this history.

### SCD Type 1 — Overwrite

Simply overwrite the old value. No history is kept.

```sql
-- User moved from US to UK
UPDATE `warehouse.dim_user`
SET country = 'UK'
WHERE user_id = 'u123';
-- All historical facts linked to user_key now show UK, even past transactions
```

**Use when:** history is irrelevant (e.g. correcting a data entry error).

### SCD Type 2 — History with Validity Dates

Add a new row for each change, with validity date ranges. The most common pattern.

```sql
CREATE TABLE `warehouse.dim_user_scd2` (
    user_key       INT64,     -- surrogate key (increments with each new version)
    user_id        STRING,    -- business key (same for all versions of the same user)
    full_name      STRING,
    country        STRING,
    account_tier   STRING,
    effective_from DATE,      -- when this version became active
    effective_to   DATE,      -- when this version expired (NULL = currently active)
    is_current     BOOLEAN    -- TRUE for the active version
);

-- User u123 moved from US to UK on 2024-06-01:
-- user_key=1: user_id='u123', country='US', effective_from=2020-01-01, effective_to=2024-05-31, is_current=FALSE
-- user_key=2: user_id='u123', country='UK', effective_from=2024-06-01, effective_to=NULL,       is_current=TRUE

-- Query: get the user's country AT THE TIME of each transaction
SELECT
    t.transaction_id,
    t.amount,
    t.transaction_date,
    u.country AS country_at_time_of_transaction
FROM `warehouse.fact_transactions` t
JOIN `warehouse.dim_user_scd2` u
    ON t.user_id = u.user_id
    AND t.transaction_date BETWEEN u.effective_from
                                AND COALESCE(u.effective_to, DATE '9999-12-31');

-- Loading a new SCD2 record (Python)
```

```python
from google.cloud import bigquery
from datetime import date

def apply_scd2_change(client, user_id: str, new_values: dict, change_date: date):
    """Expire the current row and insert a new version."""
    expire_sql = f"""
        UPDATE `warehouse.dim_user_scd2`
        SET effective_to = DATE_SUB('{change_date}', INTERVAL 1 DAY),
            is_current = FALSE
        WHERE user_id = '{user_id}' AND is_current = TRUE
    """
    client.query(expire_sql).result()

    new_row = {
        "user_id": user_id,
        "effective_from": change_date.isoformat(),
        "effective_to": None,
        "is_current": True,
        **new_values
    }
    errors = client.insert_rows_json("warehouse.dim_user_scd2", [new_row])
    if errors:
        raise RuntimeError(f"Insert failed: {errors}")
```

### SCD Type 3 — Previous Value Column

Add a column to store the previous value. Only keeps one level of history.

```sql
CREATE TABLE `warehouse.dim_user_scd3` (
    user_key          INT64,
    user_id           STRING,
    current_country   STRING,    -- current value
    previous_country  STRING,    -- one previous value only
    country_changed   DATE       -- date of last change
);
```

**Use when:** you only need to track one prior value (e.g. previous tier before a recent upgrade).

---

## Medallion Architecture

The medallion architecture (also called Bronze–Silver–Gold or multi-hop architecture) organises data into three quality tiers. Common in Databricks and GCP architectures.

```
Raw Source Systems
      ↓
 ┌─────────┐    ┌──────────┐    ┌────────┐
 │ BRONZE  │ →  │  SILVER  │ →  │  GOLD  │
 └─────────┘    └──────────┘    └────────┘
```

### Bronze Layer

Raw data, exactly as it arrived. **Never modified or deleted.**

```
Purpose:  Audit trail, source of truth, reprocessing starting point
Format:   JSON, CSV, Avro — whatever the source produces
Location: gs://bucket/bronze/transactions/2024/01/15/raw.json
Schema:   May be inferred or minimal — preserve original field names
Retention: Permanent (or very long — years)
```

```python
# Bronze load: just land the raw data, nothing else
import json
from google.cloud import storage

def land_to_bronze(raw_payload: dict, event_date: str, bucket: str):
    client = storage.Client()
    blob = client.bucket(bucket).blob(
        f"bronze/transactions/{event_date}/{raw_payload['id']}.json"
    )
    blob.upload_from_string(json.dumps(raw_payload))
```

### Silver Layer

Cleaned, validated, and standardised data. Schema enforced, types cast, nulls handled.

```
Purpose:  Reliable analytical base — "single source of truth"
Format:   Parquet or Delta Lake
Location: BigQuery: project.silver.transactions
Schema:   Explicit schema, nullable columns documented
Rules:    Drop rows with null PKs; standardise dates; cast types
```

```sql
-- Silver transformation: clean and standardise bronze data
CREATE OR REPLACE TABLE `project.silver.transactions` AS
SELECT
    SAFE_CAST(transaction_id AS STRING)     AS transaction_id,
    SAFE_CAST(user_id AS STRING)            AS user_id,
    SAFE_CAST(amount AS NUMERIC)            AS amount,
    UPPER(TRIM(currency))                   AS currency,     -- standardise
    PARSE_TIMESTAMP('%Y-%m-%dT%H:%M:%SZ', raw_timestamp) AS transaction_ts,
    DATE(PARSE_TIMESTAMP('%Y-%m-%dT%H:%M:%SZ', raw_timestamp)) AS transaction_date,
    CURRENT_TIMESTAMP()                     AS etl_loaded_at
FROM `project.bronze.raw_transactions`
WHERE transaction_id IS NOT NULL
  AND user_id IS NOT NULL
  AND SAFE_CAST(amount AS NUMERIC) > 0;   -- SAFE_CAST returns NULL on failure instead of erroring
```

### Gold Layer

Business-ready aggregates, feature tables, and denormalised views. Optimised for consumption.

```
Purpose:  Power dashboards, ML models, business reports
Format:   BigQuery tables (partitioned + clustered for fast queries)
Location: BigQuery: project.gold.daily_user_summary
Schema:   Denormalised — join dimensions in during transformation
```

```sql
-- Gold table: daily user spend summary
CREATE OR REPLACE TABLE `project.gold.daily_user_summary`
PARTITION BY transaction_date
CLUSTER BY user_id
AS
SELECT
    t.transaction_date,
    t.user_id,
    u.country,
    u.account_tier,
    COUNT(*)            AS num_transactions,
    SUM(t.amount)       AS total_spend,
    AVG(t.amount)       AS avg_transaction,
    MAX(t.amount)       AS max_transaction
FROM `project.silver.transactions` t
JOIN `project.silver.users`        u ON t.user_id = u.user_id
GROUP BY 1, 2, 3, 4;
```

---

## Data Vault

An alternative to star schema for highly auditable, source-system agnostic warehouses. Uses three table types:

| Table type | Contains | Example |
|---|---|---|
| **Hub** | Business keys only | `hub_user(user_key, user_id, load_date, source)` |
| **Link** | Relationships between hubs | `link_transaction(txn_key, user_key, merchant_key, load_date)` |
| **Satellite** | Descriptive attributes + history | `sat_user_details(user_key, load_date, country, tier)` |

**Use Data Vault when:** multiple source systems feed the same entity, full audit history is required, or the schema changes frequently.

---

## Surrogate Keys vs Natural Keys

| | Surrogate Key | Natural Key |
|---|---|---|
| **Definition** | System-generated integer (no business meaning) | Business identifier (user_id, email, order_number) |
| **Uniqueness** | Guaranteed by the warehouse | Depends on source system |
| **SCD support** | Each version gets a new surrogate key | Same key for all versions |
| **Join performance** | Fast (integer comparison) | Slower (string comparison) |
| **Example** | `user_key = 10042` | `user_id = 'ext_usr_A8F2'` |

> **Rule:** always use a surrogate key as the dimension PK. Store the natural/business key separately for joining to source data.

---

## Grain — the Most Important Concept

The **grain** is the most atomic level of detail in a fact table — what does one row represent?

```
"One row = one payment transaction"           ← transaction grain
"One row = one day of account balance"        ← daily snapshot grain
"One row = one product line in an order"      ← line-item grain
```

**Define the grain before building any tables.** If you mix grains (some rows are transactions, some are summaries), queries will produce incorrect results.

---

## Normalisation vs Denormalisation

| | Normalised (OLTP) | Denormalised (OLAP) |
|---|---|---|
| **Redundancy** | Minimal — no repeated data | Accepted — dimensions copied into facts |
| **Update cost** | Low — change in one place | High — must update all denormalised copies |
| **Query joins** | Many joins required | Few joins — data pre-joined |
| **Query speed** | Slower for analytics | Faster for analytics |
| **Storage** | Compact | Larger |

In a data warehouse, **denormalise deliberately**. Pre-join slow-moving dimension data (user country, product category) into the fact or gold table so dashboards don't pay join costs on every query.

---

## Common Gotchas

| Gotcha | Explanation | Fix |
|---|---|---|
| Mixed grain in fact table | Some rows are individual events; others are rolled-up summaries | Split into two fact tables with different grains |
| Overloaded fact table | Dozens of measure columns, many NULL for each row | Split into separate fact tables per process area |
| Missing SCD strategy | Dimension values overwritten without considering history impact | Define SCD type per dimension during design |
| Natural key as surrogate | Using `user_id` string as FK in fact table | Add an integer `user_key` surrogate; keep `user_id` in dim |
| No dim_date table | Date functions computed on the fly in every query | Pre-build a dim_date table; join to it |
| Fact table too wide | 100+ columns in one fact table | Consider column-store partitioning or separate fact tables |

---

## Resources

- [Kimball Group — Dimensional Modelling](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/)
- [Databricks — Medallion Architecture](https://www.databricks.com/glossary/medallion-architecture)
- **Related notes:**
  - [Data Engineering Index](data_engineer.md)
  - [BigQuery — Partitioning and Clustering](bigquery.md)
  - [Data Engineering System Design](system_design.md)
