# Architecture Patterns

**Domain:** Food Sharing Marketplace
**Researched:** 2026-04-21

## Recommended Architecture

The system follows a **CQRS-inspired Event-Driven Architecture** to separate the high-frequency real-time tracking path from the relational marketplace data.

### Component Boundaries

| Component | Responsibility | Communicates With |
|-----------|---------------|-------------------|
| **API Gateway (Gin)** | Request routing, Auth validation, Rate limiting | Redis, Postgres, FCM |
| **Real-time Engine (Redis)** | Geospatial indexing, current state of all active deliveries | API Gateway, WebSockets |
| **Marketplace DB (Postgres)** | Persistent storage for users, food listings, history | API Gateway |
| **Notification Service** | Orchestrating FCM alerts based on events | Redis (Events), FCM API |
| **Fleet Engine (Google)** | Managed logistics state and route optimization | Driver App, API Gateway |

### Data Flow

1. **Update Path (Hot)**: Driver App $\rightarrow$ Fleet Engine $\rightarrow$ API Gateway $\rightarrow$ Redis (`GEOADD`) $\rightarrow$ WebSocket Broadcast $\rightarrow$ User App.
2. **Discovery Path**: User App $\rightarrow$ API Gateway $\rightarrow$ Redis (`GEORADIUS`) $\rightarrow$ User App.
3. **Persistence Path (Cold)**: API Gateway $\rightarrow$ Async Queue (Kafka/RabbitMQ) $\rightarrow$ Postgres (History/Analytics).

## Patterns to Follow

### Pattern 1: The "Hot-Cold" State Split
**What:** Store current, rapidly changing data (location) in memory and permanent data in a relational DB.
**When:** When update frequency exceeds DB write capacity or latency requirements are $<100ms$.
**Example:**
\`\`\`typescript
// Hot Path: Update Redis
await redis.geoadd("active_deliveries", lng, lat, driverId);

// Cold Path: Async update to Postgres via queue
await queue.add("update-location-history", { driverId, lng, lat, timestamp });
\`\`\`

### Pattern 2: Delta Updates (updateMask)
**What:** Send only the changed fields in a request.
**When:** Updating large objects frequently.
**Example:**
\`\`\`http
PATCH /vehicles/{id}?updateMask=last_location
{ "last_location": { "lat": 40.7, "lng": -74.0 } }
\`\`\`

## Anti-Patterns to Avoid

### Anti-Pattern 1: Direct DB Polling for Location
**What:** Having the client app call `SELECT * FROM locations WHERE...` every 5 seconds.
**Why bad:** Destroys DB performance as user base grows.
**Instead:** Use WebSockets and Redis Pub/Sub to push updates only when they happen.

## Scalability Considerations

| Concern | At 100 users | At 10K users | At 1M users |
|---------|--------------|--------------|-------------|
| **Location State** | Single Redis Instance | Redis Cluster (Sharded) | Regional Redis Clusters |
| **Notifications** | Direct FCM calls | Distributed Queue (BullMQ) | Multi-region Notification Workers |
| **Database** | Single Postgres | Read Replicas + Partitioning | Distributed SQL (CockroachDB/TiDB) |

## Sources

- Google Maps Mobility Architecture (HIGH confidence)
- Redis Geospatial Best Practices (HIGH confidence)
- Uber Engineering Blog (HIGH confidence)
