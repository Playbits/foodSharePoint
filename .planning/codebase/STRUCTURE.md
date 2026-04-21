# Codebase Structure

**Analysis Date:** 2026-04-21

## Directory Layout

```
[project-root]/
├── backend/                # Go API Server
│   ├── cmd/                # Entry points for the server
│   ├── internal/           # Private application code
│   │   ├── database/       # DB connection and entity models
│   │   ├── infrastructure/ # External service integrations
│   │   └── modules/        # Domain-driven business modules
│   └── routes/             # HTTP route definitions
├── frontend/               # React Web Application
│   └── src/
│       ├── components/     # Shared UI components (Atomic Design)
│       ├── features/       # Feature-specific logic and components
│       ├── modules/        # Domain-level page compositions
│       ├── routes/         # TanStack Router definitions
│       └── providers/      # Global context providers
└── mobileapp/              # Flutter Mobile Application
    └── foodsharepoint/
        └── lib/            # Mobile source code
```

## Directory Purposes

**backend/internal/modules:**
- Purpose: Encapsulates business logic for specific domains.
- Contains: Handlers (HTTP), Services (Logic), Repositories (Data), and DTOs.
- Key files: `handler.go`, `service.go`, `repository.go`, `dto.go` within each module.

**backend/internal/database:**
- Purpose: Centralized data access and model definitions.
- Contains: GORM models and DB initialization logic.
- Key files: `models/*.go`, `db.go`.

**backend/internal/infrastructure:**
- Purpose: Implementations of external service clients.
- Contains: Mailer and shipping integrations.
- Key files: `mailer/mailer.go`, `tship/tship.go`.

**frontend/src/features:**
- Purpose: Isolated business features that can be reused across different pages.
- Contains: Feature-specific components, hooks, and API call definitions.
- Key files: `index.tsx` and `components/` within each feature.

**frontend/src/modules:**
- Purpose: High-level domain groupings (e.g., Admin, Customer, Vendor) that assemble features into full pages.
- Contains: Page-level components and layout logic.
- Key files: `pages/*.tsx`.

**frontend/src/routes:**
- Purpose: Application routing and navigation structure.
- Contains: Route definitions using TanStack Router.
- Key files: `__root.tsx`, `_authenticated/route.tsx`.

**frontend/src/components/ui:**
- Purpose: Low-level, reusable UI primitives (shadcn/ui).
- Contains: Basic elements like `button.tsx`, `input.tsx`, `dialog.tsx`.

## Key File Locations

**Entry Points:**
- `backend/main.go`: Primary backend entry point.
- `frontend/src/main.tsx`: Primary frontend entry point.
- `mobileapp/foodsharepoint/lib/main.dart`: Primary mobile entry point.

**Configuration:**
- `backend/internal/config/`: Backend configuration.
- `frontend/tsconfig.json`: Frontend TypeScript config.
- `frontend/vite.config.ts`: Vite build config.

**Core Logic:**
- `backend/internal/modules/*/service.go`: Backend business logic.
- `frontend/src/features/*/index.tsx`: Frontend feature logic.

**Testing:**
- `backend/internal/modules/*/service_test.go`: Backend unit tests.
- `frontend/src/features/*/components/*.test.tsx`: Frontend component tests.

## Naming Conventions

**Files:**
- Backend Modules: `handler.go`, `service.go`, `repository.go` (consistent per module).
- Frontend Features: Kebab-case filenames (e.g., `sharepoints-table.tsx`).
- Frontend Components: PascalCase for React components.

**Directories:**
- Backend: Lowercase (e.g., `internal`, `modules`).
- Frontend: Lowercase (e.g., `features`, `routes`).

## Where to Add New Code

**New Backend Feature:**
- Create a new directory in `backend/internal/modules/[feature-name]`.
- Implement `dto.go`, `repository.go`, `service.go`, and `handler.go`.
- Register routes in `backend/routes/router.go`.

**New Frontend Feature:**
- Create a new directory in `frontend/src/features/[feature-name]`.
- Add components and logic.
- Map it to a route in `frontend/src/routes/`.

**New Shared UI Component:**
- Add to `frontend/src/components/ui/` if it is a primitive.
- Add to `frontend/src/components/` if it is a complex shared component.

**Utilities:**
- Backend: `backend/internal/utils/`.
- Frontend: `frontend/src/hooks/` or `frontend/src/lib/`.

## Special Directories

**.planning/codebase:**
- Purpose: Contains codebase analysis documentation for AI agents.
- Generated: Yes.
- Committed: Yes.

---

*Structure analysis: 2026-04-21*
