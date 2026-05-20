# Apache Spark

Spark is a distributed computing engine for processing large datasets across a cluster of machines. Instead of one machine processing billions of rows sequentially, Spark splits data across many nodes and processes partitions in parallel — then combines results.

```
Single machine:  1 node  × 10 min  = 10 min
Spark cluster:   100 nodes × 6 sec  = 6 sec   ← same work, much faster
```

## Table of contents

- [Core Concepts](#core-concepts)
  - [RDD vs DataFrame vs Dataset](#rdd-vs-dataframe-vs-dataset)
  - [Transformations vs Actions](#transformations-vs-actions)
  - [DAG and the Catalyst Optimizer](#dag-and-the-catalyst-optimizer)
- [PySpark — Key Operations](#pyspark--key-operations)
  - [SparkSession](#sparksession)
  - [Reading Data](#reading-data)
  - [Select, Filter, Rename](#select-filter-rename)
  - [Derived Columns](#derived-columns)
  - [Aggregations](#aggregations)
  - [Joins](#joins)
  - [Window Functions](#window-functions)
  - [Writing Data](#writing-data)
- [Performance Tuning](#performance-tuning)
  - [Partitioning](#partitioning)
  - [Broadcast Join](#broadcast-join)
  - [Data Skew](#data-skew)
  - [Caching](#caching)
- [Spark Architecture](#spark-architecture)
- [Common Gotchas](#common-gotchas)
- [Resources](#resources)

---

## Core Concepts

### RDD vs DataFrame vs Dataset

| API | Description | When to use |
|---|---|---|
| **RDD** | Resilient Distributed Dataset — the original low-level API. A distributed collection with no schema. | Legacy code only; avoid in new projects |
| **DataFrame** | RDD + schema (column names and types). The primary Python/PySpark API. Like a distributed pandas DataFrame. | Always — this is the standard |
| **Dataset** | DataFrame + compile-time type safety. Java/Scala only. | Not available in Python |

> **In Python: always use DataFrame. RDD is only needed for very low-level operations.**

### Transformations vs Actions

This is the most important Spark concept.

**Transformations** are *lazy* — they describe a computation plan but do not run it:

```python
df = spark.read.parquet("gs://bucket/data/")
df_filtered = df.filter(df.amount > 100)            # lazy — nothing runs yet
df_grouped  = df_filtered.groupBy("user_id").sum()  # lazy — still nothing runs
```

**Actions** *trigger* execution — Spark runs the entire accumulated plan:

```python
df_grouped.show()                      # action — Spark executes all pending transforms
df_grouped.count()                     # action
df_grouped.write.parquet("gs://out/")  # action
```

### DAG and the Catalyst Optimizer

When you chain transformations, Spark builds a **DAG** (Directed Acyclic Graph) of operations. The **Catalyst optimizer** then:

- Reorders operations for efficiency (e.g., pushes filters before joins)
- Combines adjacent operations into a single stage where possible
- Generates optimised physical execution plans

You never interact with the optimizer directly — it runs automatically when an action is called.

---

## PySpark — Key Operations

### SparkSession

```python
from pyspark.sql import SparkSession

# Entry point to all Spark functionality
spark = SparkSession.builder \
    .appName("MyDataPipeline") \
    .config("spark.sql.shuffle.partitions", "200") \
    .getOrCreate()
```

### Reading Data

```python
from pyspark.sql import functions as F

# CSV
df = spark.read \
    .option("header", "true") \
    .option("inferSchema", "true") \
    .csv("gs://bucket/data/*.csv")

# Parquet (preferred — columnar, compressed, schema-aware)
df = spark.read.parquet("gs://bucket/data/")

# JSON
df = spark.read.option("multiLine", "true").json("gs://bucket/data/*.json")

# Inspect
df.printSchema()    # column names and types
df.show(5)          # first 5 rows
df.count()          # total row count
```

### Select, Filter, Rename

```python
# Select specific columns
df = df.select("user_id", "amount", "transaction_date")

# Filter rows
df = df.filter(F.col("amount") > 0)
df = df.filter((F.col("status") == "completed") & (F.col("amount") > 100))

# Rename
df = df.withColumnRenamed("txn_id", "transaction_id")

# Drop a column
df = df.drop("internal_flag")
```

### Derived Columns

```python
# Add or overwrite a column
df = df.withColumn("year",  F.year(F.col("transaction_date")))
df = df.withColumn("month", F.month(F.col("transaction_date")))

# Conditional column (like SQL CASE WHEN)
df = df.withColumn(
    "risk_flag",
    F.when(F.col("amount") > 50000, "HIGH")
     .when(F.col("amount") > 10000, "MEDIUM")
     .otherwise("LOW")
)

# Type casting
df = df.withColumn("amount", F.col("amount").cast("double"))
```

### Aggregations

```python
summary = df.groupBy("user_id").agg(
    F.sum("amount").alias("total_amount"),
    F.count("transaction_id").alias("num_txns"),
    F.avg("amount").alias("avg_amount"),
    F.max("transaction_date").alias("last_txn_date"),
    F.min("amount").alias("min_amount")
)

# Multiple group-by keys
by_region = df.groupBy("region", "currency").agg(
    F.sum("amount").alias("regional_volume")
)
```

### Joins

```python
users_df = spark.read.parquet("gs://bucket/users/")

# Left join
result = df.join(users_df, on="user_id", how="left")

# Join on different column names
result = df.join(users_df, df.uid == users_df.user_id, how="inner")

# Join types: "inner", "left", "right", "outer", "left_semi", "left_anti"

# left_anti = rows in df that have NO match in users_df (useful for finding orphaned records)
orphans = df.join(users_df, on="user_id", how="left_anti")
```

### Window Functions

```python
from pyspark.sql.window import Window

# Partition by user, order by date — same as SQL PARTITION BY
window_spec = Window.partitionBy("user_id").orderBy(F.desc("transaction_date"))

# Row number (1 = most recent per user)
df = df.withColumn("rank", F.row_number().over(window_spec))

# Get the single latest transaction per user
latest_per_user = df.filter(F.col("rank") == 1)

# Running total within each user's history
running_window = Window.partitionBy("user_id").orderBy("transaction_date")
df = df.withColumn("running_total", F.sum("amount").over(running_window))

# Lag/lead — compare to previous/next row
df = df.withColumn("prev_amount", F.lag("amount", 1).over(running_window))
df = df.withColumn("next_amount", F.lead("amount", 1).over(running_window))
```

### Writing Data

```python
# Write as Parquet, partitioned by date (efficient for time-series queries)
df.write \
  .mode("overwrite") \
  .partitionBy("year", "month") \
  .parquet("gs://bucket/output/clean_data/")

# Append to existing dataset
df.write.mode("append").parquet("gs://bucket/output/clean_data/")

# Write to BigQuery (requires spark-bigquery connector)
df.write \
  .format("bigquery") \
  .option("table", "my-project.dataset.table_name") \
  .option("temporaryGcsBucket", "my-temp-bucket") \
  .mode("overwrite") \
  .save()

# Write to a single file (small datasets only — coalesce first)
df.coalesce(1).write.mode("overwrite").csv("gs://bucket/output/", header=True)
```

---

## Performance Tuning

### Partitioning

```python
# Check how many partitions the DataFrame currently has
print(df.rdd.getNumPartitions())

# Repartition: redistribute data into N partitions (causes a full shuffle — expensive)
# Use when you need more partitions for parallelism
df = df.repartition(200)

# Repartition by column: all rows with the same key go to the same partition
# Good before groupBy or join on that column
df = df.repartition("user_id")

# Coalesce: reduce partitions WITHOUT a full shuffle (preferred before writing)
# Merges adjacent partitions on the same node
df = df.coalesce(10)

# When writing: partition the files on disk by a column
df.write.partitionBy("transaction_date").parquet("gs://output/")
# Creates: output/transaction_date=2024-01-01/part-00000.parquet, ...
# Queries filtering by transaction_date will skip irrelevant folders entirely
```

> **Rule of thumb:** aim for partition sizes of 100–200 MB. Too few partitions = not enough parallelism. Too many = too much overhead.

### Broadcast Join

When joining a large table with a small lookup table, broadcast the small table to every worker — this avoids a shuffle of the large table entirely.

```python
from pyspark.sql.functions import broadcast

large_df = spark.read.parquet("gs://bucket/transactions/")    # billions of rows
small_df = spark.read.parquet("gs://bucket/currency_codes/")  # tiny lookup table (~1 MB)

# Without broadcast: Spark shuffles BOTH tables — very expensive
# With broadcast: Spark sends small_df to every executor — no shuffle of large_df
result = large_df.join(broadcast(small_df), on="currency_code", how="left")
```

Spark also auto-broadcasts tables smaller than `spark.sql.autoBroadcastJoinThreshold` (default 10 MB).

### Data Skew

Skew occurs when one partition holds far more data than others — one worker does most of the work while others idle.

```python
# Diagnose: check how many rows are in each partition
df.groupBy(F.spark_partition_id()).count().orderBy(F.desc("count")).show()

# Fix option 1: salting — add a random suffix to the join key to spread the load
import random
SALT_BUCKETS = 10

df_salted = df.withColumn("salt", (F.rand() * SALT_BUCKETS).cast("int")) \
              .withColumn("salted_key", F.concat(F.col("user_id"), F.lit("_"), F.col("salt")))

# The small lookup table must be exploded to match all salt values
from pyspark.sql.functions import explode, array, lit
lookup_exploded = small_df.withColumn("salt", explode(array([lit(i) for i in range(SALT_BUCKETS)]))) \
                          .withColumn("salted_key", F.concat(F.col("user_id"), F.lit("_"), F.col("salt")))

result = df_salted.join(lookup_exploded, on="salted_key", how="left")

# Fix option 2: repartition by the skewed column before the join
df = df.repartition(400, "user_id")
```

### Caching

```python
# Cache a DataFrame in memory — avoids recomputing it across multiple actions
df_clean = df.filter(F.col("amount") > 0).withColumn("year", F.year("date"))
df_clean.cache()          # lazy — doesn't cache until an action triggers it
df_clean.count()          # triggers caching

# Use when a DataFrame is used in multiple downstream operations
report1 = df_clean.groupBy("region").sum("amount")
report2 = df_clean.groupBy("user_id").count()

# Release from memory when done
df_clean.unpersist()

# persist() lets you choose storage level (MEMORY_ONLY, MEMORY_AND_DISK, DISK_ONLY)
from pyspark import StorageLevel
df_clean.persist(StorageLevel.MEMORY_AND_DISK)
```

---

## Spark Architecture

```
Driver Program
├── SparkContext / SparkSession  ← coordinates everything
├── DAG Scheduler                ← builds execution plan from transformations
└── Task Scheduler               ← assigns tasks to executors

Cluster Manager (YARN / Kubernetes / Mesos / Standalone)
├── Executor 1  [Task] [Task] [Task]   ← worker nodes that run tasks
├── Executor 2  [Task] [Task] [Task]
└── Executor N  [Task] [Task] [Task]
```

| Term | Meaning |
|---|---|
| **Driver** | The process running your `main()` / notebook — coordinates the job |
| **Executor** | Worker process on a cluster node — runs tasks and stores cached data |
| **Job** | One top-level Spark computation triggered by an action |
| **Stage** | A set of tasks that can run without a shuffle; a job has ≥1 stages |
| **Task** | One unit of work sent to one executor on one partition |
| **Shuffle** | Redistribution of data across executors (triggered by `groupBy`, `join`, `repartition`) — the most expensive operation |

---

## Common Gotchas

| Gotcha | Explanation | Fix |
|---|---|---|
| `count()` in a loop | Each `count()` triggers a full job | Cache the DataFrame first |
| `SELECT *` before a join | Reads all columns even if unused | Select only needed columns first |
| Too few partitions | Only N executors can work at once | Repartition to `2–4× number of cores` |
| Too many `coalesce(1)` | Forces all data to one executor | Only use for final small output files |
| Using Python UDFs | Python UDFs bypass the Catalyst optimizer and are much slower | Use built-in `pyspark.sql.functions` instead; use Pandas UDFs (`@pandas_udf`) if custom logic is needed |
| Calling `.show()` in production | Collects data to driver — can OOM | Use `.write` instead |
| Joining without broadcast on small table | Causes unnecessary shuffle of large table | Always `broadcast()` the small side |

---

## Resources

- [PySpark Documentation](https://spark.apache.org/docs/latest/api/python/)
- [Spark SQL Functions Reference](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/functions.html)
- **Related notes:**
  - [Data Engineering Index](data_engineer.md)
  - [Airflow — Orchestrating Spark Jobs](airflow.md)
  - [BigQuery — Writing Spark Output](bigquery.md)
