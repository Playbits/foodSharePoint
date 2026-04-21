# FoodSharePoint - Roadmap

**Version:** 1.0  
**Status:** Roadmap Defined  
**Created:** 2026-04-21  

---

## Phases

- [ ] **Phase 1: Authentication & Basic Listings** - User accounts and food discovery
- [ ] **Phase 2: Orders & Payments** - Order management and payment processing
- [ ] **Phase 3: Logistics & Tracking** - Real-time delivery tracking
- [ ] **Phase 4: Reviews & History** - Ratings, reviews, and order history
- [ ] **Phase 5: Live Streaming** - Live food demonstrations
- [ ] **Phase 6: Admin** - Platform administration

---

## Phase Details

### Phase 1: Authentication & Basic Listings

**Goal:** Users can create accounts and browse/basic food listings

**Depends on:** Nothing (first phase)

**Requirements:** AUTH-01, AUTH-02, AUTH-03, AUTH-04, AUTH-05, VENDOR-01, VENDOR-02, VENDOR-03, CUSTOMER-01, CUSTOMER-02, CUSTOMER-03

**Success Criteria** (what must be TRUE):
1. User can create account with email/password and verify email
2. User can log in and stay logged in across browser sessions
3. User can log out from any page with one click
4. User can reset password via email link
5. System enforces role-based access (vendor/customer/driver)
6. Vendor can create food listings with name, description, price, and photos
7. Vendor can edit and delete their own listings
8. Vendor can set availability windows for pickup
9. Customer can browse food listings with filters (category, price, distance, rating)
10. Customer can view vendor profiles with ratings and availability
11. Customer can add items to cart and adjust quantities

**Plans:** TBD

---

### Phase 2: Orders & Payments

**Goal:** Vendors manage orders and customers complete purchases

**Depends on:** Phase 1

**Requirements:** VENDOR-04, VENDOR-05, CUSTOMER-04, PAYMENT-01, PAYMENT-02, PAYMENT-03

**Success Criteria** (what must be TRUE):
1. Vendor can view incoming orders with details
2. Vendor can accept or reject pending orders
3. Vendor can update order status (Pending → Accepted → Preparing → Ready → Delivered)
4. Customer can place orders from cart
5. Customer can pay securely via Paystack (card and alternative methods)
6. System processes refunds automatically for cancellations
7. System calculates and deducts platform fees from transactions

**Plans:** TBD

---

### Phase 3: Logistics & Tracking

**Goal:** Real-time delivery tracking and coordination

**Depends on:** Phase 2

**Requirements:** LOGISTICS-01, LOGISTICS-02, LOGISTICS-03, LOGISTICS-04, CUSTOMER-05, CUSTOMER-08, VENDOR-07

**Success Criteria** (what must be TRUE):
1. System automatically assigns orders to available drivers based on proximity
2. Driver can view assigned orders and accept or reject them
3. Driver location is tracked in real-time via WebSocket updates
4. Customer can view live map showing driver location and ETA
5. Delivery routes are optimized using mapping service
6. Customer receives push notifications for order status changes
7. Vendor receives notifications for new orders and status updates

**Plans:** TBD

---

### Phase 4: Reviews & History

**Goal:** Customer feedback and order history management

**Depends on:** Phase 3

**Requirements:** CUSTOMER-06, CUSTOMER-07, VENDOR-06

**Success Criteria** (what must be TRUE):
1. Customer can rate vendors with 1-5 stars and write text reviews
2. Customer can view their complete order history
3. Customer can reorder from past orders
4. Vendor can view earnings summary and transaction history

**Plans:** TBD

---

### Phase 5: Live Streaming

**Goal:** Live food demonstrations and video-on-demand

**Depends on:** Phase 4

**Requirements:** STREAM-01, STREAM-02, STREAM-03

**Success Criteria** (what must be TRUE):
1. Vendor can start live stream to demonstrate food items
2. Customers can watch live vendor streams embedded in vendor profile
3. Streams are recorded and available for on-demand viewing
4. Recorded videos are processed and accessible via video platform

**Plans:** TBD

---

### Phase 6: Admin

**Goal:** Platform administration and oversight

**Depends on:** Phase 5

**Requirements:** ADMIN-01, ADMIN-02, ADMIN-03

**Success Criteria** (what must be TRUE):
1. Admin can view platform metrics (GMV, orders, vendors, customers)
2. Admin can approve or reject vendor applications
3. Admin can view and resolve order disputes

**Plans:** TBD

---

## Coverage Map

| Phase | Requirements | Count |
|-------|--------------|-------|
| Phase 1 | AUTH-01, AUTH-02, AUTH-03, AUTH-04, AUTH-05, VENDOR-01, VENDOR-02, VENDOR-03, CUSTOMER-01, CUSTOMER-02, CUSTOMER-03 | 11 |
| Phase 2 | VENDOR-04, VENDOR-05, CUSTOMER-04, PAYMENT-01, PAYMENT-02, PAYMENT-03 | 6 |
| Phase 3 | LOGISTICS-01, LOGISTICS-02, LOGISTICS-03, LOGISTICS-04, CUSTOMER-05, CUSTOMER-08, VENDOR-07 | 7 |
| Phase 4 | CUSTOMER-06, CUSTOMER-07, VENDOR-06 | 3 |
| Phase 5 | STREAM-01, STREAM-02, STREAM-03 | 3 |
| Phase 6 | ADMIN-01, ADMIN-02, ADMIN-03 | 3 |

**Total:** 33/33 requirements mapped

---

## Progress Table

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Authentication & Basic Listings | 0/1 | Not started | - |
| 2. Orders & Payments | 0/1 | Not started | - |
| 3. Logistics & Tracking | 0/1 | Not started | - |
| 4. Reviews & History | 0/1 | Not started | - |
| 5. Live Streaming | 0/1 | Not started | - |
| 6. Admin | 0/1 | Not started | - |

---

*Last updated: 2026-04-21*