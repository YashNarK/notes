# Docker Master Cheatsheet — Part 6 — SDLC & Workflow

[← Back to index](../Docker.md)

---

## 18. SDLC Approach

Docker isn't just a "run my app" tool — it threads through every phase of the Software Development Lifecycle. The arc below follows one project from an empty machine all the way to a monitored production deployment: **dev → test → build → CI/CD → deploy → update → monitor**. Each phase reuses the same images and Compose files, so nothing gets rebuilt "differently" between your laptop and the server.

### Phase 1: Local Development Setup

**Purpose:** get a brand-new machine running the whole stack in minutes, without installing Node, Python, or Postgres locally — Docker provides all of them.

```bash
# 1. Clone the repo
git clone https://github.com/yourname/my-platform.git
cd my-platform

# 2. Set up environment
cp .env.example .env
# Edit .env with your actual values

# 3. Start everything
docker compose up
# That's it. No Node, Python, or Postgres installation needed on your machine.
```

### Phase 2: Development Loop

**Purpose:** the fast edit-and-see-it cycle — bind mounts sync your source into the container so code changes appear live without rebuilding.

```bash
# Code changes are reflected immediately (bind mounts in dev Compose)
# You just edit files and see changes live

# View logs while working
docker compose logs -f

# Rebuild if you change dependencies
docker compose up --build backend-node

# Run database migrations
docker compose exec backend-node npm run migrate

# Open a shell for debugging
docker compose exec backend-node sh
docker compose exec postgres psql -U appuser -d appdb
```

### Phase 3: Testing

**Purpose:** run your test suite in a throwaway, isolated container so it can't pollute (or be polluted by) your running dev environment.

```bash
# Run tests in isolated container (doesn't affect your running dev env)
docker compose run --rm backend-node npm test
docker compose run --rm backend-python pytest

# Or create a separate test compose file
docker compose -f docker-compose.test.yml up --abort-on-container-exit
```

**`docker-compose.test.yml`**:

```yaml
services:
  backend-node-test:
    build: ./backend-node
    environment:
      NODE_ENV: test
      DB_HOST: postgres-test
    depends_on:
      - postgres-test
    command: npm test

  postgres-test:
    image: postgres:15-alpine
    environment:
      POSTGRES_PASSWORD: testpassword
      POSTGRES_DB: testdb
```

### Phase 4: Building for Production

**Purpose:** bake versioned, immutable production images — each tagged with a version so you always know exactly what's running and can roll back to it.

```bash
# Bump version
export VERSION=1.2.0

# Build all production images
docker build -t yourregistry/frontend:$VERSION ./frontend
docker build -t yourregistry/backend-node:$VERSION ./backend-node
docker build -t yourregistry/backend-python:$VERSION ./backend-python

# Verify images
docker images | grep yourregistry

# Test production images locally before pushing
docker compose -f docker-compose.prod.yml up
```

### Phase 5: CI/CD Pipeline

**Purpose:** automate build → push → deploy on every commit to `main`, so a `git push` reliably ships the exact code that was reviewed — no manual steps to forget. Each image is tagged with the commit SHA (`github.sha`), giving you a permanent, traceable link between "what's deployed" and "which commit."

**`.github/workflows/deploy.yml`**:

```yaml
name: Build and Deploy

on:
  push:
    branches: [main]

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Login to registry
        run: echo ${{ secrets.REGISTRY_TOKEN }} | docker login -u ${{ secrets.REGISTRY_USER }} --password-stdin
      - name: Build images
        run: |
          docker build -t yourregistry/frontend:${{ github.sha }} ./frontend
          docker build -t yourregistry/backend-node:${{ github.sha }} ./backend-node
          docker build -t yourregistry/backend-python:${{ github.sha }} ./backend-python
      - name: Push images
        run: |
          docker push yourregistry/frontend:${{ github.sha }}
          docker push yourregistry/backend-node:${{ github.sha }}
          docker push yourregistry/backend-python:${{ github.sha }}

  deploy:
    needs: build-and-push
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to server
        uses: appleboy/ssh-action@v0.1.7
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: ${{ secrets.SERVER_USER }}
          key: ${{ secrets.SSH_KEY }}
          script: |
            cd /app
            export IMAGE_TAG=${{ github.sha }}
            docker compose -f docker-compose.prod.yml pull
            docker compose -f docker-compose.prod.yml up -d
            docker image prune -f
```

The `deploy` job has `needs: build-and-push`, so it only runs after every image is successfully built and pushed. It then SSHes into the server, pulls the freshly-tagged images, and restarts — fully hands-off.

### Phase 6: Production Deployment

**Purpose:** the first-time setup on a real server — prepare the environment, drop in your secrets, and bring the stack up detached (`-d`) so it keeps running after you disconnect.

```bash
# SSH into server
ssh user@your-server

# First deployment
mkdir -p /app
cd /app

# Create production env file on server
nano .env    # add production secrets

# Create docker-compose.prod.yml on server (or pull from git)

# Start everything
export IMAGE_TAG=1.2.0
docker compose -f docker-compose.prod.yml up -d

# Verify
docker compose -f docker-compose.prod.yml ps
docker compose -f docker-compose.prod.yml logs -f
```

### Phase 7: Updating in Production

**Purpose:** ship a new version with near-zero downtime — build/push the new image, then pull and restart only the changed service, leaving the database (and everything else) untouched.

```bash
# On your machine — build and push new version
docker build -t yourregistry/backend-node:1.2.1 ./backend-node
docker push yourregistry/backend-node:1.2.1

# On server — pull and restart (zero-ish downtime)
export IMAGE_TAG=1.2.1
docker compose -f docker-compose.prod.yml pull backend-node
docker compose -f docker-compose.prod.yml up -d backend-node
# Only the changed service restarts. DB is unaffected.

# Rollback if something goes wrong
export IMAGE_TAG=1.2.0
docker compose -f docker-compose.prod.yml up -d backend-node
```

Because every image is version-tagged, rollback is just "point back at the old tag and restart" — no rebuild required. This is the whole payoff of immutable, versioned images.

### Phase 8: Monitoring & Maintenance

**Purpose:** keep the running system healthy — check status, watch resource usage, reclaim disk space, and back up your data before it's ever at risk.

```bash
# Check all container status and health
docker compose ps

# Watch resource usage
docker stats

# Check disk usage
docker system df

# Clean up old images and stopped containers
docker system prune                  # removes stopped containers, unused networks, dangling images
docker system prune -a               # also removes unused images
docker image prune                   # only dangling images
docker volume prune                  # removes unused volumes (careful! — this can delete data)

# Backup database volume
docker exec postgres pg_dump -U appuser appdb > backup.sql
```

---

---

[← Back to index](../Docker.md)
