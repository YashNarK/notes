# Docker Master Cheatsheet — Part 5 — Full-Stack Reference

[← Back to index](../Docker.md)

---

## 15. Our Full Architecture

Here's the complete system the rest of these notes build toward — several services running side by side, all wired together on one Docker network.

### Services Overview

```
┌─────────────────────────────────────────────────────────┐
│                    Docker Network                        │
│                                                          │
│  ┌──────────────┐    ┌──────────────┐  ┌─────────────┐ │
│  │  Next.js     │    │  Node.js     │  │  Python     │ │
│  │  Frontend    │    │  Express     │  │  FastAPI    │ │
│  │  Port: 3000  │    │  REST API    │  │  GraphQL    │ │
│  │              │    │  Port: 4000  │  │  Port: 8000 │ │
│  └──────┬───────┘    └──────┬───────┘  └──────┬──────┘ │
│         │                   │                  │         │
│         │           ┌───────▼───────┐   ┌──────▼──────┐ │
│         │           │  PostgreSQL   │   │  BigQuery   │ │
│         │           │  Port: 5432   │   │  Emulator   │ │
│         │           │  (Node app DB)│   │  Port: 9050 │ │
│         │           └───────────────┘   └─────────────┘ │
└─────────────────────────────────────────────────────────┘
         ↑                   ↑                  ↑
    localhost:3000      localhost:4000     localhost:8000
```

How to read this diagram:

- The big outer box is a single **Docker network**. Every service inside it can reach the others by name (Docker's built-in DNS) — no IP addresses to hunt down.
- The **top row** is your three application services: a Next.js frontend, a Node.js/Express REST API, and a Python/FastAPI GraphQL service. Each exposes a port.
- The **arrows going down** show data dependencies: the Node API talks to its PostgreSQL database, and the Python service talks to a BigQuery emulator. The frontend has no database of its own — it calls the APIs.
- The **arrows at the bottom** (`↑ localhost:3000` etc.) are the ports mapped out to *your* machine. Inside the network services find each other by name; from your browser you reach them at `localhost:<port>`.

The key mental split: PostgreSQL and the BigQuery emulator are *internal* — only the services that need them talk to them. The three top-row services are what you, the outside world, actually connect to.

### Project Structure

And here's how that maps onto folders on disk. Each service is self-contained: its own `Dockerfile` (production), `Dockerfile.dev` (development), and `.dockerignore`.

```
my-platform/
├── docker-compose.yml           # development
├── docker-compose.prod.yml      # production
├── .env                         # secrets (never commit)
├── .env.example                 # template (commit this)
├── .gitignore
│
├── frontend/                    # Next.js
│   ├── Dockerfile
│   ├── Dockerfile.dev
│   ├── .dockerignore
│   ├── next.config.js
│   ├── package.json
│   └── src/
│
├── backend-node/                # Node.js Express REST API
│   ├── Dockerfile
│   ├── Dockerfile.dev
│   ├── .dockerignore
│   ├── package.json
│   └── src/
│
├── backend-python/              # Python FastAPI GraphQL
│   ├── Dockerfile
│   ├── Dockerfile.dev
│   ├── .dockerignore
│   ├── requirements.txt
│   └── app/
│
└── nginx/                       # Reverse proxy (production)
    └── nginx.conf
```

Notice the two `docker-compose` files at the root: `docker-compose.yml` for local development and `docker-compose.prod.yml` for production — same services, different settings. The paired `Dockerfile` / `Dockerfile.dev` in each service follows the same dev-vs-prod split. And `nginx/` only shows up in production: it's the reverse proxy that sits in front of everything and routes incoming requests to the right service.

---

---

## 16. All Dockerfiles

This section is a real-world reference set: the actual Dockerfiles for a full-stack app (Next.js frontend, Node.js API, Python API). Each service ships **two** files — a production `Dockerfile` and a `Dockerfile.dev`. The pattern is always the same, so once you understand one you understand all of them:

- **Production** = multi-stage build. Compile/install everything in a fat "builder" stage, then copy *only the finished artifacts* into a tiny final image. Result: small, fast, secure images with no build tools left inside.
- **Dev** = single stage. Install everything, mount your source code from the host, and run a hot-reload server. Optimized for fast feedback, not for size.

Two habits repeat across every production file below, and both are security best practices worth internalizing:
- **Non-root user.** By default a container runs as `root`. If an attacker breaks out of your app, they're root inside the container — a much bigger blast radius. Creating an unprivileged user and switching to it with `USER` limits the damage.
- **`HEALTHCHECK`.** Docker periodically runs a command inside the container; if it fails, the container is marked `unhealthy` and orchestrators (or `depends_on: condition: service_healthy`) know not to send it traffic.

### Next.js Frontend

Next.js has a special trick that makes it Docker-friendly: **standalone output**. Normally a Next app needs the whole `node_modules` folder to run. With `output: 'standalone'`, Next traces exactly which files are actually used and bundles them into a self-contained `.next/standalone` folder — so the final image can skip installing production dependencies entirely and just copy that folder. This is why the config below is *required* for the multi-stage Dockerfile to work.

**`frontend/next.config.js`** — required for standalone output:

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  output: 'standalone',    // enables minimal production build
}

module.exports = nextConfig
```

The production Dockerfile uses **three stages**. Stage 1 (`deps`) installs only production dependencies in isolation so this layer is cached and rebuilt only when `package.json` changes. Stage 2 (`builder`) does the actual `npm run build`. Stage 3 (`runner`) is the real image you ship: it copies just the standalone server, the static assets, and the public folder — no source code, no dev tools, no build cache. Notice it creates a `nextjs` user and runs as it, and the copied files are `--chown`ed to that user so the process can read them.

**`frontend/Dockerfile`** (production, multi-stage):

```dockerfile
# ── Stage 1: Install dependencies ────────────────────────
FROM node:18-alpine AS deps
RUN apk add --no-cache libc6-compat
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci --only=production

# ── Stage 2: Build the app ───────────────────────────────
FROM node:18-alpine AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build

# ── Stage 3: Production runner ───────────────────────────
FROM node:18-alpine AS runner
WORKDIR /app
ENV NODE_ENV production

# Create non-root user (security best practice)
RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs

# Copy only what's needed to run
COPY --from=builder /app/public ./public
COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static

USER nextjs
EXPOSE 3000
ENV PORT 3000

HEALTHCHECK --interval=30s --timeout=10s --retries=3 --start-period=20s \
  CMD curl -f http://localhost:3000/api/health || exit 1

CMD ["node", "server.js"]
```

The dev version throws all of that away. It's a single stage: install *all* dependencies (including dev ones), copy the code, and run `npm run dev`. In practice you don't even rely on the `COPY . .` — Compose bind-mounts your live source over `/app` so edits on your laptop reload instantly. Small, simple, made for iteration.

**`frontend/Dockerfile.dev`** (development, with hot reload):

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "run", "dev"]
```

A `.dockerignore` works like `.gitignore` but for the Docker build context: listed paths are never sent to the daemon or copied by `COPY . .`. This keeps builds fast (no shipping a huge `node_modules`) and avoids leaking secrets. The `!.env.example` line is an *un-ignore* — it excludes all `.env*` files but keeps the safe example template.

**`frontend/.dockerignore`**:

```dockerignore
node_modules
.next
.git
*.md
.env*
!.env.example
```

---

### Node.js Express Backend (REST API)

Same two-stage idea as the frontend, adapted for a plain Node/TypeScript API. Stage 1 (`builder`) installs *everything* and runs the build (e.g. compiling TypeScript to `dist/`). Stage 2 (`production`) starts fresh, installs **only** production dependencies (`npm ci --only=production`), clears the npm cache to shave off megabytes, and copies just the compiled `dist/` from the builder. The build tools and dev dependencies never make it into the shipped image.

**`backend-node/Dockerfile`** (production):

```dockerfile
# ── Stage 1: Build ───────────────────────────────────────
FROM node:18-alpine AS builder
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci
COPY . .
RUN npm run build     # if using TypeScript

# ── Stage 2: Production ──────────────────────────────────
FROM node:18-alpine AS production
WORKDIR /app
ENV NODE_ENV production

# Non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Install only production dependencies
COPY package.json package-lock.json ./
RUN npm ci --only=production && npm cache clean --force

# Copy built app from builder stage
COPY --from=builder /app/dist ./dist

USER appuser
EXPOSE 4000

HEALTHCHECK --interval=30s --timeout=10s --retries=3 --start-period=15s \
  CMD curl -f http://localhost:4000/health || exit 1

CMD ["node", "dist/server.js"]
```

The dev image again collapses to one stage and runs `npm run dev`, which typically launches `nodemon` — a watcher that restarts the server whenever a source file changes.

**`backend-node/Dockerfile.dev`**:

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
EXPOSE 4000
CMD ["npm", "run", "dev"]    # uses nodemon for hot reload
```

**`backend-node/.dockerignore`**:

```dockerignore
node_modules
dist
.git
*.md
.env*
!.env.example
coverage
*.test.ts
```

---

### Python FastAPI Backend (GraphQL)

Python doesn't compile to a `dist/` folder, so its multi-stage trick is different: it uses **`pip install --user`**. That installs packages into `/root/.local` instead of the system site-packages. The builder stage needs `gcc` to compile any packages with native extensions — but `gcc` is bulky and you don't want it in production. So the production stage copies just the finished `/root/.local` directory from the builder and adds it to `PATH`, getting the installed packages without the compiler.

First, the dependency list that both stages consume:

**`backend-python/requirements.txt`**:

```
fastapi==0.104.0
uvicorn[standard]==0.24.0
strawberry-graphql[fastapi]==0.213.0
google-cloud-bigquery==3.13.0
python-dotenv==1.0.0
pydantic==2.4.2
```

Two env vars are worth knowing here: `PYTHONDONTWRITEBYTECODE=1` stops Python from littering `.pyc` files, and `PYTHONUNBUFFERED=1` forces logs to stream out immediately instead of being buffered — critical so `docker logs` shows output in real time.

**`backend-python/Dockerfile`** (production):

```dockerfile
# ── Stage 1: Build dependencies ──────────────────────────
FROM python:3.11-slim AS builder
WORKDIR /app

# Install build tools
RUN apt-get update && \
    apt-get install -y --no-install-recommends gcc && \
    rm -rf /var/lib/apt/lists/*

COPY requirements.txt .
RUN pip install --user --no-cache-dir -r requirements.txt

# ── Stage 2: Production ──────────────────────────────────
FROM python:3.11-slim AS production
WORKDIR /app
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PATH=/root/.local/bin:$PATH

# Copy installed packages from builder
COPY --from=builder /root/.local /root/.local

# Copy app code
COPY app/ ./app/

# Non-root user
RUN adduser --disabled-password --gecos "" appuser
USER appuser
EXPOSE 8000

HEALTHCHECK --interval=30s --timeout=10s --retries=3 --start-period=20s \
  CMD curl -f http://localhost:8000/health || exit 1

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

The dev version installs `curl` too (handy for poking the API from inside the container) and adds `--reload` to uvicorn, which watches your files and restarts on every change — the Python equivalent of nodemon.

**`backend-python/Dockerfile.dev`**:

```dockerfile
FROM python:3.11-slim
WORKDIR /app
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

RUN apt-get update && \
    apt-get install -y --no-install-recommends gcc curl && \
    rm -rf /var/lib/apt/lists/*

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 8000

# --reload enables hot reload in dev
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000", "--reload"]
```

**`backend-python/.dockerignore`**:

```dockerignore
pycache
*.pyc
*.pyo
.venv
venv
.git
*.md
.env*
!.env.example
tests/
.pytest_cache
```

---

---

## 17. Docker Compose Files

The Dockerfiles above each build *one* image. Compose is what wires all those images together into a running system — frontend, two backends, a database, an emulator — with one `docker compose up`. This app keeps **two** compose files, and the difference between them is the whole lesson in dev-vs-prod:

- **`docker-compose.yml`** (development): services use `build:` to build images from local source, bind-mount your code for hot reload, expose ports to your laptop, and run a local BigQuery emulator instead of the real cloud.
- **`docker-compose.prod.yml`** (production): services use `image:` to pull *pre-built* images from a registry, mount nothing but read-only secrets, hide the database from the outside world, add `restart: always`, and put an Nginx reverse proxy out front.

### .env (development secrets — never commit)

Compose automatically reads a file named `.env` in the same directory and substitutes `${VAR}` references in the YAML. This keeps passwords and config out of the committed compose file — which is exactly why this file itself must never be committed (add it to `.gitignore`; commit only a `.env.example` template).

```bash
# Database
POSTGRES_USER=appuser
POSTGRES_PASSWORD=devpassword123
POSTGRES_DB=appdb

# Google BigQuery (for local dev, use emulator)
BIGQUERY_PROJECT_ID=local-project
GOOGLE_APPLICATION_CREDENTIALS=/app/service-account.json

# App secrets
JWT_SECRET=dev-jwt-secret-change-in-prod
NODE_ENV=development

# Service URLs (used by frontend)
NEXT_PUBLIC_API_URL=http://localhost:4000
NEXT_PUBLIC_GRAPHQL_URL=http://localhost:8000/graphql
```

### docker-compose.yml (development)

Read this top to bottom as a dependency chain. The frontend waits for both backends to be *healthy* (not just started — `condition: service_healthy` uses those `HEALTHCHECK`s); the Node backend waits for Postgres; the Python backend waits for the BigQuery emulator. That ordering is what stops the classic "app crashes because the database isn't ready yet" race.

The volume trick under `frontend` is the one that trips everyone up: `./frontend:/app` bind-mounts your live source for hot reload, but that would also overwrite the container's `node_modules` and `.next` with your host's (possibly empty or OS-mismatched) versions. The two anonymous volumes (`/app/node_modules`, `/app/.next`) are placed *on top* to shield those directories, keeping the ones the container installed. Also note services talk to each other by **service name** as hostname — `DB_HOST: postgres`, not `localhost` — because Compose puts them on a shared network where the service name resolves to the container.

```yaml
services:
  # ── Frontend: Next.js ─────────────────────────────────
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile.dev
    ports:
      - "3000:3000"
    volumes:
      - ./frontend:/app             # bind mount for hot reload
      - /app/node_modules           # anonymous volume — keeps container's node_modules
      - /app/.next                  # don't overwrite .next from container
    environment:
      NODE_ENV: development
      NEXT_PUBLIC_API_URL: http://localhost:4000
      NEXT_PUBLIC_GRAPHQL_URL: http://localhost:8000/graphql
    depends_on:
      backend-node:
        condition: service_healthy
      backend-python:
        condition: service_healthy

  # ── Backend 1: Node.js Express ────────────────────────
  backend-node:
    build:
      context: ./backend-node
      dockerfile: Dockerfile.dev
    ports:
      - "4000:4000"
    volumes:
      - ./backend-node:/app
      - /app/node_modules
    environment:
      NODE_ENV: development
      PORT: 4000
      DB_HOST: postgres
      DB_PORT: 5432
      DB_USER: ${POSTGRES_USER}
      DB_PASSWORD: ${POSTGRES_PASSWORD}
      DB_NAME: ${POSTGRES_DB}
      JWT_SECRET: ${JWT_SECRET}
    depends_on:
      postgres:
        condition: service_healthy
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:4000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 15s

  # ── Backend 2: Python FastAPI ─────────────────────────
  backend-python:
    build:
      context: ./backend-python
      dockerfile: Dockerfile.dev
    ports:
      - "8000:8000"
    volumes:
      - ./backend-python:/app
    environment:
      BIGQUERY_PROJECT_ID: ${BIGQUERY_PROJECT_ID}
      BIGQUERY_API_ENDPOINT: http://bigquery-emulator:9050   # local emulator
      GOOGLE_CLOUD_PROJECT: ${BIGQUERY_PROJECT_ID}
    depends_on:
      - bigquery-emulator
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 20s

  # ── Database 1: PostgreSQL ────────────────────────────
  postgres:
    image: postgres:15-alpine
    ports:
      - "5432:5432"                 # expose for local DB tools (DBeaver, etc.)
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./backend-node/db/init.sql:/docker-entrypoint-initdb.d/init.sql  # auto-runs on first start
    environment:
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: ${POSTGRES_DB}
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}"]
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 30s

  # ── Database 2: BigQuery Emulator (dev only) ──────────
  bigquery-emulator:
    image: ghcr.io/goccy/bigquery-emulator:latest
    ports:
      - "9050:9050"
      - "9060:9060"
    command:
      - --project=${BIGQUERY_PROJECT_ID}
      - --dataset=analytics
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:9050/bigquery/v2/projects"]
      interval: 10s
      timeout: 5s
      retries: 3

volumes:
  postgres_data:
```

### docker-compose.prod.yml (production)

Compare this file line-by-line against the dev one — the shape is identical but every choice flips toward "safe and stable" instead of "fast to iterate":

- **`image:` instead of `build:`** — production servers don't build code. Your CI pipeline builds and pushes tagged images to a registry, and prod just pulls the exact `${IMAGE_TAG}` you tested. This makes deploys reproducible and rollbacks trivial (just point at the previous tag).
- **No bind-mount volumes** — the code is *baked into* the image, so there's nothing to mount. The only volume mounts left are the persistent database and a read-only secret (`:ro`).
- **`restart: always`** — if a container crashes or the host reboots, Docker brings it back automatically.
- **Postgres exposes no port** — in dev you exposed 5432 for DB tools; in prod the database is reachable only over the internal network, never from the public internet.
- **Nginx reverse proxy added** — a single entry point on 80/443 that terminates TLS and routes to the internal services. The real BigQuery is used, so the emulator is gone.

```yaml
services:
  # ── Frontend: Next.js ─────────────────────────────────
  frontend:
    image: yourregistry/frontend:${IMAGE_TAG}    # pre-built image, no build:
    ports:
      - "3000:3000"
    environment:
      NODE_ENV: production
      NEXT_PUBLIC_API_URL: ${API_URL}
      NEXT_PUBLIC_GRAPHQL_URL: ${GRAPHQL_URL}
    depends_on:
      backend-node:
        condition: service_healthy
      backend-python:
        condition: service_healthy
    restart: always
    deploy:
      replicas: 2                               # run 2 instances

  # ── Backend 1: Node.js Express ────────────────────────
  backend-node:
    image: yourregistry/backend-node:${IMAGE_TAG}
    ports:
      - "4000:4000"
    environment:
      NODE_ENV: production
      PORT: 4000
      DB_HOST: postgres
      DB_USER: ${POSTGRES_USER}
      DB_PASSWORD: ${POSTGRES_PASSWORD}
      DB_NAME: ${POSTGRES_DB}
      JWT_SECRET: ${JWT_SECRET}
    depends_on:
      postgres:
        condition: service_healthy
    restart: always
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:4000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 15s

  # ── Backend 2: Python FastAPI ─────────────────────────
  backend-python:
    image: yourregistry/backend-python:${IMAGE_TAG}
    ports:
      - "8000:8000"
    volumes:
      - ./service-account.json:/app/service-account.json:ro  # read-only secret file
    environment:
      BIGQUERY_PROJECT_ID: ${BIGQUERY_PROJECT_ID}
      GOOGLE_APPLICATION_CREDENTIALS: /app/service-account.json
    restart: always
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 20s

  # ── Database 1: PostgreSQL ────────────────────────────
  postgres:
    image: postgres:15-alpine
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: ${POSTGRES_DB}
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER}"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: always
    # No port exposed in production — internal access only

  # ── Reverse Proxy: Nginx ──────────────────────────────
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./ssl:/etc/nginx/ssl:ro
    depends_on:
      - frontend
      - backend-node
      - backend-python
    restart: always

  # No BigQuery emulator in production — use real BigQuery

volumes:
  postgres_data:
```

---

---

[← Back to index](../Docker.md)
