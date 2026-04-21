# Technology Stack

**Analysis Date:** 2026-04-21

## Languages

**Primary:**
- Go 1.25.0 - Backend API (`/backend`)
- TypeScript ~6.0.2 - Frontend Web Application (`/frontend`)

**Secondary:**
- HTML/CSS - Frontend templates and styling

## Runtime

**Environment:**
- Go 1.25.0
- Node.js (for frontend build tools)

**Package Manager:**
- Backend: Go Modules (`/backend/go.mod`)
- Frontend: pnpm @10.30.0 (`/frontend/package.json`)
- Lockfile: Present (`/backend/go.sum`, `/frontend/pnpm-lock.yaml`)

## Frameworks

**Core:**
- Gin v1.12.0 - Backend HTTP framework (`/backend/go.mod`)
- React ^19.2.4 - Frontend UI library (`/frontend/package.json`)
- TanStack Router ^1.168.8 - Frontend routing (`/frontend/package.json`)

**Testing:**
- Vitest ^4.1.2 - Frontend unit/component testing (`/frontend/package.json`)
- Testify v1.11.1 - Backend assertions/mocking (`/backend/go.mod`)

**Build/Dev:**
- Vite ^7.3.1 - Frontend build tool and dev server (`/frontend/vite.config.ts`)
- GORM v1.31.1 - Backend ORM for PostgreSQL (`/backend/go.mod`)
- Tailwind CSS ^4.2.2 - Frontend styling (`/frontend/package.json`)

## Key Dependencies

**Critical:**
- Zustand ^5.0.12 - Frontend state management (`/frontend/package.json`)
- TanStack Query ^5.95.2 - Frontend data fetching/caching (`/frontend/package.json`)
- Asynq v0.26.0 - Backend background task processing (`/backend/go.mod`)
- Zod ^4.3.6 - Frontend schema validation (`/frontend/package.json`)

**Infrastructure:**
- PostgreSQL 17 - Primary relational database (`/backend/docker-compose.yml`)
- Redis - Caching and Asynq queue backend (`/backend/docker-compose.yml`)

## Configuration

**Environment:**
- Backend: `.env` files via `github.com/joho/godotenv` (`/backend/go.mod`)
- Frontend: Vite environment variables (`.env`)

**Build:**
- Backend: `go.mod`, `go.sum`
- Frontend: `vite.config.ts`, `tsconfig.json`, `eslint.config.js`

## Platform Requirements

**Development:**
- Go 1.25+
- Node.js (Latest LTS)
- pnpm
- Docker (for local DB/Redis)

**Production:**
- Fly.io (as indicated by `/backend/fly.toml`)

---

*Stack analysis: 2026-04-21*
