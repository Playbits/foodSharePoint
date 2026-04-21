# FoodSharePoint Project Guide

## Overview

FoodSharePoint is a **food marketplace platform** connecting vendors (food sellers) with customers. The project consists of **3 separate git repositories** in one directory structure:
- `backend/` - Go API
- `frontend/` - React web app
- `mobileapp/` - Flutter app (minimal setup)

## Tech Stack

### Backend
- **Language**: Go 1.24.x
- **Framework**: Gin v1.11.0
- **ORM**: GORM v1.30.0
- **Database**: PostgreSQL 17
- **Cache/Queue**: Redis + Asynq
- **Auth**: Clerk (JWT)
- **External APIs**: AWS S3, Paystack, Mailjet, Tship, Livepeer, Mux

### Frontend
- **Framework**: React 19.2.4
- **Language**: TypeScript 5.9.x
- **Bundler**: Vite 7.3.1
- **Routing**: TanStack Router 1.161.x
- **Data Fetching**: TanStack Query 5.90.x
- **UI**: shadcn/ui + Tailwind CSS 4
- **Forms**: React Hook Form + Zod
- **Auth**: Clerk
- **State**: Zustand

## Architecture

### Backend (Layered)
- **Handler** → **Service** → **Repository** pattern
- Modules in `internal/modules/`: auth, vendors, sharepoint, bookings, livestreams, payments, logistics, subscriptions, reviews, addressbook, common

### Frontend (Feature-based)
- Routes in `src/routes/` (TanStack Router, auto-generated)
- Features in `src/features/`
- API hooks in `src/hooks/` (TanStack Query wrappers)
- shadcn/ui components in `src/components/ui/`

## Key Files

| File | Purpose |
|------|---------|
| `backend/main.go` | Server entry point |
| `backend/routes/router.go` | Route registration, DI setup |
| `backend/internal/database/db.go` | DB connection, migrations |
| `frontend/src/main.tsx` | App entry point |
| `frontend/src/router.tsx` | Router + Query client setup |
| `frontend/src/hooks/api.tsx` | Axios with Clerk JWT interceptor |

## Development Commands

### Backend
```bash
docker-compose -f backend/docker-compose.yml up -d  # Start DB + Redis
cd backend && go run main.go                         # Run server
cd backend && go test ./...                         # Run tests
```

### Frontend
```bash
cd frontend && pnpm install
cd frontend && pnpm dev      # Start dev server (port 5173)
cd frontend && pnpm build    # Production build
cd frontend && pnpm test     # Run tests
cd frontend && pnpm lint     # Lint
```

## API Communication

- **Backend URL**: `http://localhost:8080/api/v2/`
- **Frontend API**: Axios with Clerk JWT in `Authorization: Bearer <token>` header
- **Frontend uses**: `useApi()` hook from `src/hooks/api.tsx`

## Authentication

Both frontend and backend use **Clerk**:
- Frontend: `<ClerkProvider>` + `useAuth()` hook
- Backend: JWT validation via JWKS in middleware

## Key Conventions

### Backend
- Handler → Service → Repository (strict layers)
- Interface definitions in service package
- GORM models in `internal/database/models/`
- Tests in `*_test.go` files with testify

### Frontend
- TanStack Router file-based routes
- TanStack Query for server state
- shadcn/ui for components
- Tailwind CSS with `cn()` utility
- Zustand for client state

## Environment Variables

### Backend (.env)
- Database: `DB_HOST`, `DB_PORT`, `DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD`
- Redis: `REDIS_HOST`, `REDIS_PORT`
- Auth: `CLERK_SECRET_KEY`, `CLERK_JWKS_URL`
- External: `AWS_BUCKET_NAME`, `LIVEPEER_API_KEY`, `TSHIP_API_KEY`, etc.

### Frontend (.env)
- `VITE_API_BASE_URL` = `http://localhost:8080/api/v2/`
- `VITE_CLERK_PUBLISHABLE_KEY`

## Skills

For detailed development guidance, see:
- `.opencode/skills/foodshare-backend/SKILL.md` - Backend development
- `.opencode/skills/foodshare-frontend/SKILL.md` - Frontend development
