# Docker & Containerization

## Table of contents

- [Docker \& Containerization](#docker--containerization)
  - [Table of contents](#table-of-contents)
  - [What is Docker?](#what-is-docker)
  - [Core Concepts](#core-concepts)
  - [Docker CLI Essentials](#docker-cli-essentials)
  - [Dockerfile](#dockerfile)
  - [Multi-stage Builds](#multi-stage-builds)
  - [Docker Compose](#docker-compose)
  - [Networking](#networking)
  - [Volumes and Data Persistence](#volumes-and-data-persistence)
  - [Container Registry](#container-registry)
  - [Docker for Frontend Development](#docker-for-frontend-development)
  - [Best Practices](#best-practices)
  - [Kubernetes Basics](#kubernetes-basics)

---

## What is Docker?

**Docker** packages applications and their dependencies into isolated, portable units called **containers**. Containers run the same everywhere — your laptop, CI server, and production cloud.

**Benefits:**
- **Consistency** — eliminates "works on my machine"
- **Isolation** — apps don't interfere with each other
- **Portability** — runs on any OS or cloud provider
- **Efficiency** — lightweight compared to VMs (share host OS kernel)
- **Scalability** — easy to spin up multiple instances

**Docker vs Virtual Machine:**

| | Docker Container | Virtual Machine |
|---|---|---|
| OS | Shares host kernel | Full guest OS |
| Startup | Seconds | Minutes |
| Size | MB | GB |
| Isolation | Process-level | Hardware-level |

---

## Core Concepts

| Concept | Description |
|---|---|
| **Image** | Read-only template (like a class) — built from a Dockerfile |
| **Container** | Running instance of an image (like an object) |
| **Dockerfile** | Instructions to build an image |
| **Registry** | Remote storage for images (Docker Hub, ECR, GHCR) |
| **Volume** | Persistent storage that survives container restarts |
| **Network** | Virtual network connecting containers |
| **Compose** | Tool to define and run multi-container apps |
| **Layer** | Each Dockerfile instruction creates a cacheable layer |

---

## Docker CLI Essentials

```bash
# --- Images ---
docker images                         # list local images
docker pull nginx:alpine              # pull from registry
docker build -t myapp:v1.0 .          # build from Dockerfile in current dir
docker rmi myapp:v1.0                 # remove image
docker image prune -a                 # remove all unused images

# --- Containers ---
docker run nginx                      # run container (foreground)
docker run -d nginx                   # run detached (background)
docker run -d -p 8080:80 nginx        # map host port 8080 → container port 80
docker run -d --name myapp \
  -p 3000:3000 \
  -e NODE_ENV=production \
  -v ./data:/app/data \
  myapp:latest

docker ps                             # list running containers
docker ps -a                          # list all containers (including stopped)
docker stop myapp                     # graceful stop
docker kill myapp                     # immediate stop
docker rm myapp                       # remove stopped container
docker rm -f myapp                    # force remove (running or stopped)

# --- Container interaction ---
docker logs myapp                     # view logs
docker logs -f myapp                  # follow logs
docker exec -it myapp sh              # open shell in running container
docker exec -it myapp node -e "..."   # run a command in container
docker cp myapp:/app/logs ./logs      # copy files from container

# --- Cleanup ---
docker system prune                   # remove stopped containers, unused networks/images
docker system prune -a --volumes      # full cleanup including volumes
```

---

## Dockerfile

```dockerfile
# Start from a base image
FROM node:20-alpine

# Set working directory
WORKDIR /app

# Copy package files first (layer cache optimization)
COPY package.json package-lock.json ./

# Install dependencies
RUN npm ci --only=production

# Copy application source
COPY . .

# Set environment variable
ENV NODE_ENV=production \
    PORT=3000

# Expose port (documentation only — doesn't publish)
EXPOSE 3000

# Create non-root user for security
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

# Default command
CMD ["node", "server.js"]
```

**Key Dockerfile instructions:**

| Instruction | Purpose |
|---|---|
| `FROM` | Base image |
| `WORKDIR` | Set working directory |
| `COPY` | Copy files from host to image |
| `ADD` | Like COPY but supports URLs and auto-extracts tarballs |
| `RUN` | Execute command during build (creates a new layer) |
| `ENV` | Set environment variables |
| `ARG` | Build-time variable (not in final image) |
| `EXPOSE` | Document which port the app uses |
| `CMD` | Default command (overridable at runtime) |
| `ENTRYPOINT` | Fixed command (CMD becomes its arguments) |
| `USER` | Set the user for subsequent instructions |
| `HEALTHCHECK` | Command to test if container is healthy |
| `VOLUME` | Create a mount point |

---

## Multi-stage Builds

Produce a small, production-optimized image by using multiple `FROM` stages.

```dockerfile
# ===== Stage 1: Build =====
FROM node:20-alpine AS builder
WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .
RUN npm run build               # outputs to /app/dist


# ===== Stage 2: Production =====
FROM nginx:alpine AS production

# Copy built assets from builder stage
COPY --from=builder /app/dist /usr/share/nginx/html

# Custom nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

**Result:** The production image contains only nginx + built assets — no Node.js, no dev dependencies, no source code. Typical size reduction: **500MB → 20MB**.

**nginx.conf for SPA routing:**
```nginx
server {
    listen 80;
    root /usr/share/nginx/html;
    index index.html;
    gzip on;
    gzip_types text/css application/javascript image/svg+xml;

    location / {
        try_files $uri $uri/ /index.html;   # SPA fallback
    }

    location /api/ {
        proxy_pass http://backend:3000/;    # proxy to backend service
    }

    # Cache static assets
    location ~* \.(js|css|png|jpg|svg|woff2)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}
```

---

## Docker Compose

Define and manage multi-container applications with a single YAML file.

```yaml
# docker-compose.yml
version: '3.9'

services:
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    ports:
      - '3000:80'
    environment:
      - VITE_API_URL=http://localhost:4000
    depends_on:
      - backend
    restart: unless-stopped

  backend:
    build: ./backend
    ports:
      - '4000:4000'
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgresql://user:pass@db:5432/mydb
      - REDIS_URL=redis://cache:6379
    depends_on:
      db:
        condition: service_healthy
      cache:
        condition: service_started
    restart: unless-stopped

  db:
    image: postgres:16-alpine
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./db/init.sql:/docker-entrypoint-initdb.d/init.sql
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: mydb
    healthcheck:
      test: ['CMD-SHELL', 'pg_isready -U user -d mydb']
      interval: 10s
      timeout: 5s
      retries: 5

  cache:
    image: redis:7-alpine
    volumes:
      - redis_data:/data

volumes:
  postgres_data:
  redis_data:
```

```bash
# Compose commands
docker compose up -d              # start all services (detached)
docker compose up --build         # rebuild images before starting
docker compose down               # stop and remove containers
docker compose down -v            # also remove volumes
docker compose logs -f backend    # follow backend logs
docker compose exec backend sh    # shell into backend container
docker compose ps                 # list services and status
docker compose restart backend    # restart a specific service

# Override file for development
docker compose -f docker-compose.yml -f docker-compose.dev.yml up
```

**Development override:**
```yaml
# docker-compose.dev.yml
services:
  frontend:
    build:
      target: development      # use dev stage from multi-stage Dockerfile
    volumes:
      - ./frontend/src:/app/src    # hot reload
    command: npm run dev

  backend:
    volumes:
      - ./backend:/app
    command: npm run dev
    environment:
      - NODE_ENV=development
```

---

## Networking

```bash
# Docker automatically creates a network for Compose services
# Services can reach each other by service name (DNS)
# backend → connects to db at hostname "db" on port 5432

# Custom networks
docker network create mynetwork
docker network ls
docker network inspect mynetwork
```

```yaml
# Compose networks
services:
  frontend:
    networks:
      - public
  backend:
    networks:
      - public
      - internal
  db:
    networks:
      - internal      # not reachable from frontend

networks:
  public:
  internal:
    internal: true    # no external access
```

---

## Volumes and Data Persistence

```bash
# Named volume (Docker managed)
docker volume create mydata
docker run -v mydata:/app/data nginx

# Bind mount (host directory)
docker run -v ./local/path:/app/data nginx   # development: live sync
docker run -v /absolute/path:/app/data nginx

# Inspect volumes
docker volume ls
docker volume inspect mydata
docker volume rm mydata
```

```yaml
# Compose volume types
services:
  app:
    volumes:
      - data_vol:/app/data          # named volume (persistent)
      - ./src:/app/src              # bind mount (dev hot reload)
      - /app/node_modules           # anonymous volume (don't overwrite with bind mount)

volumes:
  data_vol:
    driver: local
```

---

## Container Registry

```bash
# Docker Hub
docker login
docker tag myapp:v1.0 username/myapp:v1.0
docker push username/myapp:v1.0
docker pull username/myapp:v1.0

# GitHub Container Registry (GHCR)
echo $GITHUB_TOKEN | docker login ghcr.io -u USERNAME --password-stdin
docker tag myapp:latest ghcr.io/username/myapp:latest
docker push ghcr.io/username/myapp:latest

# AWS ECR
aws ecr get-login-password --region us-east-1 | \
  docker login --username AWS --password-stdin 123456789.dkr.ecr.us-east-1.amazonaws.com

docker tag myapp:latest 123456789.dkr.ecr.us-east-1.amazonaws.com/myapp:latest
docker push 123456789.dkr.ecr.us-east-1.amazonaws.com/myapp:latest
```

**GitHub Actions — build and push to GHCR:**
```yaml
- name: Log in to GHCR
  uses: docker/login-action@v3
  with:
    registry: ghcr.io
    username: ${{ github.actor }}
    password: ${{ secrets.GITHUB_TOKEN }}

- name: Build and push
  uses: docker/build-push-action@v5
  with:
    context: .
    push: true
    tags: ghcr.io/${{ github.repository }}:${{ github.sha }}
    cache-from: type=gha
    cache-to: type=gha,mode=max
```

---

## Docker for Frontend Development

```dockerfile
# Dockerfile with dev and prod stages
FROM node:20-alpine AS base
WORKDIR /app
COPY package.json package-lock.json ./

# Development stage
FROM base AS development
RUN npm install
COPY . .
EXPOSE 5173
CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]

# Build stage
FROM base AS builder
RUN npm ci
COPY . .
ARG VITE_API_URL
ENV VITE_API_URL=$VITE_API_URL
RUN npm run build

# Production stage
FROM nginx:alpine AS production
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

```bash
# Run dev with hot reload
docker build --target development -t myapp:dev .
docker run -p 5173:5173 -v ./src:/app/src myapp:dev

# Build production image with build arg
docker build \
  --target production \
  --build-arg VITE_API_URL=https://api.example.com \
  -t myapp:prod .
```

**`.dockerignore`:**
```
node_modules
.git
dist
.env
*.log
.DS_Store
coverage
.github
```

---

## Best Practices

1. **Use specific image tags** — never `latest` in production
   ```dockerfile
   FROM node:20.11.1-alpine3.19   # ✅ pinned version
   FROM node:latest               # ❌ unpredictable
   ```

2. **Order Dockerfile instructions for cache efficiency**
   ```dockerfile
   COPY package.json ./     # changes rarely — cached
   RUN npm ci               # cached if package.json unchanged
   COPY . .                 # changes often — invalidates from here
   ```

3. **Use non-root user**
   ```dockerfile
   RUN addgroup -S app && adduser -S app -G app
   USER app
   ```

4. **Multi-stage builds** — keep production images small

5. **Use `.dockerignore`** — exclude unnecessary files

6. **One process per container** — don't run a web server and a worker in one container

7. **Make containers stateless** — store state in volumes or external services

8. **Health checks**
   ```dockerfile
   HEALTHCHECK --interval=30s --timeout=3s \
     CMD curl -f http://localhost:3000/health || exit 1
   ```

9. **Use environment variables for config** — not baked-in values

---

## Kubernetes Basics

**Kubernetes (K8s)** orchestrates containers at scale — manages deployment, scaling, and recovery.

```yaml
# deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 3                           # run 3 instances
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
        - name: frontend
          image: ghcr.io/org/frontend:v1.2.3
          ports:
            - containerPort: 80
          resources:
            requests:
              memory: '64Mi'
              cpu: '50m'
            limits:
              memory: '128Mi'
              cpu: '100m'
          readinessProbe:
            httpGet:
              path: /health
              port: 80
            initialDelaySeconds: 5

---
# service.yml — expose the deployment
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
spec:
  selector:
    app: frontend
  ports:
    - port: 80
      targetPort: 80
  type: LoadBalancer
```

```bash
kubectl apply -f deployment.yml
kubectl get pods
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl scale deployment frontend --replicas=5
kubectl rollout status deployment/frontend
kubectl rollout undo deployment/frontend    # rollback
```
