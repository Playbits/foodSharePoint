# Technical Ecosystem Research: Food Sharing Platform

**Researched:** 2026-04-21
**Confidence:** HIGH

## 1. Real-time Location Services and Mapping

### Ecosystem Overview
The mapping ecosystem for delivery-style apps is dominated by **Google Maps Platform**, **Mapbox**, and **OpenStreetMap (OSM)**.

### Recommendations & Rationale

| Service | Recommendation | Why |
|----------|----------------|-----|
| **Google Maps Platform** | **Primary Choice** | Best-in-class data accuracy, pervasive usage, and specialized **Fleet Engine** for logistics. |
| **Mapbox** | **Alternative** | Superior customization of map styles and highly competitive pricing for high-volume vector tile usage. |
| **OpenStreetMap** | **Niche/Cost-Saving** | Ideal for projects requiring complete data ownership or zero-cost map tiles (when self-hosted). |

### Best Practices for Real-time Tracking
- **Managed State (Fleet Engine)**: Use a managed backend like Google's Fleet Engine to handle the complex state of "vehicles" (drivers/food items). It abstracts the logic of updating and retrieving the "last known location."
- **Optimized Updates**: Use `updateMask` (in REST/gRPC) to send only the changed fields (e.g., `last_location`), reducing payload size and backend processing.
- **Driver-Side Efficiency**: Implement adaptive reporting intervals. Increase frequency when the driver is near the destination and decrease it during long transit phases to save battery.

---

## 2. Push Notification Strategies

### Ecosystem Overview
**Firebase Cloud Messaging (FCM)** is the industry standard for cross-platform delivery. **OneSignal** provides a higher-level abstraction with better marketing tools.

### Strategy for Time-Sensitive Food Alerts
1. **Message Priority**: Use `high` priority for food alerts to ensure the OS delivers the notification immediately, even if the device is in Doze mode.
2. **Silent Push (Data Messages)**: Send a "silent" push to the client first. The client app fetches the latest food status and updates the local state *before* the visible notification is triggered, ensuring the notification content is 100% current.
3. **TTL (Time-to-Live)**: Set a short TTL (e.g., 30 minutes) for food alerts. A notification that a "food item is available" is useless if it arrives 4 hours later.
4. **Notification Channels**: Use specific Android channels (e.g., "Immediate Alerts") so users can prioritize food notifications over marketing ones.

---

## 3. Secure Authentication and Identity Verification

### Ecosystem Overview
Modern "Auth-as-a-Service" providers like **Clerk**, **Auth0**, and **Firebase Auth** have replaced custom JWT implementations.

### Identity Verification Patterns
- **Tiered Trust Model**:
    - **Level 1 (Basic)**: Email/Phone verification via OTP (One-Time Password). Sufficient for browsing.
    - **Level 2 (Verified)**: SMS verification via Clerk/Auth0. Required for listing food or claiming an item.
    - **Level 3 (Trusted)**: Integration with KYC providers (e.g., Stripe Identity, Onfido) for high-value or regulated sharing.
- **Session Security**: Use short-lived access tokens and sliding-window refresh tokens.
- **Protected Procedures**: Implement backend guards (e.g., tRPC `protectedProcedure`) to verify user identity before allowing access to sensitive location data of other users.

---

## 4. Scalability for High-Frequency, Low-Latency Updates

### Technical Architecture for Real-time State
To handle thousands of concurrent location updates without crashing the primary database:

#### The "Hot Path" (Real-time)
- **Redis Geospatial Indexes**: Use `GEOADD` to store current coordinates and `GEORADIUS` to find nearby food/drivers. Redis's in-memory nature provides the sub-millisecond latency required.
- **Pub/Sub for Broadcasting**: When a driver moves, publish the update to a Redis channel (e.g., `location:driver_123`). The user's client subscribes to this channel via a WebSocket.
- **WebSocket Gateway**: Use a scalable WebSocket layer (e.g., Socket.io with a Redis adapter) to maintain persistent connections with users.

#### The "Cold Path" (Persistence)
- **Asynchronous Logging**: Do not write every single location ping to the primary DB (Postgres). Instead, stream updates through **Kafka** or **RabbitMQ** and batch-write them to a time-series database or cold storage for history/analytics.

### Scalability Matrix

| Concern | Approach at 1K Users | Approach at 100K+ Users |
|---------|-------------------|-------------------------|
| **Location State** | Single Redis Instance | Redis Cluster with Geo-sharding |
| **Notifications** | Standard FCM | FCM + Distributed Queue (BullMQ/Celery) |
| **Database** | Single Postgres Instance | Read Replicas + Partitioning by Region |
| **Real-time** | Single WebSocket Server | Distributed Gateway with Redis Adapter |
