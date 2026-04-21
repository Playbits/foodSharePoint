# Feature Landscape

**Domain:** Food Sharing Marketplace
**Researched:** 2026-04-21

## Table Stakes

Features users expect. Missing = product feels incomplete.

| Feature | Why Expected | Complexity | Notes |
|---------|--------------|------------|-------|
| **User Auth/Profile** | Trust and account management | Low | Use Clerk for rapid setup. |
| **Food Listing (CRUD)** | Ability to share food | Low | Include photos, expiry date, and quantity. |
| **Map Discovery** | Find food nearby | Med | Use GEORADIUS in Redis for fast discovery. |
| **Real-time Tracking** | Know where the food/driver is | High | Integration with Google Fleet Engine. |
| **Push Notifications** | Get alerts for new food/pickup | Med | High-priority FCM for time-sensitive alerts. |
| **Chat/Messaging** | Coordinate pickup | Med | Simple WebSocket-based chat. |

## Differentiators

Features that set product apart. Not expected, but valued.

| Feature | Value Proposition | Complexity | Notes |
|---------|-------------------|------------|-------|
| **Adaptive Pinging** | Save battery while maintaining accuracy | High | Dynamic intervals based on proximity. |
| **Trust Score/Reviews** | Ensure quality and reliability | Med | Rating system for both givers and receivers. |
| **Smart Routing** | Optimize multi-stop pickups | High | Use Google Maps Routes API for optimized paths. |
| **AI-driven Categories** | Auto-categorize food from photos | Med | Use Gemini/GPT-4o for image analysis. |

## Anti-Features

Features to explicitly NOT build.

| Anti-Feature | Why Avoid | What to Do Instead |
|--------------|-----------|-------------------|
| **In-app Payment (MVP)** | Regulatory complexity (food laws) | Start with "Free" or "Cash on pickup". |
| **Custom Map Engine** | Massive engineering overhead | Use Google Maps or Mapbox SDKs. |
| **Full-blown Social Network** | Distracts from the core utility | Simple profiles and ratings. |

## Feature Dependencies

```
Identity Verification → Food Listing (Must be verified to list)
Map Discovery → Real-time Tracking (Must find item before tracking it)
FCM Notifications → Real-time Tracking (Alert user when driver is close)
```

## MVP Recommendation

Prioritize:
1. **Secure Auth & Phone Verification** (Clerk)
2. **Food Listing & Basic Map View** (Redis Geo)
3. **High-Priority Push Notifications** (FCM)
4. **Basic Real-time Tracking** (Google Maps SDK)

Defer: **AI Category Analysis**, **Optimized Multi-stop Routing**.

## Sources

- Competitor analysis of Too Good To Go and Olio (MEDIUM confidence)
- Industry patterns for on-demand delivery (HIGH confidence)
