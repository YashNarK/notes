# Docker Master Cheatsheet

## Complete Reference: From Zero to Production

**Stack: Next.js · Node.js Express (REST) · Python FastAPI (GraphQL) · PostgreSQL · BigQuery**

> These notes are split across the files in [`docs/`](docs/) for readability. Use the table of contents below — each entry links straight to its section.

---

## Table of Contents

### [Part 1 — Fundamentals](docs/01-fundamentals.md)

1. [The Problem Docker Solves](docs/01-fundamentals.md#1-the-problem-docker-solves)

2. [Images vs Containers](docs/01-fundamentals.md#2-images-vs-containers)

3. [Dockerfile Deep Dive](docs/01-fundamentals.md#3-dockerfile-deep-dive)

4. [Layers & Caching](docs/01-fundamentals.md#4-layers--caching)

5. [Docker Hub & Registries](docs/01-fundamentals.md#5-docker-hub--registries)

### [Part 2 — Core Concepts](docs/02-core-concepts.md)

6. [Ports](docs/02-core-concepts.md#6-ports)

7. [Volumes](docs/02-core-concepts.md#7-volumes)

8. [Networks](docs/02-core-concepts.md#8-networks)

9. [Environment Variables](docs/02-core-concepts.md#9-environment-variables)

### [Part 3 — Compose & Operations](docs/03-compose-and-operations.md)

10. [Docker Compose](docs/03-compose-and-operations.md#10-docker-compose)

11. [Health Checks](docs/03-compose-and-operations.md#11-health-checks)

12. [Logs & Debugging](docs/03-compose-and-operations.md#12-logs--debugging)

### [Part 4 — Builds & Optimization](docs/04-builds-and-optimization.md)

13. [Multi-Stage Builds](docs/04-builds-and-optimization.md#13-multi-stage-builds)

14. [.dockerignore](docs/04-builds-and-optimization.md#14-dockerignore)

### [Part 5 — Full-Stack Reference](docs/05-fullstack-reference.md)

15. [Our Full Architecture](docs/05-fullstack-reference.md#15-our-full-architecture)

16. [All Dockerfiles](docs/05-fullstack-reference.md#16-all-dockerfiles)

17. [Docker Compose Files](docs/05-fullstack-reference.md#17-docker-compose-files)

### [Part 6 — SDLC & Workflow](docs/06-sdlc-workflow.md)

18. [SDLC Approach](docs/06-sdlc-workflow.md#18-sdlc-approach)

### [Part 7 — Reference & Q&A](docs/07-reference-and-qa.md)

19. [Essential Commands](docs/07-reference-and-qa.md#19-essential-commands)

20. [Common Q&A](docs/07-reference-and-qa.md#20-common-qa)

21. [Scenarios & Troubleshooting](docs/07-reference-and-qa.md#21-scenarios--troubleshooting)

22. [Interview Questions](docs/07-reference-and-qa.md#22-interview-questions)

23. [Best Practices & Modern Patterns](docs/07-reference-and-qa.md#23-best-practices--modern-patterns)

---

Built with: Docker 25+, Compose v2, Node 18, Python 3.11, Next.js 14, FastAPI 0.104, PostgreSQL 15
