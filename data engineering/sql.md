# Advanced SQL for Data Engineering

This file covers SQL patterns that go beyond basic CRUD — window functions, CTEs, complex aggregations, and query optimisation techniques used in analytical and data engineering contexts. Examples use BigQuery-compatible SQL (Standard SQL).

## Table of contents

- [Window Functions](#window-functions)
  - [Ranking Functions](#ranking-functions)
  - [Offset Functions](#offset-functions)
  - [Aggregate Window Functions](#aggregate-window-functions)
  - [Frame Specification](#frame-specification)
- [Common Table Expressions (CTEs)](#common-table-expressions-ctes)
  - [Basic CTE](#basic-cte)
  - [Chained CTEs](#chained-ctes)
  - [Recursive CTEs](#recursive-ctes)
- [Complex Aggregation Patterns](#complex-aggregation-patterns)
  - [Conditional Aggregation](#conditional-aggregation)
  - [Month-over-Month Growth](#month-over-month-growth)
  - [Consecutive Days Pattern](#consecutive-days-pattern)
  - [Session Gap Analysis](#session-gap-analysis)
  - [Top-N per Group](#top-n-per-group)
  - [Deduplication](#deduplication)
- [Set Operations](#set-operations)
- [JOIN Patterns](#join-patterns)
  - [Self Join](#self-join)
  - [Anti Join](#anti-join)
  - [Non-equi Join](#non-equi-join)
- [Data Quality Checks in SQL](#data-quality-checks-in-sql)
- [Performance Patterns](#performance-patterns)
- [Common Gotchas](#common-gotchas)
- [Resources](#resources)

---

## Window Functions

Window functions compute a value for each row using a **window** of related rows — without collapsing rows into groups like `GROUP BY` does.

```sql
function_name() OVER (
    PARTITION BY col1, col2   -- group rows (optional)
    ORDER BY col3 DESC        -- order within the group (required for ranking/offset)
    ROWS BETWEEN ... AND ...  -- frame (optional, for aggregate windows)
)
```

### Ranking Functions

```sql
SELECT
    user_id,
    country,
    total_spend,

    -- ROW_NUMBER: unique sequential rank, no ties
    ROW_NUMBER() OVER (PARTITION BY country ORDER BY total_spend DESC) AS row_num,

    -- RANK: tied rows get the same rank; next rank skips (1,1,3)
    RANK()       OVER (PARTITION BY country ORDER BY total_spend DESC) AS rank,

    -- DENSE_RANK: tied rows get the same rank; next rank does NOT skip (1,1,2)
    DENSE_RANK() OVER (PARTITION BY country ORDER BY total_spend DESC) AS dense_rank,

    -- NTILE(n): divide rows into n buckets (quartiles when n=4)
    NTILE(4)     OVER (PARTITION BY country ORDER BY total_spend DESC) AS spend_quartile,

    -- PERCENT_RANK: 0.0 to 1.0 percentile position
    PERCENT_RANK() OVER (PARTITION BY country ORDER BY total_spend DESC) AS pct_rank

FROM user_spend_summary;

-- Common use: get the single top spender per country
WITH ranked AS (
    SELECT *, RANK() OVER (PARTITION BY country ORDER BY total_spend DESC) AS rnk
    FROM user_spend_summary
)
SELECT * FROM ranked WHERE rnk = 1;
```

### Offset Functions

```sql
SELECT
    user_id,
    transaction_date,
    amount,

    -- LAG: value from N rows before the current row
    LAG(amount, 1, 0) OVER (PARTITION BY user_id ORDER BY transaction_date) AS prev_amount,
    -- third arg = default if no prior row exists

    -- LEAD: value from N rows after the current row
    LEAD(amount, 1)   OVER (PARTITION BY user_id ORDER BY transaction_date) AS next_amount,

    -- Change from previous transaction
    amount - LAG(amount, 1) OVER (PARTITION BY user_id ORDER BY transaction_date) AS delta,

    -- FIRST_VALUE / LAST_VALUE: first or last value in the window
    FIRST_VALUE(amount) OVER (PARTITION BY user_id ORDER BY transaction_date) AS first_txn_amount,
    LAST_VALUE(amount)  OVER (
        PARTITION BY user_id ORDER BY transaction_date
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING  -- needed for LAST_VALUE
    ) AS last_txn_amount

FROM transactions;
```

### Aggregate Window Functions

```sql
SELECT
    user_id,
    transaction_date,
    amount,

    -- Running total
    SUM(amount)   OVER (PARTITION BY user_id ORDER BY transaction_date) AS running_total,

    -- Running count
    COUNT(*)      OVER (PARTITION BY user_id ORDER BY transaction_date) AS running_count,

    -- Running average
    AVG(amount)   OVER (PARTITION BY user_id ORDER BY transaction_date) AS running_avg,

    -- Total for the user's entire history (no ORDER BY = full partition)
    SUM(amount)   OVER (PARTITION BY user_id) AS lifetime_total,

    -- User's share of their country's total
    SUM(amount)   OVER (PARTITION BY user_id) /
    SUM(amount)   OVER ()                     AS share_of_all_users

FROM transactions;
```

### Frame Specification

Controls which rows are included in the window relative to the current row:

```sql
-- Rolling 7-day sum (includes current day + 6 preceding days)
SUM(amount) OVER (
    PARTITION BY user_id
    ORDER BY transaction_date
    ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
)

-- Rolling 30-day average using date range instead of row count
-- (handles gaps — days with no transactions are skipped)
AVG(amount) OVER (
    PARTITION BY user_id
    ORDER BY UNIX_DATE(transaction_date)
    RANGE BETWEEN 29 PRECEDING AND CURRENT ROW
    -- RANGE uses the ORDER BY value; ROWS uses physical row positions
)

-- Expanding window: everything up to and including the current row
SUM(amount) OVER (
    PARTITION BY user_id
    ORDER BY transaction_date
    ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
)
```

---

## Common Table Expressions (CTEs)

### Basic CTE

```sql
-- CTEs improve readability by naming intermediate results
WITH active_users AS (
    SELECT DISTINCT user_id
    FROM transactions
    WHERE transaction_date >= DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY)
),
user_spend AS (
    SELECT user_id, SUM(amount) AS total_spend
    FROM transactions
    GROUP BY user_id
)
SELECT
    a.user_id,
    COALESCE(s.total_spend, 0) AS spend_last_30_days
FROM active_users a
LEFT JOIN user_spend s ON a.user_id = s.user_id;
```

### Chained CTEs

```sql
-- Each CTE can reference earlier ones in the same WITH clause
WITH raw_data AS (
    SELECT * FROM `project.raw.transactions`
    WHERE status = 'completed'
),
cleaned AS (
    SELECT
        transaction_id,
        user_id,
        SAFE_CAST(amount AS NUMERIC) AS amount,
        DATE(transaction_ts)          AS txn_date
    FROM raw_data
    WHERE amount IS NOT NULL AND amount > 0
),
aggregated AS (
    SELECT
        user_id,
        txn_date,
        COUNT(*)      AS daily_txn_count,
        SUM(amount)   AS daily_spend
    FROM cleaned
    GROUP BY 1, 2
),
with_running_total AS (
    SELECT
        *,
        SUM(daily_spend) OVER (PARTITION BY user_id ORDER BY txn_date) AS cumulative_spend
    FROM aggregated
)
SELECT * FROM with_running_total
WHERE cumulative_spend > 1000
ORDER BY user_id, txn_date;
```

### Recursive CTEs

```sql
-- Traverse a hierarchy (org chart, referral tree, folder structure)
WITH RECURSIVE org_tree AS (
    -- Anchor: top-level nodes (no parent)
    SELECT
        employee_id,
        name,
        manager_id,
        0           AS depth,
        name        AS path
    FROM employees
    WHERE manager_id IS NULL

    UNION ALL

    -- Recursive: join each employee to their manager's row
    SELECT
        e.employee_id,
        e.name,
        e.manager_id,
        t.depth + 1,
        CONCAT(t.path, ' → ', e.name)
    FROM employees e
    JOIN org_tree t ON e.manager_id = t.employee_id
    WHERE t.depth < 20   -- safety: prevent infinite recursion
)
SELECT employee_id, name, depth, path
FROM org_tree
ORDER BY path;
```

---

## Complex Aggregation Patterns

### Conditional Aggregation

```sql
-- Pivot: count transactions by status using CASE inside aggregate
SELECT
    user_id,
    COUNT(*)                                          AS total_txns,
    COUNT(CASE WHEN status = 'completed' THEN 1 END)  AS completed,
    COUNT(CASE WHEN status = 'failed'    THEN 1 END)  AS failed,
    COUNT(CASE WHEN status = 'pending'   THEN 1 END)  AS pending,
    SUM(CASE WHEN status = 'completed' THEN amount ELSE 0 END) AS completed_volume,
    SAFE_DIVIDE(
        COUNT(CASE WHEN status = 'completed' THEN 1 END),
        COUNT(*)
    ) AS success_rate
FROM transactions
GROUP BY user_id;
```

### Month-over-Month Growth

```sql
WITH monthly AS (
    SELECT
        DATE_TRUNC(transaction_date, MONTH) AS month,
        SUM(amount)                         AS revenue
    FROM transactions
    GROUP BY 1
),
with_prev AS (
    SELECT
        month,
        revenue,
        LAG(revenue) OVER (ORDER BY month) AS prev_month_revenue
    FROM monthly
)
SELECT
    month,
    revenue,
    prev_month_revenue,
    ROUND(
        100.0 * (revenue - prev_month_revenue) / NULLIF(prev_month_revenue, 0),
        2
    ) AS mom_growth_pct
FROM with_prev
ORDER BY month;
```

### Consecutive Days Pattern

Find users who made a purchase on 3 or more consecutive days:

```sql
-- Technique: subtract ROW_NUMBER from the date — consecutive dates get the same "group" value
WITH daily_activity AS (
    SELECT
        user_id,
        DATE(transaction_date) AS txn_date
    FROM transactions
    GROUP BY 1, 2  -- deduplicate to one row per user per day
),
with_group AS (
    SELECT
        user_id,
        txn_date,
        DATE_SUB(
            txn_date,
            INTERVAL ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY txn_date) DAY
        ) AS grp   -- same value for consecutive dates
    FROM daily_activity
),
streaks AS (
    SELECT
        user_id,
        grp,
        MIN(txn_date) AS streak_start,
        MAX(txn_date) AS streak_end,
        COUNT(*)      AS consecutive_days
    FROM with_group
    GROUP BY 1, 2
)
SELECT user_id, streak_start, streak_end, consecutive_days
FROM streaks
WHERE consecutive_days >= 3
ORDER BY consecutive_days DESC;
```

### Session Gap Analysis

Group events into sessions where each session ends when there's a gap of more than 30 minutes:

```sql
WITH with_prev_ts AS (
    SELECT
        user_id,
        event_time,
        LAG(event_time) OVER (PARTITION BY user_id ORDER BY event_time) AS prev_event_time
    FROM user_events
),
with_session_flag AS (
    SELECT
        *,
        -- New session if gap > 30 minutes or first event
        CASE
            WHEN prev_event_time IS NULL
              OR TIMESTAMP_DIFF(event_time, prev_event_time, MINUTE) > 30
            THEN 1 ELSE 0
        END AS is_new_session
    FROM with_prev_ts
),
with_session_id AS (
    SELECT
        *,
        SUM(is_new_session) OVER (PARTITION BY user_id ORDER BY event_time) AS session_id
    FROM with_session_flag
)
SELECT
    user_id,
    session_id,
    MIN(event_time) AS session_start,
    MAX(event_time) AS session_end,
    COUNT(*)        AS events_in_session,
    TIMESTAMP_DIFF(MAX(event_time), MIN(event_time), MINUTE) AS session_duration_min
FROM with_session_id
GROUP BY 1, 2
ORDER BY user_id, session_id;
```

### Top-N per Group

```sql
-- Top 3 transactions by amount per user
WITH ranked AS (
    SELECT
        *,
        ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY amount DESC) AS rn
    FROM transactions
)
SELECT * FROM ranked WHERE rn <= 3;

-- Alternative using ARRAY_AGG (better in BigQuery for large tables)
SELECT
    user_id,
    top_transactions
FROM (
    SELECT
        user_id,
        ARRAY_AGG(
            STRUCT(transaction_id, amount, transaction_date)
            ORDER BY amount DESC
            LIMIT 3
        ) AS top_transactions
    FROM transactions
    GROUP BY user_id
);
```

### Deduplication

```sql
-- Keep the most recent row per transaction_id (e.g. after multiple loads)
WITH ranked AS (
    SELECT
        *,
        ROW_NUMBER() OVER (PARTITION BY transaction_id ORDER BY updated_at DESC) AS rn
    FROM `project.raw.transactions`
)
SELECT * EXCEPT(rn) FROM ranked WHERE rn = 1;

-- BigQuery: MERGE for upsert deduplication
MERGE `project.clean.transactions` AS target
USING (
    SELECT * EXCEPT(rn)
    FROM (
        SELECT *, ROW_NUMBER() OVER (PARTITION BY transaction_id ORDER BY updated_at DESC) AS rn
        FROM `project.raw.transactions`
    )
    WHERE rn = 1
) AS source
ON target.transaction_id = source.transaction_id
WHEN MATCHED THEN
    UPDATE SET amount = source.amount, status = source.status, updated_at = source.updated_at
WHEN NOT MATCHED THEN
    INSERT ROW;
```

---

## Set Operations

```sql
-- UNION ALL: combine results including duplicates (fastest)
SELECT user_id FROM table_a
UNION ALL
SELECT user_id FROM table_b;

-- UNION: combine and deduplicate (slower — runs DISTINCT)
SELECT user_id FROM table_a
UNION DISTINCT
SELECT user_id FROM table_b;

-- INTERSECT: rows in both tables
SELECT user_id FROM table_a
INTERSECT DISTINCT
SELECT user_id FROM table_b;

-- EXCEPT: rows in table_a that are NOT in table_b
SELECT user_id FROM table_a
EXCEPT DISTINCT
SELECT user_id FROM table_b;
```

---

## JOIN Patterns

### Self Join

```sql
-- Find pairs of transactions from the same user within 1 minute of each other
-- (potential duplicate detection)
SELECT
    a.transaction_id AS txn1,
    b.transaction_id AS txn2,
    a.user_id,
    a.amount,
    TIMESTAMP_DIFF(b.event_time, a.event_time, SECOND) AS seconds_apart
FROM transactions a
JOIN transactions b
    ON  a.user_id = b.user_id
    AND a.transaction_id < b.transaction_id   -- avoid (a,b) and (b,a) duplicates
    AND ABS(TIMESTAMP_DIFF(b.event_time, a.event_time, SECOND)) < 60;
```

### Anti Join

```sql
-- Find users who have NEVER made a transaction
-- Option 1: NOT EXISTS (readable)
SELECT u.user_id, u.email
FROM users u
WHERE NOT EXISTS (
    SELECT 1 FROM transactions t WHERE t.user_id = u.user_id
);

-- Option 2: LEFT JOIN + IS NULL (equivalent, sometimes faster)
SELECT u.user_id, u.email
FROM users u
LEFT JOIN transactions t ON u.user_id = t.user_id
WHERE t.user_id IS NULL;

-- Option 3: EXCEPT (when you only need the key column)
SELECT user_id FROM users
EXCEPT DISTINCT
SELECT user_id FROM transactions;
```

### Non-equi Join

```sql
-- Match each transaction to the fee tier that was active at transaction time
SELECT
    t.transaction_id,
    t.amount,
    f.fee_rate,
    t.amount * f.fee_rate AS fee_charged
FROM transactions t
JOIN fee_tiers f
    ON  t.transaction_date BETWEEN f.effective_from AND COALESCE(f.effective_to, CURRENT_DATE())
    AND t.amount BETWEEN f.min_amount AND f.max_amount;
```

---

## Data Quality Checks in SQL

```sql
-- 1. Null checks on required fields
SELECT 'null_user_id' AS check_name, COUNT(*) AS failures
FROM transactions
WHERE user_id IS NULL

UNION ALL

-- 2. Duplicate primary keys
SELECT 'duplicate_transaction_id', COUNT(*) - COUNT(DISTINCT transaction_id)
FROM transactions

UNION ALL

-- 3. Referential integrity: transactions without a matching user
SELECT 'orphaned_transactions', COUNT(*)
FROM transactions t
WHERE NOT EXISTS (SELECT 1 FROM users u WHERE u.user_id = t.user_id)

UNION ALL

-- 4. Out-of-range values
SELECT 'negative_amounts', COUNT(*)
FROM transactions
WHERE amount < 0

UNION ALL

-- 5. Future dates
SELECT 'future_transaction_dates', COUNT(*)
FROM transactions
WHERE transaction_date > CURRENT_DATE()

UNION ALL

-- 6. Volume anomaly: today's row count vs 7-day average
SELECT
    'low_volume_today',
    CASE
        WHEN today_count < 0.5 * avg_7day THEN 1
        ELSE 0
    END AS failures
FROM (
    SELECT
        COUNTIF(DATE(transaction_date) = CURRENT_DATE()) AS today_count,
        AVG(daily_count) AS avg_7day
    FROM (
        SELECT DATE(transaction_date) AS d, COUNT(*) AS daily_count
        FROM transactions
        WHERE transaction_date >= DATE_SUB(CURRENT_DATE(), INTERVAL 8 DAY)
          AND transaction_date < CURRENT_DATE()
        GROUP BY 1
    )
);
```

---

## Performance Patterns

```sql
-- 1. Filter early — push filters as deep as possible
-- BAD: filter after join
SELECT * FROM transactions t JOIN users u ON t.user_id = u.user_id
WHERE t.transaction_date = '2024-01-15';

-- GOOD: filter before join (reduces join input size)
SELECT * FROM (
    SELECT * FROM transactions WHERE transaction_date = '2024-01-15'
) t
JOIN users u ON t.user_id = u.user_id;

-- 2. Avoid functions on JOIN / WHERE columns — prevents predicate pushdown
-- BAD: LOWER() in JOIN prevents index/cluster use
JOIN users u ON LOWER(t.user_id) = LOWER(u.user_id)
-- GOOD: standardise at load time, join on clean columns

-- 3. Use EXISTS instead of IN for large subqueries
-- IN: evaluates the full subquery result as a list in memory
-- EXISTS: short-circuits as soon as a match is found
SELECT * FROM users u
WHERE EXISTS (SELECT 1 FROM transactions t WHERE t.user_id = u.user_id AND t.amount > 1000);

-- 4. Avoid SELECT * — specify columns
-- SELECT * reads all columns from storage even if only 2 are needed

-- 5. Materialise CTEs that are used multiple times
-- CTEs are re-evaluated each time they are referenced
CREATE TEMP TABLE expensive_cte AS
SELECT user_id, SUM(amount) AS total FROM transactions GROUP BY 1;
-- Then reference expensive_cte twice without re-running the aggregation
```

---

## Common Gotchas

| Gotcha | Explanation | Fix |
|---|---|---|
| `COUNT(DISTINCT col)` on large tables | Exact distinct count is expensive | Use `APPROX_COUNT_DISTINCT(col)` if ~1% error is acceptable |
| `LAST_VALUE` returning unexpected results | Default frame is `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` | Add `ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING` |
| `NULL` in aggregates | `SUM`, `AVG` ignore NULLs; `COUNT(col)` ignores NULLs but `COUNT(*)` counts all rows | Use `COALESCE(col, 0)` before aggregating if you need nulls treated as 0 |
| Division by zero | `amount / txn_count` when `txn_count = 0` raises an error | Use `SAFE_DIVIDE(amount, txn_count)` in BigQuery or add `NULLIF(txn_count, 0)` |
| `UNION` vs `UNION ALL` | `UNION` deduplicates (slow); `UNION ALL` does not | Use `UNION ALL` unless deduplication is specifically required |
| Joining on nullable columns | `NULL = NULL` is `NULL` (not `TRUE`) — NULLs never match in joins | Use `COALESCE(col, 'UNKNOWN')` or handle NULLs before joining |
| CTE re-execution | A CTE referenced twice is executed twice, not cached | Materialise as a temp table if the CTE is expensive |
| Mixing `ROWS` and `RANGE` frames | `RANGE` requires ORDER BY on a numeric or date type; gaps affect results | Use `ROWS` unless you specifically need range-based windowing |

---

## Resources

- [BigQuery Standard SQL Reference](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax)
- [Mode SQL Tutorial — Window Functions](https://mode.com/sql-tutorial/sql-window-functions/)
- [LeetCode — SQL Problems](https://leetcode.com/problemset/database/)
- [DataLemur — Data Engineering SQL](https://datalemur.com/)
- **Related notes:**
  - [Data Engineering Index](data_engineer.md)
  - [BigQuery — SQL Features and Optimisation](bigquery.md)
  - [Data Modelling — Fact and Dimension Design](data_modelling.md)
