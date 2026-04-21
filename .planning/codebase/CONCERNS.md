# Codebase Concerns

**Analysis Date:** 2026-04-21

## Tech Debt

**Frontend: SharePoint Creation Form**
- Issue: `frontend/src/features/sharepoints/components/create-sharepoint-form.tsx` is a "God Component" with over 1,600 lines of code. It manages state, validation, and UI for a complex 5-step process, including Mapbox and Google Places integrations.
- Files: `frontend/src/features/sharepoints/components/create-sharepoint-form.tsx`
- Impact: High cognitive load for maintainers, difficult to test individual steps, and increased risk of regressions when modifying form logic.
- Fix approach: Decompose the form into smaller components for each step (e.g., `DetailsStep.tsx`, `LocationStep.tsx`, `PricingStep.tsx`, `MediaStep.tsx`) and move the multi-step orchestrator logic to a custom hook.

**Backend: Context Usage**
- Issue: Extensive use of `context.TODO()` in S3 utility functions.
- Files: `backend/internal/utils/media.go`
- Impact: Breaks request cancellation and timeout propagation. If an S3 upload hangs, the request will not be cancelled even if the client disconnects or the handler times out.
- Fix approach: Update utility functions to accept `context.Context` as the first argument and pass the context from the Gin handler.

## Known Bugs

**Unimplemented Booking Logic**
- Symptoms: Core booking functionality is commented out.
- Files: `backend/internal/modules/sharepoint/service.go` (lines 353-364)
- Trigger: Any attempt to call `BookPortion` will return a near-empty `Booking` object without actually creating a record in the database.
- Workaround: None; the feature is currently a stub.

## Security Considerations

**Hardcoded Infrastructure URLs**
- Risk: S3 base URL is hardcoded, making it difficult to switch environments (dev/staging/prod) and risking exposure of internal naming conventions.
- Files: `backend/internal/utils/media.go`
- Current mitigation: None.
- Recommendations: Move `https://s3.playbit.org/` to an environment variable (e.g., `AWS_S3_BASE_URL`).

## Performance Bottlenecks

**S3 Client Initialization**
- Problem: `config.LoadDefaultConfig(context.TODO())` and `s3.NewFromConfig(cfg)` are called on every single function invocation in the media utility.
- Files: `backend/internal/utils/media.go`
- Cause: Creating a new AWS config and client for every upload/delete/presign operation is inefficient.
- Improvement path: Initialize the S3 client once at application startup or within a singleton/service and inject it into the utility functions.

## Fragile Areas

**Livestream Update Logic**
- Files: `backend/internal/modules/sharepoint/service.go` (lines 217-265)
- Why fragile: The code contains extensive comments indicating uncertainty about how 1:1 relations are handled between `SharePoint` and `Livestream` during updates. There is a risk of creating duplicate livestreams or failing to update existing ones correctly.
- Safe modification: Implement strict preloading of the `Livestream` relation in the repository's `FindByID` method and explicitly handle the "update vs create" logic based on the existence of an ID.
- Test coverage: Likely gaps in updating existing listings with modified livestream settings.

## Test Coverage Gaps

**Booking and Inventory Flow**
- What's not tested: The end-to-end booking process, including inventory reduction and booking record creation.
- Files: `backend/internal/modules/sharepoint/service.go`
- Risk: Critical business logic (payment and reservation) is currently a stub/commented out, meaning this area is effectively untested and broken.
- Priority: High
