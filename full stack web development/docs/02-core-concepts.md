# Docker Master Cheatsheet — Part 2 — Core Concepts

[← Back to index](../Docker.md)

---

## 6. Ports

You started a container, your app says it's listening on port 3000, you open `localhost:3000` in your browser — and nothing. This is the number-one beginner frustration, and the fix is understanding ports.

Containers run in isolation. Even if your app happily listens on a port *inside* the container, that port is sealed off from the outside world. Nothing on your machine can reach it until you explicitly build a bridge from a port on your host to the port inside the container. That bridge is **port mapping**.

### Port Mapping Syntax

The `-p` flag creates the bridge. It always reads `host:container` — your machine's port on the **left**, the container's internal port on the **right**:

```bash
docker run -p <host-port>:<container-port> my-app
#              ^               ^
#              your machine    inside container
```

The host port is the one *you* type into your browser. The container port must match whatever your app actually listens on inside:

```bash
docker run -p 8080:3000 my-app   # reach app at localhost:8080
docker run -p 3000:3000 my-app   # reach app at localhost:3000
docker run -p 9000:3000 my-app   # reach app at localhost:9000
```

Notice the container port stays `3000` in all three (that's fixed by the app), while the host port is *your* choice. `localhost:8080` reaches an app listening on `3000` inside — the mapping does the translation.

**The classic mistake:** getting the order backwards. `-p 3000:8080` maps host 3000 to container 8080, so if your app listens on 3000 inside, nothing works. Remember: **host on the left, container on the right.**

### Multiple Containers, Same Image

Want to run three copies of the same app? Each container has its own isolated internal port `3000`, so those don't clash — but they must each map to a **different host port**, because your host only has one port 3000:

```bash
docker run -p 3001:3000 my-app   # instance 1 → localhost:3001
docker run -p 3002:3000 my-app   # instance 2 → localhost:3002
docker run -p 3003:3000 my-app   # instance 3 → localhost:3003
```

Container ports don't conflict — they're isolated. Only host ports must be unique. (Try to map two containers to the same host port and the second one fails with a "port is already allocated" error.)

### EXPOSE vs -p

This is the confusion that catches nearly everyone. `EXPOSE` *sounds* like it opens a port. It does not.

```dockerfile
EXPOSE 3000   # documentation only — tells humans which port the app uses
              # does NOT publish or open anything
```

```bash
docker run -p 3000:3000 my-app   # THIS is what actually publishes the port
```

`EXPOSE` is metadata: it's a note in the image saying "hey, this app uses port 3000" for other developers and tools to read. It opens nothing. `-p` is the one that actually builds the bridge and lets traffic through.

**Side by side:**

| | `EXPOSE 3000` (Dockerfile) | `-p 8080:3000` (docker run) |
|---|---|---|
| What it does | Documents the port | Actually publishes the port |
| Opens access from host? | No | Yes |
| When it runs | Build time (metadata) | Run time |
| Required to reach the app? | No | **Yes** |

EXPOSE is for humans and tools. `-p` does the real work.

---

---

## 7. Volumes

Containers are **ephemeral** — everything written inside a container is gone the moment it stops or is deleted. Restart from the same image and you get a factory-fresh filesystem. That's great for the app code (you *want* a clean, reproducible start), but catastrophic for the one thing you can't regenerate: your data. A database inside a bare container is one `docker rm` away from oblivion.

A **volume** fixes this by connecting a folder *inside* the container to a folder *outside* it, so the data lives on even after the container is gone.

### The Mental Model

Think of your machine and the container as two separate computers. A volume is a **shared folder** they both see — like a Dropbox folder synced between two laptops, except it's instant and it's the exact same bytes on disk:

```
Your Machine                     Container
────────────────────────────────────────────
/mycomputer/data        ←→       /app/data
```

They are the SAME folder:
- Container writes to `/app/data` → the bytes land on your machine.
- Container deleted → the data is still sitting on your machine, untouched.
- New container, same volume → the data is right there again.

That last point is the whole reason volumes exist: **the data outlives the container.** Kill the Postgres container, spin up a new one pointed at the same volume, and your tables are still there.

### Three Types of Volumes

**1. Named Volume** — you give it a name; Docker decides where it lives (recommended for databases).

```bash
docker run -v mydata:/var/lib/postgresql/data postgres
#              ^                ^
#              label (not a     path inside container
#              real path)
# "mydata" is just a name you invent.
# Docker stores it somewhere like /var/lib/docker/volumes/mydata/
# You don't need to know where — Docker manages it for you.
```

**2. Bind Mount** — *you* specify the exact folder on your machine.

```bash
docker run -v /mycomputer/data:/app/data my-app
docker run -v $(pwd):/app my-app    # current folder → /app
```

**3. Anonymous Volume** — no name at all. Docker manages it, but with no name it's hard to find again and hard to reuse. Avoid it.

### How to Tell the Difference

This trips everyone up, because named volumes and bind mounts use the *identical* `-v LEFT:RIGHT` syntax. The only thing that distinguishes them is what the **left side** looks like:

```bash
-v mydata:/app/data          # named volume — just a label, no slashes
-v /real/path:/app/data      # bind mount — starts with /
-v ./relative:/app/data      # bind mount — starts with .
-v $(pwd):/app               # bind mount — $(pwd) expands to a full path
```

**Rule: if the left side starts with `/` or `./`, it's a real path (bind mount). If it's just a bare word, it's a named volume.** Docker looks at that first character to decide — get it wrong and you'll accidentally create a mystery named volume instead of mounting the folder you meant.

### When to Use Which

| Type | When to use |
|---|---|
| Named volume | Databases, any data you want to persist but don't need to open in your editor |
| Bind mount | Dev mode — mount your source code so changes reflect instantly without rebuilding |

The split comes down to *who needs to read the files*. A database's data files are Docker's business, not yours — hand them to a **named volume** and forget where they live. Your source code, though, you edit constantly in your IDE, so it needs to sit at a real path you control — that's a **bind mount**.

### Volume Lifecycle: Reusing a Volume Across Containers

The line "the data outlives the container" is easy to *say* — here it is proven end to end. This is the exact workflow you reach for when you kill a database container and want the new one to pick up right where the old left off. Note the `docker volume` commands: they're how you *find and manage* a volume after its original container is gone (the earlier examples only showed how to mount one).

```bash
# 1. CREATE a container with a named volume, and write data into it
docker run -d --name c1 -v mydata:/data busybox \
  sh -c "echo 'hello from c1' > /data/note.txt && sleep 3600"

docker exec c1 cat /data/note.txt        # → hello from c1

# 2. REMOVE the container. The volume is NOT deleted — that's the whole point.
docker rm -f c1                           # container gone; "mydata" lives on

# 3. FIND the volume again — it survived the container
docker volume ls                          # lists "mydata"
docker volume inspect mydata             # shows its Mountpoint on the host

# 4. ATTACH the SAME volume to a DIFFERENT (new) container
docker run -d --name c2 -v mydata:/data busybox sleep 3600

docker exec c2 cat /data/note.txt        # → hello from c1   ← data persisted!
```

Step 4 is the payoff: `c2` is a fresh container (could even be a different image), yet the file written by the long-gone `c1` is right there. That is how a Postgres container can be destroyed and recreated without losing a single row.

**Volume management commands** — the toolkit for finding and cleaning up volumes:

```bash
docker volume ls                 # list all named volumes (find yours again)
docker volume inspect mydata     # host Mountpoint + metadata
docker volume create mydata      # pre-create explicitly (optional; -v auto-creates it)
docker volume rm mydata          # delete the volume AND its data (must be unused)
docker volume prune              # delete ALL unused volumes — careful
```

**Two gotchas that bite in this workflow:**

1. **`docker rm` never deletes a named volume** — not even `docker rm -f`. That is deliberate: your data is safe from an accidental container delete. To actually wipe it you must run `docker volume rm mydata` yourself (after no container uses it). `docker rm -fv c1` only removes *anonymous* volumes bound to the container, never named ones.
2. **Anonymous volumes can't be cleanly reattached.** They get a random hash for a name (`docker volume ls` shows something like `a1b2c3d4…`), so you have no stable handle to point the next container at. This reattach cycle basically requires a *named* volume — which is exactly why anonymous volumes are "avoid."

### Dev Mode Bind Mount

```bash
docker run -p 3000:3000 -v $(pwd):/app my-app
```

Here `$(pwd)` (your current project folder) is mounted onto `/app` inside the container. Edit a file on your machine → the container sees the change immediately, because it's literally the same file. No rebuild, no restart. This is the trick behind hot-reload development: your code lives on your machine, but *runs* inside the container.

---

---

## 8. Networks

### Default Behavior with Docker Compose

Here's the good news: for the common case, you do **nothing**. Docker Compose **automatically creates a network** for every service in the same `docker-compose.yml`, and each service is reachable by its **service name as the hostname**. No IP addresses to track, no config to write.

```yaml
services:
  app:
    build: .
  db:
    image: postgres:15
```

Inside the `app` container, you connect to the database using the service name `db` as the host:

```javascript
host: "db"    // NOT localhost — the service name
port: 5432
```

### Why Not localhost?

This is the single biggest networking gotcha for beginners: **inside a container, `localhost` means the container itself — not your machine, and not another container.**

Every container gets its own private `localhost`, the same way two separate laptops each have their own. So when `app` says "connect to localhost:5432," it's asking *itself* for a database — and there's no database running inside the app container.

```
Your Machine (localhost = your machine)
├── app container  (localhost = the app container itself)
└── db container   (localhost = the db container itself)

To reach db from app: use "db" (the service name), NOT localhost
```

**Mental model:** each container is its own island. `localhost` never leaves the island you're standing on. To reach another island, you call it by its name (`db`), and Compose's built-in DNS does the routing.

### Network Created by Compose

Compose creates a network named `<folder-name>_default` automatically (e.g. a project in a `myapp/` folder gets `myapp_default`). Every service joins it, and every service can find every other by service name. This is why the zero-config case just works.

### Custom Networks (Advanced)

By default all services share one network, so everything can talk to everything. Sometimes you *don't* want that — you want to **isolate** services so a compromised front-end can't reach the database directly. Custom networks let a service join only the networks it belongs on:

```yaml
services:
  nginx:
    networks: [frontend]          # can only talk to app
  app:
    networks: [frontend, backend] # bridges both networks
  db:
    networks: [backend]           # nginx cannot reach db directly ✅

networks:
  frontend:
  backend:
```

Two containers can only talk if they share at least one network. `nginx` is on `frontend`, `db` is on `backend`, and they share nothing — so `nginx` physically cannot reach `db`. Only `app`, sitting on both, can bridge them. That's your database firewalled off from the public-facing layer, enforced by Docker rather than by hoping nobody misconfigures a rule.

### Manual Networking (Without Compose)

If you're using plain `docker run` instead of Compose, you get the same service-name magic by creating a network yourself and attaching containers with `--name`:

```bash
docker network create mynetwork
docker run --network mynetwork --name db postgres:15
docker run --network mynetwork --name app my-app
# now app can reach db by hostname "db"
```

The `--name` you give each container becomes its hostname on that network — exactly what Compose does for you automatically using the service name.

---

---

## 9. Environment Variables

Never hardcode passwords, API keys, or environment-specific config into your code or Dockerfile. Anything baked into an image is visible to anyone who can pull that image, and it makes the same image behave identically everywhere — which defeats the point of having separate dev, staging, and production settings. **Environment variables** keep secrets and per-environment config *outside* the image, injected at run time.

### Three Ways to Set Them

**1. In `docker-compose.yml` directly** (values are visible in the file — fine for non-secret config, NOT for secrets).

```yaml
environment:
  DB_HOST: db
  NODE_ENV: production
```

**2. `.env` file** ← the most common approach.

```bash
# .env file (NEVER commit to git)
DB_PASSWORD=supersecret
API_KEY=abc123
DB_HOST=db
```

```yaml
# docker-compose.yml references them with ${...}
environment:
  DB_PASSWORD: ${DB_PASSWORD}
  API_KEY: ${API_KEY}
```

Compose automatically reads a file named `.env` from the same directory — no extra config needed. The `${DB_PASSWORD}` syntax pulls the value out of that file at run time, so the secret stays in `.env` (which git ignores) and never appears in the committed compose file.

**3. At run time** on the command line.

```bash
docker run -e DB_PASSWORD=secret -e API_KEY=abc123 my-app
```

### In Your App Code

```javascript
// Node.js
const password = process.env.DB_PASSWORD

// Python
import os
password = os.environ.get("DB_PASSWORD")
```

Your app just reads normal environment variables. It has no idea — and doesn't care — whether they came from a `.env` file, the compose file, or a `-e` flag. That's the beauty of it: the *same* code runs in dev and prod, and only the injected values change.

### The Three Files Rule

Managing secrets safely comes down to three files that always travel together — two you commit, one you never do:

```
.env          → actual secrets (never commit) ❌ git
.env.example  → template with dummy/empty values ✅ git
.gitignore    → contains ".env"                ✅ git
```

Why three? Each has a distinct job:
- **`.env`** holds the real secrets. It's in `.gitignore`, so it stays on your machine and never reaches the repo.
- **`.env.example`** is a committed *template* that documents which variables exist, so a teammate cloning the repo knows exactly what to fill in — without ever seeing a real secret.
- **`.gitignore`** is what actually enforces the rule: listing `.env` there is what stops you from accidentally committing your secrets.

`.env.example` tells teammates what variables they need to set up:

```bash
# .env.example
DB_PASSWORD=
DB_HOST=
API_KEY=
NODE_ENV=development
```

The workflow: clone the repo → copy `.env.example` to `.env` → fill in the real values. Miss one and the app fails fast with a clear "missing DB_PASSWORD" instead of a mysterious runtime bug.

### YAML List vs Map for Environment

Both syntaxes below are valid and do the exact same thing — pick one and be consistent:

```yaml
# Map style (clean, readable)
environment:
  DB_PASSWORD: secret
  NODE_ENV: production

# List style (familiar to .env users)
environment:
  - DB_PASSWORD=secret
  - NODE_ENV=production
```

The map style uses `key: value` (a colon); the list style uses `- KEY=value` (a dash and an equals sign, mirroring how a `.env` file looks). Mixing the two under one `environment:` block is invalid YAML, so commit to one style per file.

---

---

[← Back to index](../Docker.md)
