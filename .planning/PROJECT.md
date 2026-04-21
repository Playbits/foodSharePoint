# FoodSharePoint - Project Context

**Version:** 1.0  
**Status:** Active Development  
**Last Updated:** 2026-04-21 after initialization

---

## What This Is

**FoodSharePoint** is a multi-platform food marketplace connecting food vendors (sellers) with customers. Built with three separate components: a Go backend API, React frontend web app, and Flutter mobile application.

The platform enables:
- **Vendors** to list and sell surplus food
- **Customers** to discover and order food from local vendors
- **Delivery coordination** with real-time tracking
- **Live streaming** for food demonstrations and events
- **Reviews and ratings** for trust building

---

## Core Value

Enable local food vendors to reduce waste and generate revenue by connecting with nearby customers through a trusted, real-time marketplace.

---

## Target Users

### Primary Users

1. **Food Vendors** - Restaurants, caterers, home chefs, and food entrepreneurs with surplus food inventory
2. **Customers** - Individuals and businesses looking to purchase affordable, local food
3. **Delivery Drivers** - Gig workers coordinating food pickup and delivery

### Secondary Users

1. **Platform Administrators** - Manage vendors, content, and disputes
2. **Logistics Partners** - External delivery services

---

## Success Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Vendor Activation Rate | >60% of signups become active | Weekly tracking |
| Order Completion Rate | >90% of orders completed | Per-order |
| Average Rating | >4.0 stars | Vendor review average |
| Delivery Time | <45 minutes average | Order to delivery |
| User Retention | >40% returning within 30 days | Cohort analysis |

---

## Technical Context

### Multi-Repository Structure

```
foodSharePoint/              # Root repository (this)
├── backend/                 # Go API (git submodule → git@github.com:Playbits/foodSharePoint.git)
├── frontend/                # React web app (git submodule → git@github.com:Playbits/foodSharePoint.git)
└── mobileapp/               # Flutter mobile (git submodule → git@github.com:Playbits/food_share_point_mobile.git)
```

### Technology Stack

#### Backend (`backend/`)
- **Language:** Go 1.24.x
- **Framework:** Gin v1.11.0
- **ORM:** GORM v1.30.0
- **Database:** PostgreSQL 17
- **Cache:** Redis + Asynq
- **Auth:** Clerk JWT
- **APIs:** AWS S3, Paystack, Mailjet, Tship, Livepeer, Mux

#### Frontend (`frontend/`)
- **Framework:** React 19.2.4
- **Language:** TypeScript 5.9.x
- **Bundler:** Vite 7.3.1
- **Routing:** TanStack Router 1.161.x
- **Data:** TanStack Query 5.90.x
- **UI:** shadcn/ui + Tailwind CSS 4
- **Forms:** React Hook Form + Zod
- **Auth:** Clerk
- **State:** Zustand

#### Mobile App (`mobileapp/`)
- **Framework:** Flutter
- **Platforms:** iOS, Android, Web, Desktop
- **State:** Provider/Bloc pattern
- **Navigation:** GoRouter

### External Integrations

| Service | Purpose | API |
|---------|--------|-----|
| Clerk | Authentication | JWT validation |
| AWS S3 | File storage | REST |
| Paystack | Payments | REST |
| Mailjet | Transactional email | REST |
| Tship | Logistics/shipping | REST |
| Livepeer | Live streaming | REST |
| Mux | Video processing | REST |

---

## Requirements

### Validated

_(None yet — ship to validate)_

### Active

- [ ] **[VENDOR-01]** - Vendors can create and manage food listings
- [ ] **[VENDOR-02]** - Vendors can manage orders and availability
- [ ] **[VENDOR-03]** - Vendors receive real-time notifications

- [ ] **[CUSTOMER-01]** - Customers can browse and search food listings
- [ ] **[CUSTOMER-02]** - Customers can place and pay for orders
- [ ] **[CUSTOMER-03]** - Customers can track delivery in real-time
- [ ] **[CUSTOMER-04]** - Customers can rate and review vendors

- [ ] **[AUTH-01]** - Secure onboarding with email/password
- [ ] **[AUTH-02]** - Session management across platforms
- [ ] **[AUTH-03]** - Role-based access (vendor/customer/driver)

- [ ] **[LOGISTICS-01]** - Order assignment and routing
- [ ] **[LOGISTICS-02]** - Real-time driver tracking
- [ ] **[LOGISTICS-03]** - Delivery notifications

- [ ] **[STREAM-01]** - Live food demonstrations
- [ ] **[STREAM-02]** - Video streaming infrastructure

- [ ] **[PAYMENT-01]** - Secure payment processing
- [ ] **[PAYMENT-02]** - Refund and dispute handling

### Out of Scope

- **Multi-vendor cooperatives** — Complex organizational structures defer to v2
- **AI-powered recommendations** — Requires usage data; defer after launch
- **Subscription meal plans** — Billing complexity; defer to v2
- **International expansion** — Regulatory complexity; focus on target market first
- **White-label API** — External developer SDKs; defer to v3

---

## Key Decisions

| Decision | Rationale | Outcome |
|---------|-----------|---------|
| Multi-repo architecture | Independent deployment and team ownership | — Pending |
| Clerk for authentication | Managed auth reduces security surface | — Pending |
| Google Maps Fleet Engine | Proven logistics solution for real-time tracking | — Pending |
| Real-time via WebSockets | Low latency for driver tracking | — Pending |
| Event-driven architecture | Separates hot path (real-time) from cold path (persistence) | — Pending |

---

## Evolution

This document evolves at phase transitions and milestone boundaries.

**After each phase transition** (via `/gsd-transition`):
1. Requirements invalidated? → Move to Out of Scope with reason
2. Requirements validated? → Move to Validated with phase reference
3. New requirements emerged? → Add to Active
4. Decisions to log? → Add to Key Decisions
5. "What This Is" still accurate? → Update if drifted

**After each milestone** (via `/gsd-complete-milestone`):
1. Full review of all sections
2. Core Value check — still the right priority?
3. Audit Out of Scope — reasons still valid?
4. Update Context with current state

---

## Research Summary

Based on domain research completed 2026-04-21:

- **Stack:** React/TypeScript + Go/Gin + PostgreSQL + Redis + Google Maps Platform (Fleet Engine)
- **Architecture:** Event-driven with "Hot Path" (Redis/WebSockets) and "Cold Path" (Kafka/Postgres)
- **Critical pitfalls:** Battery drain on driver devices, notification fatigue
- **Suggested phases:** Foundation & Identity → Core Marketplace & Basic Maps → Real-time Logistics → High-Urgency Engagement

See: `.planning/research/SUMMARY.md` for full research findings.

---

*Last updated: 2026-04-21 after initialization*