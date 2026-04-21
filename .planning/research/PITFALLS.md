# Domain Pitfalls

**Domain:** Food Sharing Marketplace
**Researched:** 2026-04-21

## Critical Pitfalls

Mistakes that cause rewrites or major issues.

### Pitfall 1: The "Ghost Listing" Problem
**What goes wrong:** Users arrive at a location only to find the food was already claimed, but the app still shows it as available.
**Why it happens:** Race conditions between "Claim" requests or latency in updating the geospatial index.
**Consequences:** Extreme user frustration and loss of trust in the platform.
**Prevention:** Implement **Atomic Claims** using Redis transactions (`MULTI`/`EXEC`) or Lua scripts to ensure only one user can claim an item.
**Detection:** High ratio of "claimed" but "not picked up" reports.

### Pitfall 2: Battery Drain Exhaustion
**What goes wrong:** Driver/Giver apps drain phone batteries in 2 hours.
**Why it happens:** Constant high-accuracy GPS pings (every 1-5 seconds) regardless of movement.
**Consequences:** Users disable location services or uninstall the app.
**Prevention:** Implement **Adaptive Reporting**. Use the device's activity recognition API (e.g., Google Activity Recognition) to stop pings when the device is stationary.
**Detection:** High app-uninstall rate correlated with high battery usage in OS logs.

## Moderate Pitfalls

### Pitfall 1: Notification Fatigue
**What goes wrong:** Users get 50 notifications a day for food they don't want.
**Prevention:** Implement strict **Preference Filters** (e.g., only notify for "Vegan" food within 1km). Use "Digest" notifications for low-priority items.

### Pitfall 2: Identity Spoofing
**What goes wrong:** Users create fake accounts to hoard "free" food.
**Prevention:** Require **Phone Number Verification** (SMS OTP) and implement a "Trust Score" based on successful pickups.

## Minor Pitfalls

### Pitfall 1: Map API Cost Explosion
**What goes wrong:** Monthly Google Maps bill spikes unexpectedly.
**Prevention:** Cache static map data. Use Mapbox or OSM for non-critical views. Implement request throttling for the map API.

## Phase-Specific Warnings

| Phase Topic | Likely Pitfall | Mitigation |
|-------------|---------------|------------|
| **Identity** | Over-friction (too many steps) | Use Clerk's seamless onboarding; defer KYC to later phases. |
| **Mapping** | Latency in marker updates | Use WebSockets for the "Active Delivery" view instead of polling. |
| **Notifications** | Low delivery rates | Use FCM "High Priority" and verify app background permissions. |

## Sources

- Logistics post-mortems from on-demand apps (MEDIUM confidence)
- Mobile OS battery optimization guides (HIGH confidence)
- Community discussions on peer-to-peer trust (MEDIUM confidence)
