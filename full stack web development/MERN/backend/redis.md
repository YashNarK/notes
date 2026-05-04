# Redis — Deep Dive for MERN Stack

---

## Table of Contents

- [What is Redis?](#what-is-redis)
- [Redis as a Cache](#redis-as-a-cache)
  - [Why Use Redis for Caching?](#why-use-redis-for-caching)
  - [Cache-Aside Pattern](#cache-aside-pattern)
  - [Write-Through and Write-Behind Caching](#write-through-and-write-behind-caching)
  - [Eviction Policies](#eviction-policies)
  - [Example: Express + Redis Caching](#example-express--redis-caching)
- [Redis as a Database](#redis-as-a-database)
  - [Data Structures](#data-structures)
  - [Persistence: RDB and AOF](#persistence-rdb-and-aof)
  - [Transactions and Lua Scripting](#transactions-and-lua-scripting)
  - [Example: Storing Session Data](#example-storing-session-data)
- [Redis as a Pub/Sub System](#redis-as-a-pubsub-system)
  - [How Pub/Sub Works](#how-pubsub-works)
  - [Example: Real-Time Notifications](#example-real-time-notifications)
- [Other Redis Use Cases](#other-redis-use-cases)
  - [Rate Limiting](#rate-limiting)
  - [Task Queues](#task-queues)
  - [Leaderboards and Counting](#leaderboards-and-counting)
- [Scaling Redis](#scaling-redis)
  - [Replication](#replication)
  - [Clustering and Sharding](#clustering-and-sharding)
  - [High Availability (Sentinel)](#high-availability-sentinel)
- [Security Best Practices](#security-best-practices)
- [Common Redis Commands](#common-redis-commands)

---

# What is Redis?

**Redis** (REmote DIctionary Server) is an open-source, in-memory data structure store, used as a database, cache, and message broker. It supports strings, hashes, lists, sets, sorted sets, bitmaps, hyperloglogs, and geospatial indexes.

- **Blazing fast**: All data is kept in RAM for ultra-low latency.
- **Versatile**: Can be used for caching, session storage, pub/sub, queues, counters, and more.
- **Persistence**: Can optionally persist data to disk (RDB snapshots, AOF log).
- **Atomic operations**: All commands are atomic; supports transactions and Lua scripting.

---

# Redis as a Cache

## Why Use Redis for Caching?

- **Speed**: In-memory access is orders of magnitude faster than disk or database queries.
- **Offload**: Reduces load on primary databases (MongoDB, PostgreSQL, etc.).
- **Scalability**: Handles high-throughput, low-latency workloads.

## Cache-Aside Pattern (Lazy Loading)

1. **Read**: App checks Redis for data (cache hit). If not found (cache miss), loads from DB, stores in Redis, and returns to client.
2. **Write**: App writes to DB and invalidates/updates the cache.

```javascript
// Example: Express + Redis cache-aside
const redis = require('ioredis')();

async function getUser(userId) {
  const cacheKey = `user:${userId}`;
  let user = await redis.get(cacheKey);
  if (user) return JSON.parse(user);

  user = await User.findById(userId); // MongoDB
  if (user) await redis.set(cacheKey, JSON.stringify(user), 'EX', 3600); // 1 hour TTL
  return user;
}
```

## Write-Through and Write-Behind Caching

- **Write-Through**: Writes go to cache and DB synchronously. Ensures cache is always up-to-date, but slower writes.
- **Write-Behind**: Writes go to cache, then asynchronously flushed to DB. Faster writes, but risk of data loss if Redis crashes before flush.

## Eviction Policies

When Redis memory is full, it evicts keys based on policy:
- `noeviction` (default): Returns error on write if full
- `allkeys-lru`: Least recently used key (any key)
- `volatile-lru`: LRU among keys with TTL
- `allkeys-random`: Random key
- `volatile-ttl`: Key with soonest expiration

Set with `maxmemory-policy` in `redis.conf`.

## Example: Express + Redis Caching Middleware

```javascript
function cacheMiddleware(ttl = 60) {
  return async (req, res, next) => {
    const key = `cache:${req.originalUrl}`;
    const cached = await redis.get(key);
    if (cached) return res.json(JSON.parse(cached));
    const originalJson = res.json.bind(res);
    res.json = async (body) => {
      await redis.set(key, JSON.stringify(body), 'EX', ttl);
      return originalJson(body);
    };
    next();
  };
}

// Usage
app.get('/api/products', cacheMiddleware(300), async (req, res) => {
  const products = await Product.find();
  res.json(products);
});
```

---

# Redis as a Database

## Data Structures

- **String**: `SET key value`, `GET key`
- **Hash**: `HSET user:1 name "Alice" age 30`, `HGETALL user:1`
- **List**: `LPUSH queue task1`, `RPOP queue`
- **Set**: `SADD tags nodejs redis`, `SMEMBERS tags`
- **Sorted Set**: `ZADD leaderboard 100 "alice"`, `ZRANGE leaderboard 0 -1 WITHSCORES`
- **Bitmap**: Efficient for tracking flags (e.g., user logins)
- **HyperLogLog**: Approximate unique counts
- **Geospatial**: Store and query locations

## Persistence: RDB and AOF

- **RDB (Snapshotting)**: Saves DB to disk at intervals. Fast recovery, but may lose recent writes.
- **AOF (Append Only File)**: Logs every write. Slower, but safer (can replay all changes).
- **Hybrid**: Use both for best durability.

## Transactions and Lua Scripting

- **MULTI/EXEC**: Group commands into an atomic transaction.
- **WATCH**: Optimistic locking for concurrency control.
- **Lua scripts**: Run complex logic atomically on the server.

## Example: Storing Session Data

```javascript
// express-session + connect-redis
const session = require('express-session');
const RedisStore = require('connect-redis')(session);

app.use(session({
  store: new RedisStore({ client: redis }),
  secret: 'keyboard cat',
  resave: false,
  saveUninitialized: false,
  cookie: { maxAge: 3600000 }
}));
```

---

# Redis as a Pub/Sub System

## How Pub/Sub Works

- **Publisher**: Sends messages to a channel.
- **Subscriber**: Listens for messages on a channel.
- **Decoupled**: Publishers and subscribers don't know about each other.

## Example: Real-Time Notifications

```javascript
// Publisher
const pub = new Redis();
pub.publish('notifications', JSON.stringify({ userId, message: 'You have a new message!' }));

// Subscriber
const sub = new Redis();
sub.subscribe('notifications');
sub.on('message', (channel, message) => {
  const data = JSON.parse(message);
  // Send to user's websocket, etc.
});
```

- **Use cases**: Chat apps, live feeds, real-time analytics, event-driven microservices.
- **Note**: Redis pub/sub is **fire-and-forget** — if a subscriber is offline, it misses messages. For guaranteed delivery, use Redis Streams or a message queue (RabbitMQ, Kafka).

---

# Other Redis Use Cases

## Rate Limiting

```javascript
// Sliding window rate limiter
async function rateLimiter(req, res, next) {
  const key = `ratelimit:${req.ip}`;
  const now = Date.now();
  await redis.zremrangebyscore(key, 0, now - 60000); // 1 min window
  await redis.zadd(key, now, `${now}`);
  const count = await redis.zcard(key);
  await redis.expire(key, 60);
  if (count > 100) return res.status(429).json({ error: 'Too many requests' });
  next();
}
```

## Task Queues

- Use Redis lists as a simple queue: `LPUSH` to enqueue, `BRPOP` to dequeue (blocking pop for workers).
- Libraries: `bull`, `bee-queue`, `bullmq` for robust job queues with retries, delays, priorities.

## Leaderboards and Counting

- Use **sorted sets** for real-time leaderboards: `ZADD`, `ZRANGE`, `ZREVRANGE`.
- Use **INCR**, **DECR** for atomic counters (page views, likes, etc.).

---

# Scaling Redis

## Replication

- **Master-slave** replication for read scaling and failover.
- Slaves are read-only; writes go to master.

## Clustering and Sharding

- **Redis Cluster**: Automatically shards data across multiple nodes. Handles partitioning, failover, and rebalancing.
- **Client libraries** (e.g., `ioredis`) support cluster mode.

## High Availability (Sentinel)

- **Redis Sentinel**: Monitors Redis servers, handles automatic failover, notifies clients of new master.
- Use with replication for production-grade HA.

---

# Security Best Practices

- Bind Redis to localhost or use a firewall (`bind 127.0.0.1` in `redis.conf`).
- Require authentication (`requirepass` in `redis.conf`).
- Use TLS for encrypted connections.
- Never expose Redis directly to the internet.
- Regularly update Redis to patch vulnerabilities.

---

# Common Redis Commands

| Command | Description |
|---|---|
| `SET key value [EX seconds]` | Set value with optional expiry |
| `GET key` | Get value |
| `DEL key` | Delete key |
| `EXPIRE key seconds` | Set TTL |
| `INCR key` | Increment integer value |
| `LPUSH list value` | Push to list (left) |
| `RPUSH list value` | Push to list (right) |
| `LPOP list` | Pop from list (left) |
| `RPOP list` | Pop from list (right) |
| `SADD set value` | Add to set |
| `SMEMBERS set` | Get all set members |
| `ZADD zset score value` | Add to sorted set |
| `ZRANGE zset start stop [WITHSCORES]` | Range from sorted set |
| `HSET hash field value` | Set hash field |
| `HGETALL hash` | Get all fields/values from hash |
| `PUBLISH channel message` | Publish message |
| `SUBSCRIBE channel` | Subscribe to channel |

---

For more, see: https://redis.io/commands/
