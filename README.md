# PayLink

![CI](https://github.com/ZyadRR/paylink/actions/workflows/ci.yml/badge.svg)

Bill-payment backend for the Year Work Project (electricity, water, mobile top-ups and internet bills in Egypt).

## Tech stack

- NestJS (TypeScript)
- PostgreSQL 16
- Docker and Docker Compose
- GitHub Actions for CI

## Run the project

You only need **Docker Desktop** and **Git**.

```bash
git clone https://github.com/ZyadRR/paylink.git
cd paylink
docker compose up --build
```

Then open http://localhost:3000. You should see `Hello World!`.

Stop it with `Ctrl + C`, and remove the containers and database volume with:

```bash
docker compose down -v
```

## Run tests and lint (without Docker)

Requires Node.js 22 or newer.

```bash
cd backend
npm ci
npm run lint
npm run test:cov
```

CI fails if line coverage drops below 70% (NFR-6).

## Project structure

```
backend/              NestJS application
docs/                 Requirements, diagrams and the AI prompt log
.github/workflows/    CI pipeline (lint, test with coverage, build, Docker build)
docker-compose.yml    App and PostgreSQL
```

## Environment variables

Copy `.env.example` to `.env` for local settings. Never commit `.env`.

## Team workflow

See [CONTRIBUTING.md](CONTRIBUTING.md). In short: no direct pushes to `main`, every change goes through a pull request with one approval and a passing CI check.
