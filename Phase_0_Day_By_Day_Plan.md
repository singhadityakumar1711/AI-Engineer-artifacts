# Phase 0 — Day-by-Day Plan
### Foundations Refresh: Python, FastAPI, Docker, Testing, CI
**Duration:** 2 weeks | **Weekday:** 2 hrs/day | **Weekend:** 6-8 hrs/day

---

## Week 1 — Python + FastAPI Foundations

### Day 1 (Mon, 2h) — Modern Python Type Hints & Project Structure
- **Read:**
  - [Python typing docs](https://docs.python.org/3/library/typing.html) (skim)
  - [Real Python: Type Checking](https://realpython.com/python-type-checking/)
- **Practice:**
  - Add full type hints to 2-3 old scripts from your data engineering work
  - Set up a project with `uv` (or `poetry`) instead of raw pip

### Day 2 (Tue, 2h) — Pydantic v2 Deep Dive
- **Read:**
  - [Pydantic v2 docs — Models](https://docs.pydantic.dev/latest/concepts/models/)
  - [Pydantic v2 docs — Validators](https://docs.pydantic.dev/latest/concepts/validators/)
- **Practice:**
  - Model a messy real-world JSON payload (e.g., a sample API response) with validation, defaults, and custom validators

### Day 3 (Wed, 2h) — Async/Await Fundamentals
- **Read:**
  - [Real Python: Async IO in Python](https://realpython.com/async-io-python/)
- **Practice:**
  - Write an async script that calls 3 mock/public APIs concurrently with `asyncio.gather`
  - Compare timing vs sequential calls

### Day 4 (Thu, 2h) — Git Workflow + Conventional Commits
- **Read:**
  - [Conventional Commits spec](https://www.conventionalcommits.org/en/v1.0.0/)
  - [GitHub Flow guide](https://docs.github.com/en/get-started/using-github/github-flow)
- **Practice:**
  - Set up a repo, practice feature-branch → PR → merge on a throwaway project

### Day 5 (Fri, 2h) — FastAPI Basics
- **Read:**
  - [FastAPI official tutorial](https://fastapi.tiangolo.com/tutorial/) — path/query params, request bodies, response models
- **Practice:**
  - Build a 3-endpoint CRUD API (in-memory storage is fine for now)

### Day 6 (Sat, 6-8h) — FastAPI Depth + Project Skeleton
- **Read:**
  - [FastAPI — Dependency Injection](https://fastapi.tiangolo.com/tutorial/dependencies/)
  - [FastAPI — Bigger Applications structure](https://fastapi.tiangolo.com/tutorial/bigger-applications/)
  - [FastAPI — Settings/pydantic-settings](https://fastapi.tiangolo.com/advanced/settings/)
- **Practice:**
  - Restructure your CRUD API into a proper multi-file layout (`routers/`, `services/`, `schemas/`, `core/config.py`)
  - This becomes the skeleton of your starter kit

### Day 7 (Sun, 6-8h) — Docker Fundamentals
- **Read:**
  - [Docker Get Started guide](https://docs.docker.com/get-started/)
  - [Docker Compose docs](https://docs.docker.com/compose/)
- **Practice:**
  - Write a multi-stage Dockerfile for your FastAPI app
  - Write a `docker-compose.yml` that also spins up Postgres and Redis containers alongside it

---

## Week 2 — Testing, CI, and Assembly

### Day 8 (Mon, 2h) — Pytest Basics
- **Read:**
  - [pytest — Getting Started](https://docs.pytest.org/en/stable/getting-started.html)
  - [pytest — Fixtures](https://docs.pytest.org/en/stable/how-to/fixtures.html)
- **Practice:**
  - Write unit tests for your CRUD endpoints using `TestClient`

### Day 9 (Tue, 2h) — Mocking External Calls
- **Read:**
  - [pytest-mock docs](https://pytest-mock.readthedocs.io/en/latest/)
  - [respx docs](https://lundberg.github.io/respx/) (for mocking httpx calls — needed later for mocking LLM API calls)
- **Practice:**
  - Mock a fake external API call in one of your endpoints
  - Test both success and failure paths

### Day 10 (Wed, 2h) — Linting + Pre-commit
- **Read:**
  - [Ruff docs](https://docs.astral.sh/ruff/)
  - [pre-commit.com](https://pre-commit.com/)
- **Practice:**
  - Add a `.pre-commit-config.yaml` with ruff (lint + format) and mypy running on every commit

### Day 11 (Thu, 2h) — GitHub Actions CI
- **Read:**
  - [GitHub Actions — Building and testing Python](https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-python)
- **Practice:**
  - Write a workflow that runs lint + tests on every push/PR

### Day 12 (Fri, 2h) — Postgres + Redis Integration
- **Read:**
  - [SQLAlchemy 2.0 — ORM Quickstart](https://docs.sqlalchemy.org/en/20/orm/quickstart.html) (or [asyncpg docs](https://magicstack.github.io/asyncpg/current/) to stay fully async)
  - [redis-py docs](https://redis-py.readthedocs.io/en/stable/)
- **Practice:**
  - Add a real Postgres-backed model (replace in-memory storage)
  - Add a simple Redis cache-check pattern

### Day 13 (Sat, 6-8h) — Assemble the Starter Kit
- **Read:** No new reading — integration day
- **Practice:**
  - Combine everything (FastAPI structure, Docker Compose, pydantic-settings, Postgres, Redis, pytest, pre-commit) into one clean repo template

### Day 14 (Sun, 6-8h) — CI Finalize + Logging + Polish
- **Read:**
  - [structlog docs](https://www.structlog.org/en/stable/) (quick skim)
  - [OpenTelemetry Python — Getting Started](https://opentelemetry.io/docs/languages/python/getting-started/) (stretch, just enough to scaffold)
- **Practice:**
  - Add structured logging
  - Finalize the GitHub Actions pipeline (green build required)
  - Write the README, push publicly

---

## Frontend & Backend Stack to Brush Up

### Backend — Main Investment

| Tool | Why |
|---|---|
| **Python + FastAPI** | Non-negotiable — the backbone of every project ahead |
| **SQLAlchemy (async) or asyncpg** | For Postgres access |
| **Redis client (redis-py)** | For caching/queues later |

> Skip Flask/Django entirely — FastAPI's async-native design and pydantic integration is what the whole roadmap is built around.

### Frontend — Deliberately Minimal

This is an AI-engineering track, not a full-stack one, so frontend investment stays light.

| Tool | When to Use | Time Investment |
|---|---|---|
| **Streamlit or Gradio** | Default for ~90% of demo dashboards and eval reports (Phases 1-3). Fast, Python-only, zero JS needed. | ~Half a day to get comfortable with basics |
| **React + TypeScript (lightly) + Tailwind** | Only for flagship projects needing a "real product" look (Phase 2's eval dashboard, Phase 4's incident/debugger UI) | ~Half a day refresher on function components + hooks + Tailwind, since you already have prior experience — don't relearn from scratch |

**Skip:** Next.js, state management libraries (Redux/Zustand), and any CSS framework beyond Tailwind — none of these add signal for an AI Engineer portfolio, and the time is better spent on Phases 1-6.

---

*Check off each day as you complete it. If a day's practice runs long, let it — Phase 0 is infrastructure you'll reuse for the next 8 months, so it's worth getting right rather than rushing.*
