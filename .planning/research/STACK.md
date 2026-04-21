# Technology Stack

**Project:** FoodSharePoint
**Researched:** 2026-04-21

## Recommended Stack

### Core Framework
| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| **React 19** | Latest | Frontend | Industry standard for performant, component-based UIs. |
| **TypeScript** | Latest | Language | Critical for type-safety in complex geospatial data structures. |
| **Go (Gin)** | Latest | Backend | High concurrency support, low memory footprint, ideal for real-time APIs. |

### Database
| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| **PostgreSQL** | Latest | Primary DB | Relational integrity for users, listings, and transactions. |
| **Redis** | Latest | Real-time State | Sub-millisecond latency for `GEOADD`/`GEORADIUS` and Pub/Sub. |

### Infrastructure
| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| **Google Maps Platform** | Fleet Engine | Logistics | Managed backend for vehicle tracking and routing. |
| **FCM (Firebase)** | Latest | Notifications | Reliable, cross-platform push delivery. |
| **Clerk** | Latest | Auth/User Mgmt | Rapid implementation of secure auth and identity verification. |

### Supporting Libraries
| Library | Version | Purpose | When to Use |
|---------|---------|---------|-------------|
| **Socket.io** | Latest | Real-time Comms | For the active "tracking" session between user and driver. |
| **Zod** | Latest | Validation | Schema validation for API requests/responses. |
| **TanStack Query** | Latest | State Mgmt | Efficient caching of marketplace listings. |

## Alternatives Considered

| Category | Recommended | Alternative | Why Not |
|----------|-------------|-------------|---------|
| Maps | Google Maps | Mapbox | Mapbox is great, but Fleet Engine provides a significant head start on logistics. |
| Auth | Clerk | Auth0 | Clerk offers a superior developer experience for modern React apps. |
| DB | Redis | MongoDB | MongoDB has geo-indexes, but Redis is significantly faster for high-frequency "hot" updates. |

## Installation

\`\`\`bash
# Backend (Go)
go get -u github.com/gin-gonic/gin
go get -u github.com/redis/go-redis/v9

# Frontend (React)
npm install @clerk/clerk-react @tanstack/react-query socket.io-client
\`\`\`

## Sources

- Google Maps Platform Mobility Docs (HIGH confidence)
- Redis Geospatial Documentation (HIGH confidence)
- Clerk Documentation (HIGH confidence)
- Firebase Cloud Messaging Docs (HIGH confidence)
