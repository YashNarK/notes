# Docker Master Cheatsheet — Part 4 — Builds & Optimization

[← Back to index](../Docker.md)

---

## 13. Multi-Stage Builds

**The idea in one line:** the tools you need to *build* an app are almost never the tools you need to *run* it — so build in one image, then throw that image away and ship a tiny second image with only the finished product.

### The Problem

Think about what it takes to turn source code into a running app. Building it pulls in compilers, package managers, and every development dependency. But once the app is built, all of that is dead weight — the finished output doesn't need webpack or babel sitting next to it.

```
To BUILD a Next.js app: Node 18 + npm + webpack + babel + all dev deps (~1GB)
To SERVE a Next.js app: Just static files + nginx (~40MB)
```

If you build and serve in the same image, you ship that whole ~1GB toolchain to production even though it does nothing there. It's like mailing someone a cake *and* the entire kitchen you baked it in.

Multi-stage builds let you use one image for building and a different, lighter image for running.

### How It Works

You put **two (or more) `FROM` statements** in a single Dockerfile. Each `FROM` starts a fresh, independent stage. The final image is built from the **last** stage only — earlier stages exist just long enough to produce something the last stage copies out.

```dockerfile
# ── Stage 1: Builder ──────────────────────────────
FROM node:18 AS builder      # heavy image, has the full toolchain
WORKDIR /app
COPY package.json .
RUN npm install              # includes dev dependencies
COPY . .
RUN npm run build            # compiles your app into /app/dist

# ── Stage 2: Production ──────────────────────────
FROM nginx:alpine            # completely new, tiny base image
COPY --from=builder /app/dist /usr/share/nginx/html
# Only the compiled output comes along.
# node_modules, source code, build tools — all discarded.
```

Walk through it step by step:

1. **`FROM node:18 AS builder`** starts stage 1 on the big Node image and names it `builder` (the `AS` gives the stage a label you can refer to later).
2. Stage 1 installs everything and runs `npm run build`, leaving the compiled output at `/app/dist`.
3. **`FROM nginx:alpine`** starts stage 2 completely fresh — a brand-new, tiny image. Nothing from stage 1 is here yet. No Node, no npm, no source code.
4. **`COPY --from=builder /app/dist ...`** reaches *back into stage 1* and copies out only `/app/dist` — the finished files. This is the one bridge between the stages.

Stage 1 is thrown away after stage 2 copies what it needs. **The final image is only stage 2** — nginx plus your compiled files, and nothing else.

**Mental model:** stage 1 is your messy workshop; stage 2 is the display case. You do all the sawing and gluing in the workshop, then carry *only the finished product* into the clean display case and lock the workshop away.

### Size Comparison

This is not a small optimization — the difference is an order of magnitude:

| | Without multi-stage | With multi-stage |
|---|---|---|
| Next.js app | ~950MB | ~45MB |
| Python app | ~900MB | ~130MB |
| Node API | ~950MB | ~200MB |

Smaller images push faster, pull faster, start faster, and give attackers less to work with. There's essentially no downside.

### Key Syntax

```dockerfile
FROM node:18 AS builder          # name a stage with AS
COPY --from=builder /app/dist .  # copy from a named stage
COPY --from=0 /app/dist .        # copy from stage by index (0 = first)
```

`COPY --from=<stage>` is the whole trick: `<stage>` can be a name you gave with `AS`, or a zero-based index (`0` is the first `FROM`, `1` the second, and so on). Names are clearer — reach for `AS builder` over counting stages.

### Production Examples

Below are battle-tested, production-grade multi-stage Dockerfiles for each stack. Two habits recur in every one, and both are non-negotiable in production:

- **Non-root user.** Containers run as `root` by default; a breakout then owns root inside the container. Create an unprivileged user and switch to it with `USER` to shrink the blast radius.
- **`HEALTHCHECK`.** Docker probes the container on an interval; a failing probe marks it `unhealthy` so orchestrators (and `depends_on: condition: service_healthy`) stop routing traffic to it.

Each also pins a digest-free but explicit base tag (`node:18-alpine`, `python:3.12-slim`) — never `latest` — so builds are reproducible.

#### React.js (Vite/CRA SPA → nginx)

A React app compiles to a folder of static files. Build it with the full Node toolchain, then throw all of that away and serve the static bundle from a tiny nginx image. The final image contains **no Node, no npm, no source** — just nginx and your compiled assets.

**`frontend/Dockerfile`**:

```dockerfile
# ── Stage 1: Build the static bundle ─────────────────────
FROM node:18-alpine AS builder
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci
COPY . .
RUN npm run build          # emits ./dist (Vite) or ./build (CRA)

# ── Stage 2: Serve with nginx ────────────────────────────
FROM nginx:1.27-alpine AS runner
# SPA-aware config: fall back to index.html for client-side routes
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 80
HEALTHCHECK --interval=30s --timeout=5s --retries=3 --start-period=5s \
  CMD wget -qO- http://localhost/ >/dev/null 2>&1 || exit 1

# nginx:alpine already drops to an unprivileged worker for request handling.
CMD ["nginx", "-g", "daemon off;"]
```

**`frontend/nginx.conf`** — required so deep links (`/dashboard`) don't 404 on refresh:

```nginx
server {
    listen 80;
    server_name _;
    root /usr/share/nginx/html;
    index index.html;

    # Cache hashed static assets aggressively
    location /assets/ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }

    # SPA fallback: any unknown path serves the app shell
    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

> `CRA` builds to `build/` instead of `dist/` — change the `COPY --from=builder /app/build ...` line accordingly.

#### Node.js (generic TypeScript service)

A plain Node/TypeScript service: compile TS to `dist/` in the builder, then start fresh and install **only** production dependencies. The compiler and dev deps never reach the shipped image.

**`service/Dockerfile`**:

```dockerfile
# ── Stage 1: Build (compile TypeScript) ──────────────────
FROM node:18-alpine AS builder
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci                        # all deps, incl. typescript
COPY . .
RUN npm run build                 # tsc → ./dist

# ── Stage 2: Production runtime ──────────────────────────
FROM node:18-alpine AS production
WORKDIR /app
ENV NODE_ENV=production

RUN addgroup -S nodejs && adduser -S nodeuser -G nodejs

COPY package.json package-lock.json ./
RUN npm ci --omit=dev && npm cache clean --force

COPY --from=builder --chown=nodeuser:nodejs /app/dist ./dist

USER nodeuser
EXPOSE 3000
HEALTHCHECK --interval=30s --timeout=10s --retries=3 --start-period=15s \
  CMD node -e "require('http').get('http://localhost:3000/health',r=>process.exit(r.statusCode===200?0:1)).on('error',()=>process.exit(1))"

CMD ["node", "dist/index.js"]
```

#### Express.js (REST API)

Same shape as the generic Node service, tuned for an Express REST API. If the API is plain JavaScript (no build step), the builder stage still earns its keep by isolating `devDependencies` from the runtime image.

**`api/Dockerfile`**:

```dockerfile
# ── Stage 1: Install + build ─────────────────────────────
FROM node:18-alpine AS builder
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci
COPY . .
RUN npm run build                 # omit this line for plain-JS APIs

# ── Stage 2: Production ──────────────────────────────────
FROM node:18-alpine AS production
WORKDIR /app
ENV NODE_ENV=production

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

COPY package.json package-lock.json ./
RUN npm ci --omit=dev && npm cache clean --force

# Compiled output for TS; for plain JS copy ./src instead.
COPY --from=builder --chown=appuser:appgroup /app/dist ./dist

USER appuser
EXPOSE 4000
HEALTHCHECK --interval=30s --timeout=10s --retries=3 --start-period=15s \
  CMD wget -qO- http://localhost:4000/health >/dev/null 2>&1 || exit 1

CMD ["node", "dist/server.js"]
```

#### Next.js (standalone output)

Next.js ships a Docker-friendly trick: **standalone output**. With `output: 'standalone'`, Next traces exactly which files are used and bundles a self-contained `.next/standalone` server, so the final image skips installing production `node_modules` entirely.

**`web/next.config.js`** — required:

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  output: 'standalone',
}
module.exports = nextConfig
```

**`web/Dockerfile`** — three stages: isolated deps, build, minimal runner:

```dockerfile
# ── Stage 1: Dependencies ────────────────────────────────
FROM node:18-alpine AS deps
RUN apk add --no-cache libc6-compat
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci

# ── Stage 2: Build ───────────────────────────────────────
FROM node:18-alpine AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
ENV NEXT_TELEMETRY_DISABLED=1
RUN npm run build

# ── Stage 3: Runner (the shipped image) ──────────────────
FROM node:18-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
ENV NEXT_TELEMETRY_DISABLED=1

RUN addgroup --system --gid 1001 nodejs \
 && adduser  --system --uid 1001 nextjs

COPY --from=builder /app/public ./public
COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static

USER nextjs
EXPOSE 3000
ENV PORT=3000 HOSTNAME=0.0.0.0
HEALTHCHECK --interval=30s --timeout=10s --retries=3 --start-period=20s \
  CMD wget -qO- http://localhost:3000/api/health >/dev/null 2>&1 || exit 1

CMD ["node", "server.js"]
```

#### Python + FastAPI

Python has no `dist/` folder, so the multi-stage trick differs: build a **virtual environment** in the builder (where `gcc`/build headers exist to compile native wheels), then copy just the finished venv into a clean `slim` runtime that never carries a compiler.

**`api/requirements.txt`**:

```
fastapi==0.111.0
uvicorn[standard]==0.30.1
pydantic==2.7.4
```

**`api/Dockerfile`**:

```dockerfile
# ── Stage 1: Build wheels into a venv ────────────────────
FROM python:3.12-slim AS builder
WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends gcc \
 && rm -rf /var/lib/apt/lists/*

RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# ── Stage 2: Production runtime ──────────────────────────
FROM python:3.12-slim AS production
WORKDIR /app
ENV PYTHONUNBUFFERED=1 PYTHONDONTWRITEBYTECODE=1 \
    PATH="/opt/venv/bin:$PATH"

RUN groupadd --system appgroup && useradd --system --gid appgroup appuser

# Copy just the finished virtual environment — no gcc, no build headers.
COPY --from=builder /opt/venv /opt/venv
COPY --chown=appuser:appgroup ./app ./app

USER appuser
EXPOSE 8000
HEALTHCHECK --interval=30s --timeout=10s --retries=3 --start-period=10s \
  CMD python -c "import urllib.request,sys; sys.exit(0 if urllib.request.urlopen('http://localhost:8000/health').status==200 else 1)"

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

#### PostgreSQL and MongoDB — multi-stage usually *doesn't* apply

Databases are the exception. You run them from the **official images** (`postgres:16-alpine`, `mongo:7`) unchanged — there's no source to build, so a multi-stage Dockerfile buys you nothing. In production you don't rebuild the database image at all; you supply configuration, credentials (via secrets, not `ENV`), and a persistent volume through Compose/orchestration:

```yaml
services:
  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: appdb
      POSTGRES_USER_FILE: /run/secrets/pg_user
      POSTGRES_PASSWORD_FILE: /run/secrets/pg_password
    volumes:
      - pgdata:/var/lib/postgresql/data
      - ./init:/docker-entrypoint-initdb.d:ro   # *.sql / *.sh run once on first boot
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U $$(cat /run/secrets/pg_user) -d appdb"]
      interval: 10s
      timeout: 5s
      retries: 5
volumes:
  pgdata:
```

**The one time multi-stage *is* justified for a database:** compiling a native extension (e.g. a PostgreSQL C extension, `pgvector`, or PostGIS from source) that isn't in the base image. Build the extension in a stage that has the toolchain, then copy only the resulting `.so` and control files into the clean official image:

```dockerfile
# ── Stage 1: Compile the extension ───────────────────────
FROM postgres:16 AS builder
RUN apt-get update && apt-get install -y --no-install-recommends \
      build-essential postgresql-server-dev-16 git \
 && git clone --depth 1 --branch v0.7.4 https://github.com/pgvector/pgvector.git /tmp/pgvector \
 && make -C /tmp/pgvector && make -C /tmp/pgvector install

# ── Stage 2: Clean runtime image ─────────────────────────
FROM postgres:16-alpine AS runtime
# Copy only the built artifacts — none of the build-essential/dev toolchain.
COPY --from=builder /usr/lib/postgresql/16/lib/vector.so /usr/local/lib/postgresql/
COPY --from=builder /usr/share/postgresql/16/extension/vector* /usr/local/share/postgresql/extension/
```

MongoDB follows the same rule: run `mongo:7` as-is with a volume on `/data/db`; there is no build step and thus no multi-stage use, short of the rare case of compiling a custom module.

---

---

## 14. .dockerignore

`COPY . .` copies **everything** in your build context into the image. `.dockerignore` tells Docker what to skip — same idea as `.gitignore`, but for Docker builds.

### Why It Matters

Three separate reasons, and every one of them bites eventually:

- **Size**: `node_modules` alone can be 200–500MB. Copying it in bloats the image — and it's pointless, because the container installs its own dependencies with `RUN npm install`.
- **Speed**: before a build even starts, Docker packages up the whole build context and sends it to the daemon. A smaller context means a faster `docker build` (and a fast cache).
- **Security**: `.env` contains secrets — API keys, passwords, tokens. If it gets baked into an image, anyone who pulls that image can read your secrets. **Never put `.env` in an image.**

### Standard .dockerignore

```dockerignore
# Dependencies (container installs its own)
node_modules
pycache
*.pyc
.venv
venv

# Version control
.git
.gitignore

# Environment & secrets
.env
.env.*
!.env.example

# Compose files (not needed inside container)
docker-compose*.yml
Dockerfile*

# Documentation
*.md
docs/

# Test files
*.test.js
*.spec.js
tests/
tests/
coverage/

# Build artifacts (will be regenerated)
.next/
dist/
build/
out/

# OS files
.DS_Store
Thumbs.db

# IDE files
.vscode/
.idea/
```

Two patterns worth calling out: `.env.*` ignores every environment variant, but `!.env.example` *un-ignores* the template with a leading `!` — you want that committed so teammates know which variables to set. And build artifacts like `.next/` and `dist/` are skipped because the build regenerates them; copying stale ones in just causes confusion.

### The Three Files Every Project Must Have

These three work together, and beginners constantly mix them up:

```
.gitignore        → keeps secrets out of git
.dockerignore     → keeps secrets out of Docker images
.env              → the actual secrets (listed in both above)
```

The gotcha that trips everyone up: `.gitignore` and `.dockerignore` are **separate files with separate jobs**. A secret excluded from git can still leak into a Docker image if `.dockerignore` doesn't also list it. `.env` must appear in *both* ignore files — git protects your repo, `.dockerignore` protects your images.

---

---

[← Back to index](../Docker.md)
