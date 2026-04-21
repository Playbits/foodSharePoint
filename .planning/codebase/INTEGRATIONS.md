# External Integrations

**Analysis Date:** 2026-04-21

## APIs & External Services

**Authentication:**
- Clerk - User identity and session management
  - SDK/Client: `github.com/clerk/clerk-sdk-go/v2` (Backend), `@clerk/clerk-react` (Frontend)
  - Auth: Managed by Clerk

**Video Infrastructure:**
- Livepeer - Video transcoding and streaming
  - SDK/Client: `github.com/livepeer/livepeer-go` (Backend), `@livepeer/react` (Frontend)
- Mux - Video playback and analytics
  - SDK/Client: `github.com/muxinc/mux-go/v4` (Backend), `@mux/mux-player-react` (Frontend)

**Communication:**
- Mailjet - Transactional email delivery
  - SDK/Client: `github.com/mailjet/mailjet-apiv3-go` (Backend)

**Payments:**
- Paystack - Payment processing for African markets
  - SDK/Client: `react-paystack` (Frontend)

## Data Storage

**Databases:**
- PostgreSQL 17
  - Connection: Managed via GORM (`gorm.io/driver/postgres`)
  - Client: `gorm.io/gorm`

**File Storage:**
- AWS S3
  - SDK/Client: `github.com/aws/aws-sdk-go-v2/service/s3` (Backend)

**Caching:**
- Redis
  - Client: `github.com/go-redis/redis/v8` (Backend)

## Authentication & Identity

**Auth Provider:**
- Clerk
  - Implementation: Integrated via Clerk SDKs in both frontend and backend for seamless session management.

## Monitoring & Observability

**Error Tracking:**
- Not detected

**Logs:**
- Logrus - Structured logging in Go backend (`github.com/sirupsen/logrus`)

## CI/CD & Deployment

**Hosting:**
- Fly.io - Backend deployment (referenced in `/backend/fly.toml`)

**CI Pipeline:**
- Husky - Git hooks for linting/formatting (`/frontend/package.json`)

## Environment Configuration

**Required env vars:**
- Database credentials (`DB_USERNAME`, `DB_PASSWORD`, `DB_DATABASE`)
- Redis configuration
- Clerk API keys
- AWS S3 credentials
- Mailjet API keys
- Mux/Livepeer API keys

**Secrets location:**
- `.env` files (Note: Not committed to version control)

## Webhooks & Callbacks

**Incoming:**
- Potential webhooks from Paystack, Clerk, and Video providers (to be verified in `/backend/routes`)

**Outgoing:**
- API calls to Clerk, AWS S3, Livepeer, Mux, and Mailjet

---

*Integration audit: 2026-04-21*
