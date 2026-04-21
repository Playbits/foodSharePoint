# Coding Conventions

**Analysis Date:** 2026-04-21

## Naming Patterns

**Files:**
- Frontend: kebab-case for components and pages (e.g., `address-book-form.test.tsx`).
- Backend: snake_case for files (e.g., `service_test.go`, `db.go`).
- Mobile: snake_case for Dart files (e.g., `widget_test.dart`).

**Functions:**
- Frontend: camelCase (e.g., `useCartStore`).
- Backend: PascalCase for exported functions, camelCase for internal (e.g., `NewSharepointService`, `updateStatus`).
- Mobile: camelCase (e.g., `main`).

**Variables:**
- Frontend: camelCase.
- Backend: camelCase.
- Mobile: camelCase.

**Types:**
- Frontend: PascalCase (e.g., `SharePoint`).
- Backend: PascalCase for exported structs/types (e.g., `SharePoint`).

## Code Style

**Formatting:**
- Frontend: Prettier
  - Semi: `false`
  - Single Quote: `true`
  - Tab Width: `2`
  - Print Width: `80`
  - JSX Single Quote: `true`
- Backend: `gofmt` (standard Go formatting).
- Mobile: `dart format` (standard Flutter formatting).

**Linting:**
- Frontend: ESLint with `typescript-eslint` and `eslint-plugin-unused-imports`.
  - Rules: `no-console` is an error, `unused-imports` are errors.
- Backend: Standard Go linting.
- Mobile: `flutter_lints` (via `analysis_options.yaml`).

## Import Organization

**Order (Frontend):**
Enforced by `@trivago/prettier-plugin-sort-imports`:
1. Vite/Built-ins
2. React
3. Third-party modules
4. Internal aliases (`@/assets`, `@/api`, `@/stores`, `@/lib`, `@/utils`, `@/constants`, `@/context`, `@/hooks`, `@/components`, `@/features`)
5. Relative imports

**Path Aliases:**
- Frontend: `@/` points to `src/`.

## Error Handling

**Patterns:**
- Backend: Explicit error return values `(result, error)`.
- Frontend: TanStack Query for API error handling and Zod for validation.

## Logging

**Framework:**
- Frontend: `console` (though `no-console` is set to error in ESLint, likely used during dev).
- Backend: Standard Go logging or custom infrastructure.

## Comments

**When to Comment:**
- Standard practice for complex logic.

**JSDoc/TSDoc:**
- Not widely observed in samples.

## Function Design

**Size:** Not explicitly defined.
**Parameters:** Standard for language (Go context, TS objects).
**Return Values:** Standard (Go error patterns, TS promises).

## Module Design

**Exports:**
- Frontend: Named exports preferred.
- Backend: Exported via PascalCase.

**Barrel Files:**
- Not prominently observed in samples.

---

*Convention analysis: 2026-04-21*
