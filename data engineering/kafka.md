# Apache Kafka & GCP Pub/Sub

Kafka is a distributed, durable message queue (also called an event streaming platform). It decouples producers that generate events from consumers that process them — producers write to a topic, consumers read from it independently and at their own pace. Messages are persisted on disk, so consumers can replay them even if they were offline.

```
Producer  →  [Topic: payment_events]  →  Consumer A (fraud detection)
                                       →  Consumer B (analytics aggregation)
                                       →  Consumer C (audit log)
```

GCP Pub/Sub is Google Cloud's fully managed alternative — same pattern, no cluster to operate.

## Table of contents

- [Kafka Core Concepts](#kafka-core-concepts)
  - [Topics and Partitions](#topics-and-partitions)
  - [Producers and Consumers](#producers-and-consumers)
  - [Consumer Groups](#consumer-groups)
  - [Offsets and Replay](#offsets-and-replay)
  - [Delivery Semantics](#delivery-semantics)
- [PySpark Kafka Integration](#pyspark-kafka-integration)
- [Python Kafka Client](#python-kafka-client)
  - [Producer](#producer)
  - [Consumer](#consumer)
- [GCP Pub/Sub](#gcp-pubsub)
  - [Publishing Messages](#publishing-messages)
  - [Subscribing and Processing](#subscribing-and-processing)
  - [Pull vs Push Subscriptions](#pull-vs-push-subscriptions)
- [Kafka vs Pub/Sub Comparison](#kafka-vs-pubsub-comparison)
- [Kafka vs Redis Pub/Sub](#kafka-vs-redis-pubsub)
- [Common Patterns](#common-patterns)
  - [Dead Letter Queue](#dead-letter-queue)
  - [Fan-out Pattern](#fan-out-pattern)
  - [Event Sourcing](#event-sourcing)
- [Common Gotchas](#common-gotchas)
- [Resources](#resources)

---

## Kafka Core Concepts

### Topics and Partitions

A **topic** is a named category for messages (like a table in a database). Each topic is split into **partitions** — ordered, append-only logs stored on disk.

```
Topic: "payment_transactions"
├── Partition 0: [msg1, msg4, msg7, msg10...]
├── Partition 1: [msg2, msg5, msg8, msg11...]
└── Partition 2: [msg3, msg6, msg9, msg12...]
```

- Messages within a partition are **strictly ordered** — offset 0 always before offset 1
- Across partitions there is **no ordering guarantee**
- More partitions = more parallelism (but also more overhead)
- The **replication factor** controls how many broker copies exist for fault tolerance

### Producers and Consumers

**Producers** write messages to a topic. They choose which partition using:
- Round-robin (default, when no key)
- Hash of message key — all messages with the same key always go to the same partition (guarantees per-key ordering)

**Consumers** read messages from a topic. They track their position using **offsets**.

### Consumer Groups

A **consumer group** is a set of consumers that share the work of reading a topic. Each partition is assigned to exactly **one** consumer within the group at a time.

```
Topic: "payment_transactions" (3 partitions)
Consumer Group: "fraud-detection-service"
├── Consumer A  →  Partition 0
├── Consumer B  →  Partition 1
└── Consumer C  →  Partition 2

Add a 4th consumer? → it sits idle (more consumers than partitions = some idle)
Remove Consumer B?  → Kafka rebalances: A gets P0+P1, C gets P2
```

> **Scaling rule:** to increase parallelism, increase the number of partitions. Then you can add consumer instances to match.

### Offsets and Replay

An **offset** is a message's sequential position number within a partition. Kafka stores the last committed offset per consumer group — if a consumer crashes, it resumes from where it left off.

```
Partition 0: [offset 0][offset 1][offset 2][offset 3]...
                                              ↑
                              Consumer last read here → resumes at offset 3
```

**Replay** — because messages are stored on disk (not deleted after reading), you can:
- Reset a consumer group's offset back to the beginning
- Reprocess all historical data
- Run a new consumer group from the start without affecting existing ones

Message retention is controlled by `log.retention.hours` (default: 168 hours / 7 days).

### Delivery Semantics

| Semantic | Meaning | Risk |
|---|---|---|
| **At most once** | Message delivered ≤1 time — may be lost | Data loss on failure |
| **At least once** | Message delivered ≥1 time — may be duplicated | Requires idempotent processing |
| **Exactly once** | Message delivered exactly 1 time | Requires Kafka transactions + idempotent producer config |

```python
# Exactly-once producer config
producer = KafkaProducer(
    bootstrap_servers=["broker:9092"],
    enable_idempotence=True,           # dedup at broker level
    acks="all",                        # wait for all replicas to confirm
    retries=10,
    max_in_flight_requests_per_connection=5
)
```

---

## PySpark Kafka Integration

Reading a Kafka topic as a streaming DataFrame:

```python
from pyspark.sql import SparkSession
from pyspark.sql import functions as F
from pyspark.sql.types import StructType, StringType, DoubleType

spark = SparkSession.builder \
    .appName("KafkaStreamProcessor") \
    .getOrCreate()

# Read from Kafka as a streaming DataFrame
raw_stream = spark.readStream \
    .format("kafka") \
    .option("kafka.bootstrap.servers", "broker1:9092,broker2:9092") \
    .option("subscribe", "payment_transactions") \
    .option("startingOffsets", "latest") \
    .load()

# Kafka gives you: key, value, topic, partition, offset, timestamp
# value is binary — deserialize it
schema = StructType() \
    .add("user_id", StringType()) \
    .add("amount", DoubleType()) \
    .add("currency", StringType())

parsed = raw_stream.select(
    F.from_json(F.col("value").cast("string"), schema).alias("data"),
    F.col("timestamp")
).select("data.*", "timestamp")

# Apply transformations
enriched = parsed.filter(F.col("amount") > 0) \
                 .withColumn("hour", F.hour("timestamp"))

# Write results back to another Kafka topic
enriched.select(
    F.to_json(F.struct("*")).alias("value")
).writeStream \
  .format("kafka") \
  .option("kafka.bootstrap.servers", "broker1:9092") \
  .option("topic", "payment_transactions_enriched") \
  .option("checkpointLocation", "gs://bucket/checkpoints/") \
  .start()

# Or write to a sink (BigQuery, Parquet, console for testing)
query = enriched.writeStream \
    .outputMode("append") \
    .format("console") \
    .start()

query.awaitTermination()
```

---

## Python Kafka Client

Install: `pip install kafka-python`

### Producer

```python
from kafka import KafkaProducer
import json

producer = KafkaProducer(
    bootstrap_servers=["broker1:9092", "broker2:9092"],
    value_serializer=lambda v: json.dumps(v).encode("utf-8"),
    key_serializer=lambda k: k.encode("utf-8") if k else None,
    acks="all",        # wait for all in-sync replicas
    retries=3
)

# Send with a key — all messages with the same key go to the same partition
# This guarantees ordering per user_id
event = {"user_id": "u123", "amount": 500.0, "currency": "USD"}
producer.send(
    topic="payment_transactions",
    key="u123",         # partition key
    value=event
)

# Flush ensures all buffered messages are sent before the script exits
producer.flush()
producer.close()
```

### Consumer

```python
from kafka import KafkaConsumer
import json

consumer = KafkaConsumer(
    "payment_transactions",
    bootstrap_servers=["broker1:9092", "broker2:9092"],
    group_id="fraud-detection-service",  # consumer group ID
    auto_offset_reset="latest",          # start from newest message on first run
    enable_auto_commit=True,             # auto-commit offsets every 5 seconds
    value_deserializer=lambda x: json.loads(x.decode("utf-8"))
)

for message in consumer:
    event = message.value
    # message.partition, message.offset, message.key are also available
    print(f"Partition {message.partition}, Offset {message.offset}")
    print(f"User: {event['user_id']}, Amount: {event['amount']}")
    # process event...
```

---

## GCP Pub/Sub

Install: `pip install google-cloud-pubsub`

### Publishing Messages

```python
from google.cloud import pubsub_v1
import json

publisher = pubsub_v1.PublisherClient()
topic_path = publisher.topic_path("my-gcp-project", "payment_transactions")

def publish_event(event: dict) -> str:
    data = json.dumps(event).encode("utf-8")
    # Attributes are key-value metadata for server-side filtering
    future = publisher.publish(
        topic_path,
        data,
        user_id=event["user_id"],      # attribute — filterable
        currency=event["currency"]     # attribute — filterable
    )
    return future.result()  # blocks until broker confirms receipt

event_id = publish_event({"user_id": "u123", "amount": 500.0, "currency": "USD"})
print(f"Published with message ID: {event_id}")
```

### Subscribing and Processing

```python
from google.cloud import pubsub_v1
import json

subscriber = pubsub_v1.SubscriberClient()
subscription_path = subscriber.subscription_path("my-gcp-project", "fraud-detection-sub")

def process_message(message):
    try:
        event = json.loads(message.data.decode("utf-8"))
        print(f"Processing user {event['user_id']}, amount {event['amount']}")
        # ... fraud scoring logic ...
        message.ack()   # acknowledge: tells Pub/Sub this message was processed
    except Exception as e:
        print(f"Error: {e}")
        message.nack()  # negative-ack: Pub/Sub will redeliver after ack_deadline

streaming_pull = subscriber.subscribe(subscription_path, callback=process_message)

print("Listening for messages...")
try:
    streaming_pull.result(timeout=60)  # block for 60 seconds
except Exception:
    streaming_pull.cancel()
    streaming_pull.result()
```

### Pull vs Push Subscriptions

| Mode | How it works | Best for |
|---|---|---|
| **Pull** | Your consumer calls Pub/Sub to fetch messages | Long-running workers, Spark streaming, batch consumers |
| **Push** | Pub/Sub sends HTTP POST to your endpoint (webhook) | Cloud Run, Cloud Functions, serverless endpoints |

---

## Kafka vs Pub/Sub Comparison

| Feature | Apache Kafka | GCP Pub/Sub |
|---|---|---|
| **Operations** | Self-managed cluster (or Confluent Cloud) | Fully managed by Google |
| **Message retention** | Configurable — days, weeks, forever | Max 7 days |
| **Replay** | Yes — seek to any offset | Limited (within retention window, with Seek API) |
| **Ordering** | Strict per partition | Best-effort (ordering keys available in Pub/Sub Lite) |
| **Throughput** | Extremely high (millions/sec) | Very high (millions/sec) |
| **Latency** | Low (milliseconds) | Low (milliseconds) |
| **Ecosystem** | Kafka Connect, Kafka Streams, ksqlDB | Dataflow, Cloud Functions, Cloud Run |
| **Cost model** | Infrastructure cost (cluster) | Pay-per-message |
| **Best for** | High-throughput, on-prem or hybrid, long retention | GCP-native, serverless, event-driven architectures |

---

## Kafka vs Redis Pub/Sub

| | Kafka | Redis Pub/Sub |
|---|---|---|
| **Persistence** | Messages stored on disk | Fire-and-forget — messages lost if consumer is offline |
| **Replay** | Yes | No |
| **Consumer groups** | Yes — multiple independent groups | No native group concept |
| **Use case** | Data pipelines, event sourcing, audit logs | Low-latency real-time notifications within a system |

> **Rule of thumb:** use Redis pub/sub for ephemeral, low-latency signals (cache invalidation, live dashboards). Use Kafka/Pub/Sub for anything that must be durable, replayable, or consumed by multiple independent services.

---

## Common Patterns

### Dead Letter Queue

Messages that fail processing are routed to a separate DLQ topic for inspection and replay:

```python
from kafka import KafkaProducer, KafkaConsumer
import json

dlq_producer = KafkaProducer(
    bootstrap_servers=["broker:9092"],
    value_serializer=lambda v: json.dumps(v).encode("utf-8")
)

consumer = KafkaConsumer(
    "payment_transactions",
    bootstrap_servers=["broker:9092"],
    group_id="payment-processor",
    value_deserializer=lambda x: json.loads(x.decode("utf-8"))
)

MAX_RETRIES = 3

for message in consumer:
    event = message.value
    retry_count = event.get("_retry_count", 0)

    try:
        # attempt processing
        process(event)
    except Exception as e:
        if retry_count < MAX_RETRIES:
            event["_retry_count"] = retry_count + 1
            event["_last_error"] = str(e)
            dlq_producer.send("payment_transactions_retry", value=event)
        else:
            event["_failed_permanently"] = True
            dlq_producer.send("payment_transactions_dlq", value=event)
```

### Fan-out Pattern

One producer, multiple independent consumer groups — each reads the full stream independently:

```
Topic: "raw_transactions"
├── Consumer Group A: "fraud-detection"     → writes to BigQuery fraud_events
├── Consumer Group B: "analytics-pipeline"  → writes to data warehouse
└── Consumer Group C: "audit-log-service"   → writes to compliance storage
```

Each group maintains its own offset — adding Consumer Group C has zero impact on A or B.

### Event Sourcing

Store every state change as an immutable event. Replay the event log to reconstruct state at any point in time:

```python
# Instead of: UPDATE users SET balance = 900 WHERE id = 'u123'
# Do this:
event = {
    "event_type": "PAYMENT_DEBITED",
    "user_id": "u123",
    "amount": 100.0,
    "timestamp": "2024-01-15T10:30:00Z",
    "balance_after": 900.0
}
producer.send("user_ledger", key="u123", value=json.dumps(event).encode())
# Replay all events for u123 → reconstruct full balance history
```

---

## Common Gotchas

| Gotcha | Explanation | Fix |
|---|---|---|
| Consumer lag growing | Consumers can't keep up with producer rate | Scale consumer instances to match partition count |
| Rebalance storms | Too many consumers joining/leaving rapidly | Tune `session.timeout.ms` and `heartbeat.interval.ms` |
| Message ordering lost | Publishing to multiple partitions without a key | Set a partition key for entities that need ordered processing |
| Large messages | Kafka default max message size is 1 MB | Use a pointer pattern — store the payload in object storage, send the reference in Kafka |
| Uncommitted offsets on crash | Auto-commit sends offsets before processing finishes | Use `enable_auto_commit=False` and commit manually after successful processing |
| Schema drift | Producer changes message structure; consumers break | Use Schema Registry (Confluent) or Pub/Sub schemas |

---

## Resources

- [Apache Kafka Documentation](https://kafka.apache.org/documentation/)
- [GCP Pub/Sub Documentation](https://cloud.google.com/pubsub/docs)
- [kafka-python Documentation](https://kafka-python.readthedocs.io/)
- **Related notes:**
  - [Data Engineering Index](data_engineer.md)
  - [Apache Spark — Structured Streaming with Kafka](spark.md)
  - [Data Engineering System Design](system_design.md)
