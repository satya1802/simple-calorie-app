# Developer Guide

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start status -->
_Up to date._
<!-- sdlc:auto:end status -->

<!-- sdlc:auto:start source -->
Generated from commit `2d28e19` · model `claude-sonnet-5`
<!-- sdlc:auto:end source -->

<!-- sdlc:auto:start overview -->
Simple calorie app is a Quorq-generated project containing two independent applications in one repo. The frontend (frontend/) is an approved Vite + React 19 SPA with all screens built against seeded sample data, not yet wired to a real API. The backend (backend/) is a FastAPI 0.115 service scaffolded before any architecture was approved, so it currently only exposes a /health check; it uses SQLite locally by default (via SQLAlchemy) and is designed to be pointed at Postgres/Meilisearch (available via docker-compose) once real architecture is added.
<!-- sdlc:auto:end overview -->

<!-- sdlc:auto:start prerequisites -->
- Node.js (for Vite 6 / React 19 / Vitest, per frontend/package.json)
- npm
- Python 3.12 (matches backend/Dockerfile base image)
- pip
- Docker and Docker Compose (optional, for the Postgres and Meilisearch services in docker-compose.yml)
<!-- sdlc:auto:end prerequisites -->

<!-- sdlc:auto:start setup -->
1. git clone <repo> && cd <repo>
2. cd backend && pip install -r requirements.txt
3. cd ../frontend && npm install
4. (optional) from the repo root, run `docker compose up -d` to start Postgres and Meilisearch for future architecture work
5. Start the backend: cd backend && uvicorn app.main:app --reload
6. Start the frontend: cd frontend && npm run dev
<!-- sdlc:auto:end setup -->

<!-- sdlc:auto:start run -->
| Name | Command | Description |
|---|---|---|
| backend dev | `cd backend && uvicorn app.main:app --reload` | Runs the FastAPI service with autoreload; creates SQLite tables on startup and serves /health |
| frontend dev | `cd frontend && npm run dev` | Starts the Vite dev server for the React SPA |
| frontend build | `cd frontend && npm run build` | Bundles the app with Vite (does not typecheck) |
| frontend preview | `cd frontend && npm run preview` | Serves the production build locally |
| docker services | `docker compose up -d` | Starts Postgres 16 and Meilisearch containers defined in docker-compose.yml |
<!-- sdlc:auto:end run -->

<!-- sdlc:auto:start structure -->
| Path | Purpose |
|---|---|
| `backend/app` | FastAPI application: main.py (entrypoint, CORS, health route), database.py (engine/session), models.py (SQLAlchemy models, currently empty), schemas.py (Pydantic schemas incl. StubResponse), services.py, routers/ (one router per architecture component, currently empty) |
| `backend/tests` | Pytest test suite (test_health.py) |
| `frontend/src` | React SPA source: main.tsx entrypoint, lib/ (api.ts client, ui.tsx, icons.tsx, navigate.ts), index.css, test-setup.ts |
| `frontend/e2e` | Playwright end-to-end specs |
| `.quorq` | Quorq platform metadata (handoff.json) |
| `docker-compose.yml / .env.example` | Local infrastructure (Postgres, Meilisearch) and environment variable template at the repo root |
<!-- sdlc:auto:end structure -->

<!-- sdlc:auto:start conventions -->
- Backend routers are organized one per approved architecture component (backend/app/routers), with one route per API-spec endpoint
- Unimplemented backend routes return a typed StubResponse (schemas.py) rather than failing, keeping the service and its OpenAPI doc complete
- All SQLAlchemy models inherit from the shared Base in database.py; models.py is imported in main.py before Base.metadata.create_all
- Backend config is environment-driven: DATABASE_URL (defaults to local SQLite) and ALLOWED_ORIGINS (comma-separated, defaults to the Vite dev server origins)
- Frontend API access is centralized in src/lib/api.ts, using VITE_API_BASE_URL and VITE_API_SECRET_KEY env vars for the base URL and bearer auth
- Frontend uses the "@/" path alias (vite.config.ts, tsconfig.json) mapped to src/
- Frontend component tests live beside the components they test and are scoped to src/**/*.{test,spec}.{ts,tsx}; Playwright specs live separately in e2e/
<!-- sdlc:auto:end conventions -->

<!-- sdlc:auto:start testing -->
| Name | Command | Description |
|---|---|---|
| backend tests | `cd backend && pytest` | Runs the pytest suite (pythonpath set via pytest.ini) |
| backend lint | `cd backend && ruff check .` | Lints the backend with Ruff |
| backend format | `cd backend && ruff format .` | Formats backend code with Ruff |
| frontend unit tests | `cd frontend && npm test` | Runs Vitest component tests |
| frontend lint | `cd frontend && npm run lint` | Runs ESLint across the frontend app |
| frontend typecheck | `cd frontend && npm run typecheck` | Runs tsc --noEmit (not run by the build itself) |
| frontend e2e install | `cd frontend && npm run test:e2e:install` | Installs the Chromium browser needed for Playwright |
| frontend e2e | `cd frontend && npm run test:e2e` | Runs Playwright acceptance tests from e2e/, booting backend and frontend locally unless E2E_BASE_URL is set |
<!-- sdlc:auto:end testing -->

<!-- sdlc:auto:start troubleshooting -->
| Problem | Solution |
|---|---|
| Frontend requests to the backend are blocked by CORS | Set the ALLOWED_ORIGINS environment variable (comma-separated) on the backend to include the frontend's origin; it defaults to http://localhost:5173 and http://127.0.0.1:5173 |
| `pytest` fails with ImportError for `app` | Run pytest from the backend/ directory so pytest.ini's `pythonpath = .` setting applies |
| Backend has no persistent/shared database | It uses local SQLite (app.db) by default; set DATABASE_URL to point at the Postgres instance started via `docker compose up -d` instead |
| `npm run build` succeeds but the app has type errors | Build does not run tsc; run `npm run typecheck` separately to catch TypeScript errors before deploying |
| Playwright e2e tests fail to start | Run `npm run test:e2e:install` first to fetch the Chromium browser; without E2E_BASE_URL set, Playwright will install deps, build, and serve both backend and frontend itself |
<!-- sdlc:auto:end troubleshooting -->
