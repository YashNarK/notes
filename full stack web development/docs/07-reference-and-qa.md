# Docker Master Cheatsheet — Part 7 — Reference & Q&A

[← Back to index](../Docker.md)

---

## 19. Essential Commands

Your quick-reference sheet, grouped by what you're acting on — images, containers, Compose, volumes, networks, and the system as a whole. Skim the group you need; every command has a one-line reminder of what it does.

### Image Commands

```bash
docker build -t name:tag .                     # build image from Dockerfile in current dir
docker build -t name:tag -f Dockerfile.dev .   # build using a specific Dockerfile
docker images                                  # list all local images
docker image ls                                # same as above
docker pull nginx:alpine                       # pull an image from a registry
docker push yourname/app:1.0                   # push an image to a registry
docker tag app:latest yourname/app:1.0         # give an existing image a new tag
docker rmi image-name                          # remove an image
docker image prune                             # remove dangling (untagged) images
docker image prune -a                          # remove all unused images
docker history image-name                      # show the layers that make up an image
docker inspect image-name                      # detailed image info (JSON)
```

### Container Commands

```bash
docker run image-name                # run a container
docker run -d image-name             # run in background (detached)
docker run -p 3000:3000 image-name   # map host port to container port
docker run -v mydata:/app/data image # mount a volume
docker run -e KEY=value image        # set an environment variable
docker run --name myapp image        # give the container a name
docker run --rm image                # auto-remove the container when it stops
docker run -it image sh              # run with an interactive shell
docker ps                            # list running containers
docker ps -a                         # list all containers (including stopped)
docker stop container-name           # graceful stop (sends SIGTERM)
docker kill container-name           # force stop (sends SIGKILL)
docker start container-name          # start a stopped container
docker restart container-name        # restart a container
docker rm container-name             # remove a stopped container
docker rm -f container-name          # force-remove a running container
docker exec -it container sh         # open a shell inside a running container
docker exec container env            # run a command inside a running container
docker logs container                # view a container's logs
docker logs -f container             # follow logs live
docker logs --tail 100 container     # show the last 100 log lines
docker inspect container             # detailed container info (JSON)
docker stats                         # live resource usage (CPU, memory, I/O)
docker top container                 # processes running inside the container
docker cp file.txt container:/app/   # copy a file into the container
docker cp container:/app/file.txt .  # copy a file out of the container
```

### Compose Commands

```bash
docker compose up                    # start all services
docker compose up -d                 # start in background (detached)
docker compose up --build            # rebuild images, then start
docker compose up --scale app=3      # run 3 instances of the app service
docker compose down                  # stop and remove containers
docker compose down -v               # also remove volumes
docker compose down --rmi all        # also remove images
docker compose ps                    # list services
docker compose logs                  # show all logs
docker compose logs -f               # follow all logs live
docker compose logs -f service-name  # follow logs for one service
docker compose build                 # build all images
docker compose build service-name    # build one image
docker compose pull                  # pull all images
docker compose restart service-name  # restart one service
docker compose exec service sh       # open a shell in a running service
docker compose run --rm service cmd  # run a one-off command in a fresh container
docker compose config                # validate and print the merged config
docker compose -f other.yml up       # use a specific compose file
```

### Volume Commands

```bash
docker volume ls                     # list all volumes
docker volume create mydata          # create a named volume
docker volume inspect mydata         # show details about a volume
docker volume rm mydata              # remove a volume
docker volume prune                  # remove all unused volumes
```

### Network Commands

```bash
docker network ls                            # list all networks
docker network create mynetwork              # create a network
docker network inspect mynetwork             # show network details
docker network rm mynetwork                  # remove a network
docker network prune                         # remove unused networks
docker network connect mynetwork container   # attach a container to a network
```

### System Commands

```bash
docker system df                     # show Docker disk usage
docker system prune                  # remove stopped containers, unused networks, dangling images
docker system prune -a               # also remove unused images
docker system prune -a --volumes     # nuclear — removes everything unused, including volumes
docker version                       # show Docker version info
docker info                          # show system-wide info
```

---

---

## 20. Common Q&A

### From Our Conversation

**Q: What does COPY . . do?**

The syntax is `COPY <source> <destination>`. There are two dots and they mean different things:

- The **first `.`** = your current project folder on your machine (the build context).
- The **second `.`** = the current `WORKDIR` inside the container.

So `COPY . .` means: *copy everything from my project folder into the container's working directory.* If you set `WORKDIR /app` earlier, the files land in `/app`.

---

**Q: What is the Dockerfile filename? How do you specify multiple?**

The default filename is exactly `Dockerfile` — no extension, capital D. Docker looks for it automatically. When you want different setups per environment, give each file a name and point at it with `-f`:

```bash
docker build -t app -f Dockerfile.dev .    # development
docker build -t app -f Dockerfile.prod .   # production
```

Common naming convention: `Dockerfile`, `Dockerfile.dev`, `Dockerfile.prod`, `Dockerfile.test`. The base `Dockerfile` is the one used when you don't pass `-f`.

---

**Q: What is the `.` at the end of `docker build`?**

It's the **build context** — the folder Docker packages up and uses as the root for every `COPY` command. This is separate from *where the Dockerfile lives*. Most of the time they're the same folder (hence the `.`), but they don't have to be:

```bash
docker build -f /path/to/Dockerfile /path/to/context
```

Here the Dockerfile comes from one place and the files it copies come from another. The gotcha: `COPY` can only reach files *inside* the build context — a file outside that folder is invisible to the build, no matter where it is on your disk.

---

**Q: Volumes inside vs outside — how does the mapping work?**

Think of it as a **shared folder between two computers**. A volume is not *syncing* two copies — it's the *same physical folder* accessed from two sides. When the container writes to `/app/data`, that write lands on your machine at the mapped path immediately. There's no copy step and no delay.

Why you care: the data outlives the container. Delete the container and the data is still on your machine. Mount a brand-new container to the same volume and the data is right back where it was. That's how a database survives a `docker rm`.

---

**Q: What is `mydata` in `-v mydata:/var/lib/postgresql/data`? Is it a folder or a label?**

It's a **label** — a *named volume* that Docker manages for you. You don't say where it lives on disk; Docker picks the location internally (usually somewhere under `/var/lib/docker/volumes/`). You just use the name `mydata` to refer to it.

Here's the rule that tells you which kind of mount you're looking at:

- Starts with `/` or `./` → a **real path** on your machine (a bind mount).
- Just a plain word → a **named volume** Docker manages.

So `-v ./data:/app` mounts your local `./data` folder, but `-v mydata:/app` uses a Docker-managed volume called `mydata`.

---

**Q: Why do `volumes` use hyphens but `environment` doesn't?**

This is plain **YAML syntax**, not a Docker rule:

- **Hyphens** = a *list* (used when a field accepts many values of the same type).
- **`key: value` with no hyphen** = a *map* (used when each entry has a unique name).

`volumes` is a list because you can mount many volumes. `environment` is a map because each variable has a unique name and one value. (Note: `environment` *also* accepts list style — `- KEY=value` — so both forms are valid there. When in doubt, either works for env vars.)

---

**Q: Do I need network commands in `docker-compose.yml`?**

No. Compose **automatically creates a network** for your project and attaches every service to it. Services reach each other by service name out of the box — no config required. You only define custom networks when you deliberately want to **isolate** services from one another (for example, keeping a database off the same network as a public-facing service).

---

**Q: How do I know if a Compose field accepts a list, a map, or both?**

- Fields holding multiple values of the same type → **list** with hyphens (`ports`, `volumes`, `depends_on`).
- Fields with named key-value pairs → **map** without hyphens (`environment`, `labels`, `build`).
- Some fields accept **both** (`environment`, `labels`). The Docker docs at `docs.docker.com/compose/compose-file` are the source of truth.
- Easiest in practice: VS Code with the Docker extension gives autocomplete and validation as you type, so you'll see immediately if a field wants a list or a map.

---

**Q: What is `depends_on`?**

It controls **startup order**. `depends_on: - db` tells Compose to start `db` before `app`. But here's the catch everyone hits: it only waits for the container to **START**, not for the service inside it to be **READY**. Postgres's container can be "up" while Postgres itself is still initializing and refusing connections. To wait for actual readiness, combine it with a healthcheck:

```yaml
depends_on:
  db:
    condition: service_healthy
```

Now Compose waits until the healthcheck passes before starting `app`.

---

**Q: In production, why use `image:` instead of `build:` in Compose?**

Your production server doesn't have your source code — it only has Docker. That's the whole point:

- `build:` tells Compose to build the image from a Dockerfile, which **requires the source code** to be present.
- `image:` tells Compose to pull a **pre-built image** from a registry — no source code needed.

The workflow: build on your laptop (or CI), push the finished image to a registry, then `image:` pulls it on the server. The server never sees your code, only the packaged result.

---

### Additional Scenarios

**Q: My container starts then immediately exits. Why?**

A container lives exactly as long as its **foreground process** (the one in `CMD`) keeps running. When that process finishes or crashes, the container stops — that's by design, not a bug. Common causes:

- The app crashed on startup (bad env var, can't reach the DB).
- `CMD` ran a one-off command that completed, instead of a long-running server.

To find out which:

```bash
docker logs container-name   # check what the process printed before dying
docker run -it image sh      # get inside and run things manually to debug
```

---

**Q: I changed my code but the container still shows the old code.**

It depends on how the code got into the container:

- **In dev with a bind mount**: the container sees your edits live — so if you're not seeing changes, your bind mount probably isn't set up. Check your `volumes:` mapping.
- **In production**: the code is *baked into the image at build time*, so an edit on disk changes nothing until you rebuild:

```bash
docker compose up --build    # force a rebuild so new code gets baked in
```

---

**Q: My Node app can't connect to Postgres. It says "connection refused".**

Three usual suspects:

1. **Wrong hostname** — inside Compose you reach the DB by its *service name* (`postgres`), not `localhost`. `localhost` inside a container means *that container itself*, not the DB.
2. **Race condition** — the app started before the DB was ready. Add `depends_on` with `condition: service_healthy`.
3. **Wrong credentials** — `DB_USER`, `DB_PASSWORD`, `DB_NAME` must match what Postgres was started with.

Debug from inside the app container:

```bash
docker exec -it backend-node sh
curl http://postgres:5432       # test connectivity to the DB service
env | grep DB                   # check the env vars actually got set
```

---

**Q: How do I run database migrations?**

Two clean ways:

```bash
# One-off command inside the already-running container
docker compose exec backend-node npm run migrate

# Or as a throwaway service that runs once and removes itself
docker compose run --rm backend-node npm run migrate
```

Use `exec` when the service is already up; use `run --rm` when you want a fresh, self-cleaning container just for the migration.

---

**Q: My container says "healthy" but the app still doesn't work. How?**

Your `/health` endpoint is probably returning `200` without actually checking anything — so the healthcheck passes while the real dependency (the DB) is down. Make the health check verify the things the app truly needs:

```javascript
app.get('/health', async (req, res) => {
  try {
    await db.query('SELECT 1')    // actually hit the DB
    res.status(200).json({ status: 'ok' })
  } catch {
    res.status(503).json({ status: 'error' })
  }
})
```

Now "healthy" means "can actually serve requests," which is what everyone assumes it already meant.

---

**Q: How do I share data between two containers without a volume?**

You don't — containers have completely separate filesystems by design. Your options are:

- Use a **volume** both containers mount, for shared files.
- Use a **shared service** (Redis, a database) as the intermediary for shared state.

Reaching into another container's filesystem directly isn't a thing; go through a volume or a service.

---

**Q: BigQuery is a cloud service — how do I run it locally?**

You use the **BigQuery emulator**, which runs the API locally in a container:

```bash
docker run -p 9050:9050 ghcr.io/goccy/bigquery-emulator:latest \
  --project=local-project --dataset=analytics
```

Your Python app points at `http://localhost:9050` instead of the real GCP endpoint. In production you remove the emulator and swap in real GCP credentials — ideally the app reads the endpoint from an env var so no code changes between local and prod.

---

**Q: What's the difference between `docker compose down` and `docker compose stop`?**

The key difference is what gets **removed**:

```bash
docker compose stop    # stops the containers but keeps them (fast restart, data preserved)
docker compose down    # stops AND removes the containers (network too)
docker compose down -v # stops, removes containers AND named volumes — DATA IS GONE!
```

`stop` is a pause; `down` is a teardown. The `-v` flag is the dangerous one — it deletes your volumes, so never run it against production data you want to keep.

---

**Q: How do I avoid reinstalling `node_modules` on every rebuild?**

Use the **layer-caching pattern**: copy only the dependency manifest first, install, *then* copy the rest of the code. Because Docker caches layers, the expensive `npm ci` step is reused unless `package.json` actually changes:

```dockerfile
COPY package.json package-lock.json ./
RUN npm ci                  # cached — only re-runs if package.json changed
COPY . .                    # editing code doesn't bust the npm cache above
```

If you copied everything first (`COPY . .` before `npm ci`), any code edit would invalidate the install layer and force a full reinstall every time.

---

---

## 21. Scenarios & Troubleshooting

### Scenario: Onboarding a New Developer

The whole point of Docker for a team: a new developer clones the repo and types one command — no installing Node, Python, or Postgres on their machine.

```bash
cp .env.example .env   # fill in values
docker compose up
```

All services start. Their laptop stays clean; everything runs in containers.

---

### Scenario: Running Multiple Environments Simultaneously

You can run dev and staging side by side on the same machine without them clashing:

```bash
# Dev on port 3000
docker compose -f docker-compose.yml -p myapp-dev up -d

# Staging on port 3001
docker compose -f docker-compose.staging.yml -p myapp-staging up -d
```

The `-p` flag sets the **project name**, which prefixes container and network names — that's what prevents the two stacks from colliding.

---

### Scenario: Debugging a Production Issue

Reproduce prod locally by pulling the *exact* image that's running there:

```bash
# Pull the exact image that's running in prod to your laptop
docker pull yourregistry/backend-node:1.2.0

# Run it with prod env vars
docker run --env-file .env.prod yourregistry/backend-node:1.2.0

# Or get inside the running prod container (carefully)
docker exec -it backend-node sh
```

Because you pinned a version tag (`1.2.0`, not `latest`), the image on your laptop is byte-for-byte the one in production — so bugs reproduce faithfully.

---

### Scenario: Database Volume Got Corrupted

Tear down, drop the bad volume, and restore from backup:

```bash
# Stop everything
docker compose down

# Remove the corrupted volume
docker volume rm myapp_postgres_data

# Restore from backup
docker compose up -d postgres
docker exec -i postgres psql -U appuser appdb < backup.sql

# Start everything else
docker compose up -d
```

Note the volume name is prefixed with the project name (`myapp_`) — that's the same `-p` project-naming behavior showing up in the auto-created volume.

---

### Scenario: Image Size is Too Large

First find out *what* is taking the space, then apply the standard fixes:

```bash
# Check what's taking space
docker history image-name

# Fixes:
# 1. Use Alpine base: node:18 (950MB) → node:18-alpine (170MB)
# 2. Multi-stage builds to discard build tools
# 3. Clean up in same RUN layer:
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*
# 4. Check .dockerignore — are node_modules being copied in?
```

The subtle one is #3: cleanup must happen in the *same* `RUN` as the install. If you delete files in a later layer, the earlier layer still carries them — layers only add, never shrink what came before.

---

### Scenario: Container Keeps Restarting

A restart loop means the app keeps crashing and a `restart` policy keeps relaunching it. Read the exit code and logs to find the cause:

```bash
# Check exit code and logs
docker ps -a                    # shows Exit code
docker logs container-name      # see what crashed

# Common causes:
# - Bad env vars (missing DB_PASSWORD etc.)
# - DB not ready when app starts (fix: depends_on with health check)
# - Port already in use on host
# - Out of memory
```

---

---

## 22. Interview Questions

### Beginner Level

**Q: What is Docker and why do we use it?**

Docker packages an application together with all its dependencies (runtime, libraries, config) into a portable **container**. It solves the classic "works on my machine" problem by guaranteeing an identical environment across dev, CI, and production. The payoffs: consistency, faster onboarding, isolated processes, and easy scaling.

---

**Q: What is the difference between a Docker image and a container?**

An **image** is a static, read-only blueprint stored on disk — it does nothing on its own. A **container** is a running instance of an image — a live process. You can launch many containers from one image. The analogy that sticks: image = class, container = object instance.

---

**Q: What is a Dockerfile?**

A text file of step-by-step instructions for building a Docker image. Each instruction (`FROM`, `COPY`, `RUN`, `CMD`, etc.) creates a **layer**. During `docker build`, Docker reads the file top to bottom and executes each instruction in order.

---

**Q: What is the difference between RUN and CMD?**

Timing is the whole distinction:

- `RUN` executes at **build time** and bakes its result into the image (installing packages, compiling code).
- `CMD` executes at **run time**, when a container starts (launching your app).

A Dockerfile can have many `RUN` instructions but only one effective `CMD`.

---

**Q: What is Docker Hub?**

A public registry for Docker images — think GitHub, but for images. When you write `FROM node:18`, Docker pulls that image from Docker Hub. You can push your own images there so anyone can pull and run them anywhere.

---

### Intermediate Level

**Q: Explain Docker layers and why their order matters.**

Every Dockerfile instruction creates a layer, and Docker **caches** each one. On rebuild, Docker reuses unchanged layers and only re-runs a changed layer *plus everything after it*. So a frequently-changing layer near the top invalidates every cache below it. Best practice: put slow, rarely-changing steps (like `npm install`) **before** fast, frequently-changing ones (like `COPY . .`), so your code edits don't trigger a full reinstall.

---

**Q: What is the difference between a bind mount and a named volume?**

A **bind mount** maps a specific path on your host machine into the container — *you* choose the location, and you see the files directly. A **named volume** is managed by Docker — you give it a name and Docker decides where it lives on disk. Rule of thumb: named volumes for databases (Docker manages them efficiently and safely), bind mounts for development (instant access to your source files for hot reload).

---

**Q: How does networking work in Docker Compose?**

Compose automatically creates a default network and attaches every service to it. Each service is reachable **by its service name** as a hostname — a service named `db` is reachable at `db:5432` from other containers. You do *not* use `localhost` between containers, because inside a container `localhost` refers to that container itself, not its neighbors.

---

**Q: What is `depends_on` and what doesn't it guarantee?**

`depends_on` controls container **startup order** — it guarantees a container *starts* before the dependent one. What it does **not** guarantee is that the service *inside* is ready: Postgres might need 5 seconds to initialize after its container is up. The fix is `depends_on` with `condition: service_healthy`, backed by a `healthcheck` that verifies the service is truly accepting connections.

---

**Q: What is a multi-stage build and when do you use it?**

A multi-stage build uses multiple `FROM` instructions in one Dockerfile — each `FROM` begins a new stage. You use earlier stages for **building** (with all the compilers and tools installed) and a later, minimal stage for **running**. Files move between stages with `COPY --from=stage-name`. The result is a tiny production image that carries only the finished artifact — no build tools, no compilers, no dev dependencies.

---

**Q: What is `.dockerignore` and why is it important?**

A file that tells Docker what to **exclude** from the build context sent to the daemon during `docker build`. It matters for three reasons: (1) **size** — `node_modules` can be hundreds of MB you don't want shipped, (2) **speed** — a smaller context uploads and builds faster, (3) **security** — `.env` files with secrets must never be baked into an image. It works like `.gitignore`, but for the build.

---

### Advanced Level

**Q: How would you achieve zero-downtime deployments with Docker?**

Several approaches, from simplest to most robust:

1. **Docker Compose with `--scale`**: bring up new instances before stopping the old ones.
2. **Nginx upstream rotation**: update the Nginx config to point at the new containers.
3. **Kubernetes rolling update**: native, built-in support for zero-downtime rollouts.
4. **Blue-green deployment**: run two identical environments and switch traffic at the DNS / load-balancer level.

---

**Q: How do you manage secrets in Docker production environments?**

Never put secrets in images or in Compose files committed to git. Real options:

- **Docker Secrets** (Docker Swarm): encrypted secrets mounted into the container as files.
- **AWS Secrets Manager / Parameter Store**: fetched at runtime.
- **HashiCorp Vault**: dynamic secrets with rotation.
- **Environment variables from CI/CD**: injected at deploy time, never stored in the repo.
- Bare minimum: a `.env` file that lives *only* on the server, never inside the image.

---

**Q: What's the difference between ENTRYPOINT and CMD?**

Both help define what runs when a container starts, but the actual command is built by **joining them together**:

```
what actually runs  =  ENTRYPOINT  +  CMD
```

The rule that explains everything is *which part your `docker run` arguments replace*:

- Anything you type after the image name (`docker run my-app ...`) **replaces CMD entirely**.
- It does **not** touch ENTRYPOINT.

So `CMD` provides **defaults you can override** by passing arguments to `docker run`, while `ENTRYPOINT` defines the **fixed executable** — your `docker run` arguments get appended to it rather than replacing it. The common pattern is `ENTRYPOINT` = the executable, `CMD` = its default arguments; together they make the container behave like a self-contained binary.

**Mental model:** `ENTRYPOINT` is written in **ink** (permanent — always runs); `CMD` is written in **pencil** (your `docker run` args erase and rewrite it). Need to override the ink too? `docker run --entrypoint bash my-app` is the escape hatch.

---

**Q: How would you debug a container that exits immediately?**

```bash
docker logs container-name              # check crash reason
docker ps -a                            # see the exit code
docker run -it --entrypoint sh image    # override entrypoint to get a shell
docker inspect container                # check config, env vars, mounts
```

Then check the usual culprits: missing env vars, wrong `CMD`, port conflict, out of memory, or a dependency that wasn't ready.

---

**Q: What is layer caching and how can it go wrong?**

Docker caches layers and reuses them when their inputs haven't changed. It bites you when: (1) you *expect* fresh data but get a stale cache — bust it with `docker build --no-cache`; (2) `COPY . .` appears early, so every code change invalidates all downstream caches; (3) you run `apt-get update` without `apt-get install` in the *same* `RUN` — the update layer gets cached and later installs pull from a stale package list. The theme: cache is your friend for speed and your enemy when it hides staleness.

---

**Q: How would you handle a Python FastAPI connecting to BigQuery in both local dev and production?**

In **development**, run a BigQuery emulator as a Docker service and point the app at `http://bigquery-emulator:9050`. In **production**, use real GCP credentials via a service-account JSON or Workload Identity. The trick that ties them together: the app reads `BIGQUERY_API_ENDPOINT` from an env var — dev points it at the emulator, prod points it at real GCP. No code change between environments, only config.

---

---

## 23. Best Practices & Modern Patterns

### Security

```dockerfile
# 1. Run as non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

# 2. Use specific versions, not latest
FROM node:18.19.0-alpine3.19    # pin exact version

# 3. Scan images for vulnerabilities
docker scout cves image-name
docker scan image-name

# 4. Never COPY secrets into image
# Use .dockerignore to exclude .env files

# 5. Read-only filesystem where possible
docker run --read-only image-name

# 6. Minimal base images — smaller attack surface
FROM node:18-alpine    # not node:18
FROM python:3.11-slim  # not python:3.11
```

Why these matter: running as root means a container escape is a *host* root escape; `latest` makes builds non-reproducible; and every extra package in a fat base image is one more thing that can have a CVE.

### Performance

```dockerfile
# 1. Layer caching — dependencies before code
COPY package.json .
RUN npm ci
COPY . .            # code last

# 2. Use npm ci instead of npm install in production
RUN npm ci --only=production   # faster, uses lockfile exactly

# 3. Combine RUN commands to reduce layers
RUN apt-get update && \
    apt-get install -y curl && \
    rm -rf /var/lib/apt/lists/*

# 4. Use .dockerignore aggressively

# 5. Multi-stage builds for smaller images
```

`npm ci` installs *exactly* what the lockfile says and is built for automation, so it's both faster and more reproducible than `npm install` in CI and production.

### Modern Compose Patterns

```yaml
# 1. Use profiles for optional services
services:
  app:
    profiles: []              # always starts
  debug-tools:
    profiles: [debug]         # only starts with: docker compose --profile debug up

# 2. Resource limits (production safety)
services:
  app:
    deploy:
      resources:
        limits:
          cpus: '0.5'
          memory: 512M

# 3. restart policies
restart: always           # always restart (production)
restart: unless-stopped   # restart unless manually stopped
restart: on-failure       # only on non-zero exit

# 4. Logging configuration
logging:
  driver: "json-file"
  options:
    max-size: "10m"
    max-file: "3"
```

Resource limits keep one misbehaving container from starving the whole host, and log rotation (`max-size` / `max-file`) stops container logs from silently filling the disk — a classic production outage.

### Environment Management

```bash
# Use different .env files per environment
docker compose --env-file .env.staging up
docker compose --env-file .env.prod up

# Validate your compose file
docker compose config          # merges and validates

# Never interpolate secrets directly
# Bad:
environment:
  - DB_PASSWORD=hardcoded      # visible to anyone who reads the file

# Good:
environment:
  - DB_PASSWORD=${DB_PASSWORD} # from .env file or CI/CD injection
```

`docker compose config` is worth running before every deploy — it merges your files, expands variables, and shows you the exact final config, so you catch a broken interpolation locally instead of in production.

### Tagging Strategy

```bash
# Use semantic versioning + git SHA
docker build -t app:1.2.0 .
docker build -t app:1.2.0-$(git rev-parse --short HEAD) .

# Tag for multiple registries
docker tag app:1.2.0 dockerhub/app:1.2.0
docker tag app:1.2.0 gcr.io/project/app:1.2.0
docker tag app:1.2.0 ghcr.io/username/app:1.2.0

# Never use latest in production CI/CD
# Always use explicit version tags for reproducibility
```

The git SHA in the tag lets you trace any running image straight back to the exact commit that built it — invaluable when you're debugging "which version is actually deployed?"

### Health Check Best Practices

```dockerfile
# Good: lightweight endpoint, no heavy operations
HEALTHCHECK --interval=30s --timeout=5s --retries=3 --start-period=30s \
  CMD curl -f http://localhost:3000/health || exit 1

# Better: check actual dependencies too
# (in your /health route, query the DB)

# Always set start_period for slow-starting apps (JVM, Python startup, etc.)
```

`--start-period` gives a slow-booting app grace time: failures during that window don't count against the retry limit, so a JVM that takes 20 seconds to warm up isn't marked unhealthy and killed before it ever finishes starting.

### Local Development Best Practices

```yaml
# Dev compose — mount source for hot reload
volumes:
  - ./src:/app/src              # your source code
  - /app/node_modules           # preserve container's node_modules
  - /app/.next                  # preserve Next.js build cache

# This pattern ensures:
# - Your code changes reflect immediately
# - Container's dependencies aren't overwritten by your machine's
```

The trick is the second and third lines: an *anonymous* volume on `/app/node_modules` shadows that path so your host's (possibly empty or wrong-platform) `node_modules` doesn't clobber the one the container installed. You get live code edits *and* the container keeps its own dependencies.

### Production Checklist

```
✅ All images use specific version tags (not latest)
✅ No secrets in Dockerfiles or committed compose files
✅ All services have health checks
✅ All services have restart: always
✅ No source code bind mounts (code baked into image)
✅ Non-root user in all Dockerfiles
✅ Volumes declared for all stateful data (databases)
✅ Database ports not exposed publicly (no ports: for postgres in prod)
✅ Multi-stage builds used for smaller images
✅ .dockerignore present in all service directories
✅ Images scanned for vulnerabilities before deploy
✅ Resource limits set (memory, CPU)
✅ Log rotation configured
✅ Backup strategy for database volumes
```

---

[← Back to index](../Docker.md)
