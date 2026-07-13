# Docker Master Cheatsheet — Part 1 — Fundamentals

[← Back to index](../Docker.md)

---

## 1. The Problem Docker Solves

Before you learn *how* Docker works, it helps to feel the pain it removes. Docker exists to answer one frustrating question that has cost developers millions of hours: *"Why does my code run perfectly on my laptop but break the moment it lands on the server?"*

### The "Works on My Machine" Problem

Your code never runs in a vacuum. It quietly depends on the exact world around it — the language version, the OS libraries, the system config. Before Docker, deploying software meant praying that the server had:

- The same language runtime version as your laptop
- The same OS libraries
- The same system configuration
- The same environment variables

They rarely matched. A server running Node 16 instead of Node 18, or an older `libssl`, is enough to turn "it works" into a crash log. Teams wasted days chasing these ghosts — bugs that only appeared in one environment and vanished in another.

### Docker's Solution

The fix is simple to state: stop shipping just your code and hoping the destination is set up correctly. Instead, package your app **together with everything it needs** — runtime, libraries, config, OS files — into one portable, self-contained unit called a **container**. Wherever that container runs, it carries its own world with it, so the environment is identical every time.

```
Without Docker:
  Dev laptop:  Node 18, Ubuntu 22, libssl 1.1  ✅ works
  Server:      Node 16, CentOS 7, libssl 1.0   ❌ crashes

With Docker:
  Dev laptop:  [container: Node 18, Ubuntu 22, libssl 1.1]  ✅ works
  Server:      [container: Node 18, Ubuntu 22, libssl 1.1]  ✅ identical
```

Notice what changed: in the "with Docker" world, the laptop and the server are no longer running *their* environments — they're both running the *same* bundled environment. The host machine barely matters anymore.

> **Analogy:** Instead of giving someone a recipe and hoping they have all the ingredients, you hand them a sealed lunchbox with the meal already made. It doesn't matter what's in their kitchen — the meal is complete before it leaves yours.

### What Docker Is NOT

Docker gets mythologized, so it's worth clearing up what it *isn't*:

- **Not a virtual machine.** VMs virtualize an entire machine down to fake hardware and boot a full guest OS on top — heavy and slow. Containers skip all that: they share the host's OS kernel and just isolate the process. That's why a container starts in milliseconds while a VM takes minutes.
- **Not just for deployment.** It's equally great for local dev (spin up a database in one command), CI/CD, and testing against production-identical setups.
- **Not magic.** Under the hood it's just two ordinary Linux features — process isolation (namespaces/cgroups) + filesystem layering. No sorcery, just clever use of the kernel.

---

---

## 2. Images vs Containers

These two words trip everyone up, and getting them straight early saves endless confusion later. Here's the clearest mental model: an **image** is the frozen blueprint; a **container** is a living thing created *from* that blueprint.

|  | Image | Container |
|--------|--------|-----------|
| **What it is** | Blueprint (static, read-only) | Running instance of a blueprint |
| **Lives on** | Disk | Memory + disk |
| **Does anything?** | No — just a file | Yes — a live process |
| **Analogy** | Recipe | The actual dish |
| **Analogy** | Class | Object instance |
| **Analogy** | .exe file | Running program |

The single arrow that connects them is `docker run` — it takes a static image off disk and brings a container to life:

```
Image (blueprint)
    ↓ docker run
Container (running instance)
```

Because the image is just a read-only template, you can stamp out **multiple containers from the same image** simultaneously — the same way one class can create many independent objects:

```bash
docker run nginx   # container 1
docker run nginx   # container 2
docker run nginx   # container 3
```

All three are completely independent processes — each with its own memory, its own writable layer, its own lifecycle. One crashing doesn't touch the others, and **the image itself never changes** no matter how many containers you launch from it or what those containers do internally.

**Key rule: You build/pull images. You run/stop containers.** If a verb is about creating or fetching a blueprint, it acts on an image; if it's about starting, stopping, or inspecting something *alive*, it acts on a container. Keep that split straight and most Docker commands become obvious.

---

---

## 3. Dockerfile Deep Dive

If an image is a blueprint, the **Dockerfile** is the set of build instructions that produces it. It's a plain text file with step-by-step instructions Docker follows, top to bottom, to assemble an image. The file must be named exactly `Dockerfile` with no extension — that's the default name Docker looks for when you run `docker build`.

Think of it as a recipe written down: each line is one step, Docker executes them in order, and the finished image is the result you can run anywhere.

### Every Instruction Explained

Here is a complete, minimal Dockerfile for a Node app. Read it top to bottom the way Docker does — each instruction builds on the state left by the one above it:

```dockerfile
# Base image to start from — don't build from scratch
FROM node:18

# Set working directory inside the container
# All subsequent commands run from here
WORKDIR /app

# Copy specific file first (caching strategy — explained in section 4)
COPY package.json .

# RUN executes at BUILD TIME — baked into the image
RUN npm install

# Copy everything else (after install, so install is cached)
COPY . .

# Document which port the app uses (informational only — does NOT publish)
EXPOSE 3000

# CMD executes at RUN TIME — when container starts
CMD ["node", "server.js"]
```

Walking through it: `FROM` picks a starting point that already has Node installed, so you inherit a working runtime instead of building an OS from zero. `WORKDIR` sets the folder every later command operates in (and creates it if needed). Then comes the deliberate ordering — copy `package.json` alone, install dependencies, *then* copy the rest of the code. That order isn't cosmetic; it's a caching trick that makes rebuilds dramatically faster (the full story is in section 4). `EXPOSE` merely documents the port for humans and tools — it does **not** actually open it. And `CMD` is the one line that runs when the finished container *starts*, not when it's built.

The crucial distinction hiding in here: **`RUN` happens once, at build time, and its result is frozen into the image. `CMD` happens every time you start a container from that image.** Mixing these up is one of the most common beginner mistakes.

### COPY Syntax Explained

`COPY` moves files from your machine into the image. It always takes two arguments — source first, destination second:

```dockerfile
COPY <source-on-your-machine> <destination-inside-container>
COPY package.json .         # copy one file to WORKDIR
COPY package.json /app/     # copy one file to explicit path
COPY . .                    # copy EVERYTHING from current folder to WORKDIR
COPY src/ /app/src/         # copy a folder
```

The line that confuses everyone is `COPY . .` — two dots that mean two completely different things depending on which side of the space they're on:

- **First `.`** = your entire current project folder *on your machine* (the build context).
- **Second `.`** = the current `WORKDIR` *inside the container* (here, `/app`).

So `COPY . .` reads as "take everything from my project folder and drop it into `/app` inside the image." Same character, two different worlds.

### RUN vs CMD vs ENTRYPOINT

These three instructions all "run something," which is exactly why they get muddled. Here's the quick reference:

| Instruction | When it runs | Use case |
|---|---|---|
| RUN | At build time | Install packages, compile code, set up files |
| CMD | At run time (default, overridable) | Start your app |
| ENTRYPOINT | At run time (fixed, not easily overridden) | Make container behave like a binary |

`RUN` is the easy one — it's the odd one out that fires at *build* time to shape the image. The real confusion is between `CMD` and `ENTRYPOINT`, which both fire at *run* time. Getting them straight is worth doing carefully, because it decides what actually happens when someone types `docker run`.

When you run `docker run my-app`, the container needs to execute ONE command. That final command is built by joining **ENTRYPOINT + CMD** together:

```
what actually runs  =  ENTRYPOINT  +  CMD
```

The trick is *which part* your `docker run` arguments replace:
- Anything you type after the image name (`docker run my-app ...`) **replaces CMD entirely**.
- It does **NOT** replace ENTRYPOINT.

That single rule explains everything.

**Case 1 — CMD only (no ENTRYPOINT).** CMD is the whole command, and your args replace it:

```dockerfile
CMD ["node", "server.js"]
```
```bash
docker run my-app                 # runs: node server.js   (default)
docker run my-app node other.js   # runs: node other.js    (your args REPLACED the whole CMD)
```

**Case 2 — ENTRYPOINT + CMD together.** ENTRYPOINT is fixed; CMD is a replaceable default arg:

```dockerfile
ENTRYPOINT ["node"]      # fixed part — always runs
CMD ["server.js"]        # default argument — replaceable
```
```bash
docker run my-app              # runs: node server.js   (uses default CMD)
docker run my-app other.js     # runs: node other.js    ("other.js" replaced CMD; "node" stayed)
```

You typed only `other.js`, but it ran `node other.js` — because `node` came from ENTRYPOINT (locked in) and `other.js` replaced the CMD default.

**Mental model:** ENTRYPOINT is written in ink (permanent); CMD is written in pencil (your `docker run` args erase and rewrite it).

**Decision:** use CMD-only for normal apps (so `docker run my-app bash` can drop you into a shell for debugging); use ENTRYPOINT+CMD when the container should behave like one dedicated tool. Escape hatch: `docker run --entrypoint bash my-app` overrides ENTRYPOINT too.

### Multiple Dockerfiles

Docker looks for a file literally named `Dockerfile` by default. But real projects often need different builds for different environments — a lean production image, a dev image with hot-reload tooling, a test image with extra fixtures. The convention is to suffix the extra files:

```
my-app/
  Dockerfile           # default (production)
  Dockerfile.dev       # development
  Dockerfile.test      # testing
```

Since only the bare `Dockerfile` is picked up automatically, you point Docker at the others explicitly with the `-f` (file) flag:

```bash
docker build -t my-app -f Dockerfile.dev .
docker build -t my-app -f Dockerfile.prod .
```

One detail here trips people up: the `.` at the end is **not** the Dockerfile — it's the **build context**, the folder Docker hands to the build as the source for every `COPY` command. The `-f` flag says *which recipe to use*; the trailing `.` says *which folder to copy files from*. They're independent, which is why you can keep your Dockerfiles anywhere and still build against the project root.

---

---

## 4. Layers & Caching

Here's the single most useful thing to understand about why Docker builds are sometimes instant and sometimes take five minutes: **layers**.

Every instruction in a Dockerfile (`FROM`, `COPY`, `RUN`, ...) creates one **layer** — a saved snapshot of the filesystem after that step. An image is just a stack of these layers, one on top of the next.

```dockerfile
FROM node:18             # layer 1 — base image layers
WORKDIR /app             # layer 2
COPY package.json .      # layer 3
RUN npm install          # layer 4 — heaviest layer
COPY . .                 # layer 5
CMD ["node","server.js"] # layer 6
```

**Mental model:** think of it like a stack of transparent sheets. Each instruction draws on a new sheet laid over the previous ones. The final image is everything you see looking down through the whole stack.

### How Caching Works

Docker caches each layer after it builds it. When you rebuild, Docker walks down the stack and reuses a cached layer as long as nothing about it (or anything before it) changed. The moment it hits a layer that **did** change, it re-runs that layer **and everything after it** — because every later layer was built on top of the one that just changed, so they can't be trusted anymore.

```
Change in layer 3 (package.json) → re-run layers 3, 4, 5, 6
Change in layer 5 (your code)    → re-run layers 5, 6
                                    layers 1-4 are CACHED ✅
```

The key insight: **the cache only survives up to your first change.** Everything below the change point is thrown away and rebuilt.

### Why Order Matters

This is where a tiny reordering turns a slow build into a fast one. Since a changed layer invalidates everything after it, you want your **expensive, rarely-changing** steps to sit *above* your **cheap, frequently-changing** steps.

Your source code changes constantly (every edit). Your dependencies (`package.json`) change rarely. `npm install` is expensive. So the question is: does an expensive install sit above or below your constantly-changing code?

**Bad order (slow builds):**

```dockerfile
COPY . .              # layer 3 — changes on every code edit
RUN npm install       # layer 4 — re-runs every time! Slow ❌
```

Here, editing a single line of code changes layer 3, which invalidates layer 4 — so `npm install` re-runs on *every* build, even when you didn't touch a single dependency.

**Good order (fast builds):**

```dockerfile
COPY package.json .   # layer 3 — only changes when deps change
RUN npm install       # layer 4 — cached unless package.json changes ✅
COPY . .              # layer 5 — changes often, but no expensive step after
```

Now `npm install` only re-runs when `package.json` actually changes. Editing your code invalidates only layer 5 (a cheap file copy) and everything after it — which is basically nothing expensive. Builds go from minutes to seconds.

**This trips everyone up:** the "copy everything, then install" order feels natural, but it's the classic cache-killer. Copy your dependency manifest *first*, install, *then* copy the rest of your code.

**Rule of thumb: Put things that change less frequently higher up. Things that change often go near the bottom.**

### Layer Size Tips

Every `RUN` creates a new layer — and every layer is permanent. If one `RUN` downloads a file and a *later* `RUN` deletes it, the file still lives in the earlier layer, bloating your image. Deleting in a later layer doesn't shrink the earlier one; the bytes are already baked in.

The fix: do related work — including cleanup — in a **single** `RUN`, chained with `&&`, so the temporary files never get committed to a layer at all.

```dockerfile
# Bad — 3 layers, intermediate files persist
RUN apt-get update
RUN apt-get install -y curl
RUN rm -rf /var/lib/apt/lists/*

# Good — 1 layer, cleanup in same command
RUN apt-get update && \
    apt-get install -y curl && \
    rm -rf /var/lib/apt/lists/*
```

In the "good" version, the `apt` package lists are created and deleted within the same layer, so the final layer never contains them. Same result, smaller image.

---

---

## 5. Docker Hub & Registries

You've been using Docker Hub this whole time without realizing it. Every `FROM node:18` quietly downloads that image from somewhere — this section is about *where*.

### What is Docker Hub?

Docker Hub (`hub.docker.com`) is a public **registry** — think GitHub, but for Docker images instead of source code. It's the default place Docker looks. When you write `FROM node:18`, Docker fetches it from Docker Hub automatically (and caches it locally so the next build is instant).

### Image Naming Convention

An image name packs several pieces of information into one string. Reading it left-to-right tells you *where* it lives and *which version* you want:

```
node:18
 ^    ^
 |    tag (version)
 name

yourname/my-app:1.0
    ^        ^    ^
    |        |    version tag
    |        image name
    Docker Hub username

registry.example.com/yourname/my-app:1.0
         ^                              ^
         custom registry URL           tag
```

The pattern is `[registry/]username/image:tag`. Official images (like `node`) skip the username; your own images include it; and if you leave off the registry, Docker assumes Docker Hub. The `tag` after the colon picks the version — leave it off and Docker silently assumes `:latest` (which, as you'll see below, you should almost never rely on).

### Common Base Images

Most images come in a few flavors: a "full" version with everything, and a slimmed-down version (`-alpine` or `-slim`) that strips out extras. The size difference is dramatic — and smaller images build, push, and pull faster.

| Image | Use case | Size |
|---|---|---|
| node:18 | Full Node.js | ~950MB |
| node:18-alpine | Lightweight Node.js | ~170MB |
| python:3.11 | Full Python | ~900MB |
| python:3.11-slim | Slim Python | ~130MB |
| nginx:alpine | Serve static files | ~40MB |
| postgres:15 | PostgreSQL database | ~380MB |
| ubuntu:22.04 | Full Ubuntu | ~77MB |
| alpine:3.18 | Minimal Linux | ~7MB |

Notice `node:18-alpine` is roughly **5x smaller** than `node:18`. The catch with Alpine: it uses `musl` instead of `glibc` and a minimal package set, so occasionally a native dependency won't compile. Start with the slim/alpine variant; fall back to the full image only if something breaks.

> **Always pin versions.** latest can break your build when a new version drops.
> Use node:18 not node:latest. Use postgres:15 not postgres:latest.

**Why pinning matters:** `:latest` isn't a fixed version — it's a moving pointer to whatever the maintainer most recently pushed. A build that worked yesterday can break today because `node:latest` silently jumped a major version overnight. Pinning to `node:18` means "give me the same base every single time," which is exactly what reproducible builds require.

### Push Your Own Image

Once you've built an image, you can share it by pushing it to a registry. The tag you build with (`yourname/my-app:1.0`) tells Docker *where* to push:

```bash
docker login                          # authenticate once
docker build -t yourname/my-app:1.0 . # build with full tag
docker push yourname/my-app:1.0       # push to Hub
docker pull yourname/my-app:1.0       # anyone can pull now
```

The `yourname/` prefix in the tag is what routes the push to *your* Docker Hub account — you can't push to an image name you don't own. Once pushed, anyone (or your production server) can `docker pull` it from anywhere.

### Private Registries

Docker Hub is public by default. In production, you'll usually push to a **private** registry so your images stay internal. The only thing that changes is the registry prefix on the tag — the `push`/`pull` workflow is identical:

- **AWS ECR**: `123456789.dkr.ecr.us-east-1.amazonaws.com/my-app:1.0`
- **Google Artifact Registry**: `us-docker.pkg.dev/project/repo/my-app:1.0`
- **GitHub Container Registry**: `ghcr.io/yourname/my-app:1.0`

---

---

[← Back to index](../Docker.md)
