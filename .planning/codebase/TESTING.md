# Testing Patterns

**Analysis Date:** 2026-04-21

## Test Framework

**Runner:**
- Frontend: Vitest `^4.1.2`
- Backend: Go `testing` package
- Mobile: `flutter_test`
- Config: `frontend/vite.config.ts` (indirectly via Vitest)

**Assertion Library:**
- Frontend: `@testing-library/jest-dom`, `@testing-library/react`
- Backend: Standard `t.Errorf`
- Mobile: `expect` from `flutter_test`

**Run Commands:**
```bash
pnpm test               # Frontend: Run all tests (vitest)
pnpm test:ui            # Frontend: Vitest UI mode
go test ./...           # Backend: Run all tests
flutter test            # Mobile: Run all tests
```

## Test File Organization

**Location:**
- Frontend: Co-located with components (e.g., `address-book-form.test.tsx`) or in `__tests__` directories.
- Backend: Co-located with implementation (e.g., `service_test.go`).
- Mobile: Separate `test/` directory.

**Naming:**
- Frontend: `*.test.tsx` or `*.test.ts`.
- Backend: `*_test.go`.
- Mobile: `*_test.dart`.

**Structure:**
```
frontend/src/**/[file].test.tsx
backend/internal/**/[file]_test.go
mobileapp/foodsharepoint/test/[file]_test.dart
```

## Test Structure

**Suite Organization:**
```typescript
// Frontend example (conceptual based on findings)
describe('Component Name', () => {
  it('should render correctly', () => {
    // test logic
  })
})
```

**Patterns:**
- Backend: Use `gomock` for repository mocking.
- Frontend: Use React Testing Library for component interaction.

## Mocking

**Framework:**
- Backend: `go.uber.org/mock/gomock`
- Frontend: Vitest mocks

**Patterns:**
```go
// Backend mocking pattern
ctrl := gomock.NewController(t)
defer ctrl.Finish()
mockRepo := mocks.NewMockSharepointRepository(ctrl)
mockRepo.EXPECT().
    UpdateStatus(gomock.Any(), spID, status).
    Return(nil)
```

**What to Mock:**
- Backend: Repositories, external services.
- Frontend: API calls, complex state providers.

## Fixtures and Factories

**Test Data:**
- Frontend: `@faker-js/faker` used for generating test data.

**Location:**
- Not explicitly isolated in found samples; usually defined within the test file.

## Coverage

**Requirements:** Not explicitly enforced in configs.

**View Coverage:**
```bash
pnpm test --coverage # Frontend (if configured in vitest)
go test -cover ./...  # Backend
```

## Test Types

**Unit Tests:**
- Backend: Service and handler tests.
- Frontend: Hook and utility tests.

**Integration Tests:**
- Backend: Database tests (`backend/internal/database/db_test.go`).
- Frontend: Component integration tests.

**E2E Tests:**
- Not detected.

## Common Patterns

**Async Testing:**
- Frontend: `async/await` with Vitest.
- Backend: Go routines/channels (standard Go).

**Error Testing:**
- Backend: Verifying `err != nil`.
- Frontend: Testing error states in UI.

---

*Testing analysis: 2026-04-21*
