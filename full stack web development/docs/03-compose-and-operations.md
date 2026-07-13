# Docker Master Cheatsheet — Part 3 — Compose & Operations

[← Back to index](../Docker.md)

---

## 10. Docker Compose

### Why Compose Exists

A real app is rarely one container. A typical web app is at least *two*: your code, plus a database. Add a cache (Redis), a queue, a reverse proxy, and suddenly you're juggling five containers — each needing the right network, the right volumes, the right env vars, started in the right order.

Doing that by hand with `docker run` is miserable and error-prone. You have to remember every flag, type them in the correct sequence, and repeat the whole ritual every time you (or a teammate) start the stack:

```bash
# Without Compose — 4 commands, easy to forget flags
docker network create mynetwork
docker run --name db --network mynetwork -v mydata:/var/lib/postgresql/data \
  -e POSTGRES_PASSWORD=secret postgres:15
docker build -t my-app .
docker run --name app --network mynetwork -p 3000:3000 -v .:/app my-app
```

Forget the `--network` flag on one of them and the app can't reach the database. Forget the volume and your data vanishes on restart.

**Docker Compose** fixes this. You describe the *entire* stack once, in a file called `docker-compose.yml`, and then bring the whole thing up with a single command:

```bash
# With Compose — 1 command
docker compose up
```

**Mental model:** if an image is a recipe and a container is the cooked dish, then Compose is the *menu* — it lists every dish for the meal and how they relate, so one command plates the whole thing. The file becomes the single source of truth: check it into git, and any teammate runs `docker compose up` to get an identical environment.

### docker-compose.yml Structure

Everything lives under a few top-level keys. `services` is where each container is defined; `volumes` and `networks` at the bottom *declare* the named resources your services reference.

```yaml
# Version is optional in modern Compose (v2+)
services:                        # define your containers
  service-name:
    image: or build:             # where to get the image
    ports: []                    # port mappings
    volumes: []                  # volume mounts
    environment: {}              # env vars
    depends_on: {}               # startup dependencies
    healthcheck: {}              # health check config
    restart: always              # restart policy
    networks: []                 # which networks to join
volumes:                         # declare named volumes
  mydata:
networks:                        # declare custom networks (optional)
  frontend:
  backend:
```

Two things worth knowing up front:

- **Each key under `services:` is a service name you choose** (`service-name`, `db`, `app`, ...). That name is *also* the container's hostname on the shared network — so `app` can reach the database at `db:5432` with no IP addresses involved. Compose puts every service on one network automatically, so they can talk to each other by name out of the box.
- **`image:` OR `build:`, not both (usually).** Use `image:` to pull a prebuilt image (like `postgres:15`); use `build: .` when Compose should build from a local Dockerfile.

### YAML Syntax: List vs Map

This confuses everyone, and the errors YAML gives you when you get it wrong are cryptic. Here's the one rule that untangles it:

**Hyphens (`-`) = list items** — used when a field accepts multiple values of the same kind, and order/duplication is fine:

```yaml
ports:
  - "3000:3000"
  - "443:443"
volumes:
  - mydata:/var/lib/postgresql/data
  - logs:/app/logs
```

A port mapping isn't "named" — you might have several — so it's a plain list. Each `-` is one more item.

**No hyphens = key-value map** — used when each entry has a unique name that points to a value:

```yaml
environment:
  DB_PASSWORD: secret
  DB_HOST: db
  NODE_ENV: production
```

An env var *has* a name (`DB_PASSWORD`) and a value (`secret`), so it's a map: `key: value`, no hyphens.

Some fields accept **both** styles (environment is the classic one). These are identical in effect — pick whichever reads better:

```yaml
# Map style
environment:
  DB_PASSWORD: secret
# List style (same result)
environment:
  - DB_PASSWORD=secret
```

Notice the punctuation flips: map style uses `KEY: value` (colon-space), list style uses `- KEY=value` (hyphen, equals sign). Mixing them up — a hyphen where a map is expected, or vice versa — is the single most common Compose-file error.

### depends_on Explained

`depends_on` controls **startup order**. Without it, Compose starts everything at once, and your app might come up before the database exists — it tries to connect, gets refused, and crashes.

```yaml
app:
  depends_on:
    - db              # basic: start db before app (container start only)
```

**Critical caveat — read this twice.** In its basic form, `depends_on` waits for the database container to *START*, not for the database *inside* it to be *READY to accept connections*. Those are not the same moment:

```
container started ≠ service is ready
Postgres container starts in ~0.1 seconds
Postgres database initializes in ~3-5 seconds
Your app crashes if it connects in that gap
```

This trips *everyone* up. Compose sees the Postgres container running after a tenth of a second and immediately launches your app — but Postgres is still bootstrapping its data directory and won't answer for another few seconds. Your app connects into that gap, gets "connection refused," and dies. The stack *looks* correctly ordered, yet it fails intermittently, which makes it maddening to debug.

**The fix:** pair `depends_on` with a **health check** (see Section 11) and wait for the *healthy* condition, not just "started." This is the map form of `depends_on` (note the `db:` key and the `condition:` under it):

```yaml
app:
  depends_on:
    db:
      condition: service_healthy   # waits until db passes health check
```

Now Compose won't start `app` until `db`'s health check actually passes — meaning Postgres is genuinely ready. This one change turns a flaky stack into a reliable one.

### Side-by-Side Comparison

| | Without Compose | With Compose |
|---|---|---|
| Extra files | None | docker-compose.yml |
| Commands to start | 4+ | docker compose up |
| Commands to stop | 3+ | docker compose down |
| Network setup | Manual | Automatic |
| Teammate onboarding | Complex | Just docker compose up |
| Config readability | Scattered in terminal | All in one file |

### Key Compose Commands

The mental split: `up`/`down` manage the *whole* stack's lifecycle; the rest inspect or poke at individual services.

```bash
docker compose up              # start all services (foreground)
docker compose up -d           # start all services (background/detached)
docker compose down            # stop and remove containers
docker compose down -v         # also remove volumes (wipes database data!)
docker compose ps              # list running services and status
docker compose logs            # all logs
docker compose logs -f app     # follow logs for one service
docker compose build           # rebuild images
docker compose pull            # pull latest images from registry
docker compose restart app     # restart one service
docker compose exec app sh     # shell into a running container
docker compose run app node    # run a one-off command in a new container
```

Two safety notes worth burning in:

- **`docker compose down -v` deletes your named volumes** — that includes your database data. Great for a clean reset in development; catastrophic if you run it against something you care about. The plain `docker compose down` leaves volumes alone.
- **`exec` vs `run`:** `exec` runs a command *inside an already-running* service container. `run` spins up a *brand-new* one-off container from the same config (handy for migrations or a REPL). If the service isn't up, `exec` has nothing to attach to — use `run`.

### Specifying a Compose File

By default Compose reads `docker-compose.yml`. Use `-f` to point at a different file — the standard way to keep separate dev and prod configurations:

```bash
docker compose -f docker-compose.dev.yml up
docker compose -f docker-compose.prod.yml up -d
```

---

---

## 11. Health Checks

A container can be **running** but **broken**. Docker only knows whether the *process* is alive — it has no idea whether your web server is actually answering requests, or whether it's wedged in a deadlock, stuck reconnecting to a database, or serving 500s on every request. From Docker's point of view "the process hasn't exited" equals "fine," which is often a lie.

A **health check** closes that gap. You give Docker a command to run *inside* the container on a schedule; if the command succeeds, the container is `healthy`, if it keeps failing, it's `unhealthy`. This is exactly the signal `depends_on: condition: service_healthy` (Section 10) waits on, and it's what orchestrators use to decide whether to route traffic to a container or restart it.

### In Dockerfile

Bake the check into the image so it travels with it. Here the check hits an HTTP `/health` endpoint; `|| exit 1` turns any curl failure into the non-zero exit code Docker reads as "unhealthy":

```dockerfile
HEALTHCHECK --interval=30s --timeout=10s --retries=3 --start-period=15s \
  CMD curl -f http://localhost:3000/health || exit 1
```

| Option | Meaning |
|---|---|
| --interval=30s | Run check every 30 seconds |
| --timeout=10s | If check takes > 10s, it fails |
| --retries=3 | Mark unhealthy after 3 consecutive failures |
| --start-period=15s | Grace period on startup before checks count |

The `--start-period` is the one beginners forget. Apps need a moment to boot; without a grace period, the checks that run during startup count as failures and your container can be declared unhealthy before it ever had a chance. During the start period, failing checks are *ignored* — they only start counting toward `--retries` once the app has had time to come up. (And `curl` must exist inside the image — slim/Alpine bases often don't ship it, so either install it or use a language-native check.)

### Container Health States

A container with a health check moves through three states:

```
starting   → booting up, checks don't count yet
healthy    → checks are passing ✅
unhealthy  → checks are failing ❌
```

`starting` is the `--start-period` window. After that, a passing check flips it to `healthy`; enough consecutive failures flip it to `unhealthy`. You'll see this status right in `docker ps` / `docker compose ps` output (e.g. `Up 2 hours (healthy)`).

### In docker-compose.yml

Same idea, expressed in YAML. `test` is the command to run; the rest are the same timing knobs as the Dockerfile flags. `CMD-SHELL` runs the string through a shell (so pipes and `||` work); a plain `CMD` list runs the binary directly. Here `pg_isready` is Postgres's own "am I accepting connections?" tool — ideal for the readiness gate `app` depends on:

```yaml
db:
  image: postgres:15
  healthcheck:
    test: ["CMD-SHELL", "pg_isready -U postgres"]
    interval: 10s
    timeout: 5s
    retries: 5
    start_period: 30s
```

### Health Endpoint in Your App

For a web service, the cleanest check is a dedicated `/health` route. Keep it **lightweight** — it runs every few seconds, so it should return fast and *not* do heavy work like a full database query on every hit (a busy check becomes its own load problem). A simple "the process is up and responding" is usually enough:

```javascript
// Node.js Express — lightweight, no DB call
app.get('/health', (req, res) => {
  res.status(200).json({ status: 'ok', timestamp: Date.now() })
})
```

```python
# FastAPI
@app.get("/health")
def health_check():
    return {"status": "ok"}
```

---

---

## 12. Logs & Debugging

When a container misbehaves, you can't open a debugger on a mystery box — you *observe* it: read what it printed, check its status, then step inside and look around. This section is that toolkit, roughly in the order you'll reach for it.

### View Logs

Docker captures whatever your app writes to stdout/stderr — that's the first place to look. `-f` *follows* the stream live (like `tail -f`); `--tail N` shows only the last N lines so you're not drowning in history; `-t` adds timestamps:

```bash
docker logs <container-name>           # all logs
docker logs -f app                     # follow (stream) logs
docker logs --tail 50 app              # last 50 lines
docker logs -t app                     # with timestamps
docker logs -f --tail 100 -t app       # combined
# With Compose
docker compose logs                    # all services
docker compose logs -f                 # follow all
docker compose logs -f app             # follow one service
docker compose logs -f app db          # follow multiple services
```

The Compose variants are usually what you want in a multi-service stack: `docker compose logs -f` interleaves output from every service with a prefix per line, so you can watch the app and the database react to each other in real time.

### Check Status

Before anything else, confirm *what's actually running* and whether it's healthy:

```bash
docker compose ps
```

Output:

```
NAME    IMAGE           STATUS                   PORTS
app     my-app:1.0      Up 2 hours (healthy)     0.0.0.0:3000->3000/tcp
db      postgres:15     Up 2 hours (healthy)     5432/tcp
```

Read the `STATUS` column carefully. `Up ... (healthy)` is good; `Up ... (unhealthy)` means the process is running but failing its health check (see Section 11); a container missing from the list entirely means it crashed or never started — go straight to its logs.

### Get Inside a Running Container

When logs aren't enough, open a shell *inside* the container and inspect it as if it were a tiny machine. Use `sh` on Alpine images, `bash` on Ubuntu/Debian (Alpine usually has no bash):

```bash
docker exec -it app sh            # sh shell (Alpine)
docker exec -it app bash          # bash shell (Ubuntu/Debian)
docker compose exec app sh        # same, via Compose
```

The `-it` is two flags: `-i` keeps input open, `-t` gives you a proper terminal — together they make the shell interactive. Once inside, you're checking the assumptions your app depends on:

```bash
ls /app                           # check files
env                               # see all environment variables
curl http://db:5432               # test if db is reachable
cat /etc/hosts                    # see hostname mappings
ping db                           # test network connectivity
```

This is how you catch the classic failures: an env var that didn't get set, a config file that isn't where you thought, or a database that the app simply can't reach by name.

### Debugging Workflow

When something's broken, resist the urge to poke randomly. Follow the funnel — from the widest, cheapest view down to the most detailed:

```bash
# 1. Check what's running
docker compose ps
# 2. Check logs for errors
docker compose logs -f
# 3. Get inside the broken container
docker exec -it app sh
# 4. Inspect container details
docker inspect app
# 5. Check resource usage
docker stats
```

The logic: **`ps`** tells you *if* it's up and healthy; **`logs`** usually tells you *what* went wrong (stack traces, connection errors); if the message is ambiguous, **`exec`** lets you reproduce the problem by hand from inside; **`inspect`** reveals the exact config Docker gave the container (env, mounts, network, ports) when reality doesn't match your file; and **`stats`** catches the sneaky cases — a container being OOM-killed or pegged at 100% CPU that no log line will confess to.

### Inspect a Container

`docker inspect` dumps the container's *complete* configuration as JSON — every env var, mount, network, and port as Docker actually applied it. It's verbose, so pipe it through `grep` to find the field you need:

```bash
docker inspect app                # full JSON config dump
docker inspect app | grep -i ip   # find IP address
docker inspect app | grep -i port # find port mappings
```

This is the tool of last resort when the running container disagrees with what you *think* you configured — the JSON never lies about what Docker actually did.

---

---

[← Back to index](../Docker.md)
