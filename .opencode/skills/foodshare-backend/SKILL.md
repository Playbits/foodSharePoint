---
name: foodshare-backend
description: Backend development skill for FoodSharePoint - Go API using Gin, GORM, PostgreSQL, and Redis/Asynq
allowed-tools:
  - "Bash"
  - "Read"
  - "Write"
  - "glob"
  - "grep"
  - "edit"
---

# Backend Development Skill for FoodSharePoint

## Overview

FoodSharePoint is a food marketplace platform connecting vendors with customers. The backend is built with **Go** using **Gin** framework, **GORM** for ORM, **PostgreSQL** for data storage, and **Redis + Asynq** for caching and background jobs.

## Technology Stack

| Component | Technology | Version |
|-----------|------------|---------|
| Language | Go | 1.24.x |
| HTTP Framework | Gin | v1.11.0 |
| ORM | GORM | v1.30.0 |
| Database | PostgreSQL | 17 |
| Cache/Queue | Redis + Asynq | Redis + v0.26.0 |
| Auth | Clerk | clerk-sdk-go/v2 |
| File Storage | AWS S3 | - |
| Email | Mailjet/MailerLite | - |
| Payments | Paystack | - |
| Logistics | Tship (Terminal Africa) | - |
| Livestream | Livepeer + Mux | - |

## Project Structure

```
backend/
├── main.go                          # Entry point - server startup, DI
├── routes/
│   ├── router.go                    # Router setup, middleware, service DI
│   ├── auth.go                      # Auth routes
│   ├── vendor.go                    # Vendor routes
│   ├── sharepoint.go                # Food listing routes
│   ├── booking.go                   # Booking routes
│   ├── livestreams.go              # Livestream routes
│   ├── payments.go                 # Payment routes
│   ├── logistics.go                # Logistics routes
│   ├── subscription.go             # Subscription routes
│   ├── review.go                   # Review routes
│   ├── addressbook.go              # Address book routes
│   └── common.go                   # Common routes (categories)
└── internal/
    ├── config/                     # Configuration (currently minimal)
    ├── database/
    │   ├── db.go                   # DB connection, migrations, seeding
    │   ├── redis.go                # Redis connection
    │   ├── models/                 # GORM models
    │   │   ├── user.go             # User model
    │   │   ├── vendor.go           # Vendor model
    │   │   ├── sharepoint.go       # Food listing model
    │   │   ├── booking.go          # Booking model
    │   │   ├── payment.go          # Payment model
    │   │   ├── logistics.go        # Delivery model
    │   │   ├── livestream.go      # Livestream model
    │   │   ├── subscription.go     # Subscription model
    │   │   ├── review.go           # Review model
    │   │   ├── addressbook.go      # Address model
    │   │   └── base.go             # Base model with timestamps
    │   └── init/                   # SQL migrations (enums)
    ├── middleware/
    │   ├── admin.go                # Admin authorization
    │   ├── customer.go             # Customer authorization
    │   ├── vendor.go               # Vendor authorization
    │   └── cors.go                 # CORS middleware
    ├── modules/                     # Feature modules (layered architecture)
    │   ├── auth/                   # Clerk JWT auth, JWKS
    │   ├── vendors/                # Vendor CRUD, documents
    │   ├── sharepoint/             # Food listings, occurrences
    │   ├── bookings/               # Food bookings
    │   ├── livestreams/            # Live video, comments
    │   ├── payments/               # Paystack integration
    │   ├── logistics/              # Tship API integration
    │   ├── subscriptions/          # Vendor subscriptions
    │   ├── reviews/                # Vendor ratings/reviews
    │   ├── addressbook/            # User addresses
    │   └── common/                 # Categories
    ├── infrastructure/
    │   ├── mailer/                 # Mailjet/MailerLite factory
    │   └── tship/                  # Logistics API client
    ├── tasks/                      # Asynq task types
    └── worker/                     # Background job processor
```

### Directory Responsibilities

| Directory | Responsibility | Examples |
|-----------|---------------|----------|
| `cmd/` | Entry points | `main.go` |
| `routes/` | HTTP routing, handler registration, middleware | `router.go`, `vendor.go` |
| `internal/modules/*/` | Handler → Service → Repository layers | Each feature module |
| `internal/database/models/` | GORM models, database schema | `vendor.go`, `booking.go` |
| `internal/middleware/` | Auth middleware (Clerk JWT) | `admin.go`, `vendor.go` |
| `internal/infrastructure/` | External service clients | `mailer/`, `tship/` |
| `internal/tasks/` | Asynq task type constants | Task type strings |
| `internal/worker/` | Background job handlers | Email, notifications |

## Module Architecture (Handler → Service → Repository)

Each feature follows a strict layered architecture:

```
internal/modules/vendors/
├── handler.go    # HTTP handlers (Gin)
├── service.go    # Business logic
├── repository.go # Database queries (GORM)
├── dto.go        # Request/Response DTOs
└── mocks/        # Mock interfaces for testing
```

### Example: Adding a New Endpoint

**1. Repository** (`internal/modules/vendors/repository.go`):
```go
type VendorRepository interface {
    FindByID(id string) (*Vendor, error)
    Create(vendor *Vendor) error
    // ...
}

type PostgresVendorRepository struct {
    db *gorm.DB
}

func (r *PostgresVendorRepository) FindByID(id string) (*Vendor, error) {
    var vendor Vendor
    err := r.db.First(&vendor, "id = ?", id).Error
    return &vendor, err
}
```

**2. Service** (`internal/modules/vendors/service.go`):
```go
type VendorService struct {
    repo VendorRepository
}

func (s *VendorService) GetVendorByID(id string) (*Vendor, error) {
    return s.repo.FindByID(id)
}
```

**3. Handler** (`internal/modules/vendors/handler.go`):
```go
type VendorHandler struct {
    service *VendorService
}

func (h *VendorHandler) GetVendor(c *gin.Context) {
    id := c.Param("id")
    vendor, err := h.service.GetVendorByID(id)
    if err != nil {
        c.JSON(404, gin.H{"error": "vendor not found"})
        return
    }
    c.JSON(200, vendor)
}
```

**4. Route** (`routes/vendor.go`):
```go
func VendorRoutes(r *gin.RouterGroup, handler *vendors.VendorHandler) {
    r.GET("/vendor/:id", handler.GetVendor)
}
```

## Development Commands

```bash
# Start infrastructure (PostgreSQL + Redis)
docker-compose -f backend/docker-compose.yml up -d

# Run development server
cd backend && go run main.go

# Run tests
go test ./internal/modules/vendors/...

# Run tests with coverage
go test -cover ./...

# Build for production
go build -o bin/server ./main.go
```

## Authentication (Clerk)

The backend uses Clerk for JWT authentication:

1. Frontend obtains JWT from Clerk
2. Backend validates JWT via JWKS (`CLERK_JWKS_URL`)
3. Middleware extracts user ID and role from JWT claims

### Auth Middleware Types
- **Optional**: Public endpoints (browse vendors, view profiles)
- **Required**: Authenticated endpoints (create booking, update profile)
- **Vendor**: Vendor-only endpoints (manage own shop)
- **Admin**: Admin-only endpoints (approve vendors)

## Background Jobs (Asynq)

Tasks are defined in `internal/tasks/` and processed by `internal/worker/`:

| Task Type | Handler | Purpose |
|-----------|---------|---------|
| `TypeEmailWelcome` | HandleWelcomeEmail | Welcome emails |
| `TypeEmailOrderConfirmed` | HandleBookingEmail | Booking confirmations |
| `TypeEmailNewOrderAlert` | HandleNewOrderAlert | New order notifications |
| `TypeEmailVendorSignup` | HandleVendorEmail | Vendor signup approvals |
| `TypeLivestreamFlush` | HandleLivestreamFlush | Livestream transcript |

## Database Models

Key models in `internal/database/models/`:

- **User** - Clerk user reference
- **Vendor** - Food seller profile (business info, documents)
- **SharePoint** - Food listing with schedule
- **SharePointOccurrence** - Specific date/time for SharePoint
- **Booking** - Customer order
- **Payment** - Paystack payment record
- **Logistics** - Delivery/shipping
- **Livestream** - Live video session
- **Subscription** - Vendor subscription plan
- **Review** - Customer review
- **AddressBook** - User saved addresses

## Testing

```bash
# Run all tests
go test ./...

# Run specific module
go test ./internal/modules/vendors/...

# Run with verbose output
go test -v ./...

# Run with coverage
go test -coverprofile=coverage.out ./...
go tool cover -html=coverage.out
```

Tests use **testify/assert** and **mock** interfaces:

```go
// Example test with mock
func TestVendorService_GetVendor(t *testing.T) {
    mockRepo := new(mocks.VendorRepository)
    mockRepo.On("FindByID", "123").Return(&Vendor{ID: "123"}, nil)
    
    service := NewVendorService(mockRepo)
    vendor, err := service.GetVendorByID("123")
    
    assert.NoError(t, err)
    assert.Equal(t, "123", vendor.ID)
}
```

## Code Patterns

### Error Handling
```go
result, err := someOperation()
if err != nil {
    return err  // Return up the stack
}
```

### Request Validation (Gin binding)
```go
type CreateVendorRequest struct {
    BusinessName string `json:"business_name" binding:"required"`
    Email        string `json:"email" binding:"required,email"`
}
```

### Dependency Injection
Services and repositories are injected via constructors in `routes/router.go`:

```go
vendorRepo := vendors.NewPostgresVendorRepository(db)
vendorService := vendors.NewVendorService(vendorRepo, reviewService, asynqClient)
vendorHandler := vendors.NewVendorHandler(vendorService)
```

## Conventions

1. **Layered Architecture**: Handler → Service → Repository (never skip layers)
2. **Interface Definitions**: Repository interfaces in the service package
3. **Error Handling**: Always check `if err != nil`, return errors up the stack
4. **Testing**: Mock interfaces for repository layer
5. **Background Jobs**: Use Asynq for async tasks (emails, notifications)
6. **Auth**: Clerk JWT, validate via JWKS in middleware
7. **Response Format**: Use consistent JSON structs (DTOs)

## Common Tasks

| Task | Location | Action |
|------|----------|--------|
| Add API endpoint | `routes/` + `internal/modules/` | Create handler method → register route |
| Add business logic | `internal/modules/*/service.go` | Create service method |
| Add database table | `internal/database/models/` | Create GORM model + add to Migrate() |
| Add background job | `internal/tasks/` + `internal/worker/` | Define task type + handler |
| Add auth middleware | `internal/middleware/` | Create middleware function |
| Add external API | `internal/infrastructure/` | Create client package |

## Environment Variables

Required in `backend/.env`:
- `DB_HOST`, `DB_PORT`, `DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD` - PostgreSQL
- `REDIS_HOST`, `REDIS_PORT` - Redis
- `CLERK_SECRET_KEY`, `CLERK_JWKS_URL` - Clerk authentication
- `AWS_BUCKET_NAME`, `AWS_REGION` - S3 file uploads
- `LIVEPEER_API_KEY`, `MUX_TOKEN_ID`, `MUX_TOKEN_SECRET` - Livestream
- `TSHIP_API_KEY` - Logistics (Terminal Africa)
- `MAILJET_API_KEY` / `MAILERLITE_API_KEY` - Email
