# Architecture

**Analysis Date:** 2026-04-21

## Pattern Overview

**Overall:** Monorepo containing a Go-based REST API backend, a React-based web frontend, and a Flutter-based mobile app.

**Key Characteristics:**
- **Backend:** Strictly layered architecture focusing on separation of concerns.
- **Frontend:** Feature-based architecture to ensure scalability and maintainability.
- **Mobile:** Skeleton structure (currently minimal implementation).

## Layers

**Backend - Handler Layer:**
- Purpose: Handles HTTP request parsing and response formatting.
- Location: `backend/internal/modules/*/handler.go`
- Contains: Route handlers, input validation (via DTOs).
- Depends on: Service Layer.
- Used by: `backend/routes/router.go`.

**Backend - Service Layer:**
- Purpose: Orchestrates business logic and domain rules.
- Location: `backend/internal/modules/*/service.go`
- Contains: Business logic, transaction management.
- Depends on: Repository Layer, Infrastructure Layer.
- Used by: Handler Layer.

**Backend - Repository Layer:**
- Purpose: Abstracts data persistence.
- Location: `backend/internal/modules/*/repository.go`
- Contains: SQL queries and database interactions.
- Depends on: `backend/internal/database/`.
- Used by: Service Layer.

**Frontend - Route Layer:**
- Purpose: Defines application navigation and page mapping.
- Location: `frontend/src/routes/`
- Contains: Page components, route guards.
- Depends on: Module/Feature Layers.
- Used by: `frontend/src/main.tsx`.

**Frontend - Module Layer:**
- Purpose: Composes multiple features into high-level domain pages.
- Location: `frontend/src/modules/`
- Contains: Layouts for Admin, Vendor, and Customer domains.
- Depends on: Feature Layer.
- Used by: Route Layer.

**Frontend - Feature Layer:**
- Purpose: Encapsulates a specific business capability.
- Location: `frontend/src/features/`
- Contains: Feature-specific UI, hooks, and state logic.
- Depends on: `frontend/src/components/ui/`.
- Used by: Module Layer.

## Data Flow

**API Request Flow:**
1. Client (Frontend/Mobile) sends HTTP request to `backend/routes/`.
2. `handler.go` parses the request into a DTO.
3. `service.go` processes business logic using the DTO.
4. `repository.go` retrieves or persists data in PostgreSQL.
5. Response flows back through Service $\rightarrow$ Handler $\rightarrow$ Client.

**Frontend State Flow:**
1. Route triggers a page load in `frontend/src/modules/`.
2. Page invokes feature-specific components from `frontend/src/features/`.
3. Features use hooks to fetch data from the Backend API.
4. State is managed locally within features or globally via `frontend/src/stores/` or `frontend/src/context/`.

**State Management:**
- **Backend:** Stateless REST API.
- **Frontend:** Combination of TanStack Query (for server state) and local React state/Context (for UI state).

## Key Abstractions

**DTO (Data Transfer Object):**
- Purpose: Decouples API contract from internal database models.
- Examples: `backend/internal/modules/auth/dto.go`
- Pattern: Request/Response structs.

**Repository Pattern:**
- Purpose: Decouples business logic from the database implementation.
- Examples: `backend/internal/modules/bookings/repository.go`
- Pattern: Interface-based implementation.

**Feature Wrapper:**
- Purpose: Groups all assets for a single business capability.
- Examples: `frontend/src/features/sharepoints/`
- Pattern: Directory-per-feature.

## Entry Points

**Backend Server:**
- Location: `backend/main.go`
- Triggers: Process start.
- Responsibilities: Initialize DB, load config, start HTTP server.

**Frontend App:**
- Location: `frontend/src/main.tsx`
- Triggers: Browser page load.
- Responsibilities: Mount React app, initialize Router and Providers.

**Mobile App:**
- Location: `mobileapp/foodsharepoint/lib/main.dart`
- Triggers: App launch on device.
- Responsibilities: Initialize Flutter engine and root widget.

## Error Handling

**Strategy:** Centralized error handling with specific exception types.

**Patterns:**
- **Backend:** Use of an `exceptions` package (`backend/internal/exceptions`) to return standardized error codes and messages to the handler.
- **Frontend:** Error boundary components and route-level error handling (`frontend/src/routes/_authenticated/errors/`).

## Cross-Cutting Concerns

**Logging:** Standard Go logging in backend; console logging in frontend.
**Validation:** DTO-based validation in backend; likely Zod/React Hook Form in frontend.
**Authentication:** JWT-based authentication managed by `backend/internal/modules/auth` and Clerk on the frontend.

---

*Architecture analysis: 2026-04-21*
