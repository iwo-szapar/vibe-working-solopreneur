# Client A - E-commerce Platform - Task Queue

**Last Updated:** 2025-11-10
**Current Sprint:** Week 9 (Nov 10-16)

---

## 🔴 URGENT (Due This Week)

### 1. Fix Stripe Webhook Failures
**Priority:** URGENT
**Status:** In Progress
**Assignee:** Luca
**Due:** Nov 12, 2025
**Estimate:** 8 hours

**Description:**
Stripe webhooks failing intermittently (30% failure rate). Need to implement:
- Idempotency keys for all webhook handlers
- Retry logic with exponential backoff
- Proper webhook signature verification
- Dead letter queue for failed events

**Blockers:**
- Need to understand Stripe's retry behavior better
- Local testing of webhooks is painful (using Stripe CLI)

**Acceptance Criteria:**
- [ ] Webhook failure rate < 1%
- [ ] All events processed exactly once (idempotency)
- [ ] Failed webhooks automatically retry
- [ ] Monitoring dashboard shows webhook health

**Related Files:**
- `api/src/webhooks/stripe.ts`
- `api/src/services/order.service.ts`

---

### 2. Deploy Payment Flow to Staging
**Priority:** URGENT
**Status:** Blocked
**Assignee:** Luca
**Due:** Nov 13, 2025
**Estimate:** 4 hours

**Description:**
Deploy Stripe integration to staging for client demo Thursday.

**Blockers:**
- Waiting for webhook fix (Task #1)
- Need Stripe test keys for staging environment

**Acceptance Criteria:**
- [ ] End-to-end checkout flow works on staging
- [ ] Test payment successful (Stripe test card)
- [ ] Order confirmation email sent
- [ ] Admin dashboard shows order

**Environment Setup:**
- Staging URL: `https://staging.retailco.com`
- Stripe test mode keys needed

---

### 3. Client Demo Prep (Thursday 2pm)
**Priority:** URGENT
**Status:** Not Started
**Assignee:** Luca
**Due:** Nov 14, 2025 (2pm PT)
**Estimate:** 3 hours

**Description:**
Prepare demo for Sarah (CTO):
- Working checkout flow
- Order confirmation email
- Admin dashboard showing orders
- Performance metrics (page load times)

**Demo Script:**
1. Browse products (show filtering/search)
2. Add to cart (show real-time inventory)
3. Checkout with Stripe (test card)
4. Show order confirmation email
5. Show admin dashboard (order details)
6. Discuss next steps (inventory management)

**Acceptance Criteria:**
- [ ] Demo script written and rehearsed
- [ ] Test data seeded in staging
- [ ] Screen recording backup (if demo fails)
- [ ] Slides for next phase discussion

---

## 🟠 HIGH PRIORITY (This Sprint)

### 4. Fix Inventory Race Conditions
**Priority:** HIGH
**Status:** Not Started
**Assignee:** Luca
**Due:** Nov 15, 2025
**Estimate:** 6 hours

**Description:**
Current implementation allows overselling (2 users can buy last item). Need to implement pessimistic locking or use database transactions properly.

**Technical Approach:**
- Use PostgreSQL row-level locking (`SELECT ... FOR UPDATE`)
- OR use Redis distributed locks
- Add inventory reservation system (15-min hold during checkout)

**Acceptance Criteria:**
- [ ] Concurrent checkout test passes (2 users, 1 item)
- [ ] Inventory never goes negative
- [ ] Abandoned carts release inventory after 15 min

**Related Files:**
- `api/src/services/inventory.service.ts`
- `api/src/services/cart.service.ts`

---

### 5. Optimize Product Image Loading
**Priority:** HIGH
**Status:** Not Started
**Assignee:** Junior Dev + Luca (review)
**Due:** Nov 16, 2025
**Estimate:** 5 hours

**Description:**
Product pages loading slowly (4-6 seconds) due to large images. Need to:
- Implement image optimization with Cloudinary
- Add lazy loading for below-fold images
- Use Next.js Image component properly
- Add blur placeholders

**Current Performance:**
- Largest Contentful Paint (LCP): 6.2s ❌
- Target: < 2.5s ✅

**Acceptance Criteria:**
- [ ] LCP < 2.5s on product pages
- [ ] Images lazy load below fold
- [ ] Blur placeholders while loading
- [ ] Lighthouse score > 90

**Related Files:**
- `storefront/src/components/ProductImage.tsx`
- `storefront/next.config.js`

---

## 🟢 NORMAL PRIORITY (Next Sprint)

### Test task for command validation
**Priority:** MEDIUM
**Status:** Not Started
**Added:** 2025-11-10

---

### 6. Order Confirmation Email Template
**Priority:** NORMAL
**Status:** Not Started
**Assignee:** Designer + Luca
**Due:** Nov 18, 2025
**Estimate:** 4 hours

**Description:**
Design and implement beautiful order confirmation email with:
- Order summary (items, quantities, prices)
- Shipping address
- Estimated delivery date
- Track order link
- Upsell recommendations

**Design Status:** Waiting for designer

**Acceptance Criteria:**
- [ ] Email template matches brand guidelines
- [ ] Responsive (mobile + desktop)
- [ ] Test email sent to client for approval
- [ ] Implemented with SendGrid templates

---

### 7. Admin Dashboard - Order Management
**Priority:** NORMAL
**Status:** In Progress (50% done)
**Assignee:** Junior Dev
**Due:** Nov 20, 2025
**Estimate:** 8 hours

**Description:**
Admin can view, filter, and manage orders:
- Order list with filters (status, date, customer)
- Order detail view
- Mark as shipped
- Refund processing
- Export to CSV

**Progress:**
- ✅ Order list view
- ✅ Basic filtering
- 🔄 Order detail view
- ⏳ Shipping status update
- ⏳ Refund flow

**Acceptance Criteria:**
- [ ] Admin can view all orders
- [ ] Filter by status, date, customer
- [ ] Mark order as shipped
- [ ] Process refund (Stripe API)
- [ ] Export orders to CSV

---

### 8. Add Error Monitoring (Sentry)
**Priority:** NORMAL
**Status:** Not Started
**Assignee:** Luca
**Due:** Nov 19, 2025
**Estimate:** 3 hours

**Description:**
Integrate Sentry for error tracking:
- Frontend errors (React error boundary)
- Backend errors (Express middleware)
- Performance monitoring
- Release tracking

**Why Now:**
- Approaching launch, need visibility into production errors
- Client expects 99.9% uptime

**Acceptance Criteria:**
- [ ] Sentry integrated frontend + backend
- [ ] Error alerts sent to Slack
- [ ] Source maps uploaded (for readable stack traces)
- [ ] Test error sent and received

---

## 🔵 LOW PRIORITY (Backlog)

### 9. Multi-Warehouse Inventory Support
**Priority:** LOW
**Status:** Not Started
**Due:** TBD (post-launch)
**Estimate:** 20 hours

**Description:**
Support multiple warehouses with smart routing:
- Inventory tracked per warehouse
- Order routing (ship from nearest warehouse)
- Warehouse management UI

**Deferred Until:** Post-launch (client agreed)

---

### 10. AI Product Recommendations
**Priority:** LOW
**Status:** Not Started
**Due:** TBD (Phase 3)
**Estimate:** 30 hours

**Description:**
Use collaborative filtering or GPT-4 to recommend products:
- "Customers also bought"
- Personalized homepage
- Cart upsells

**Deferred Until:** Phase 3 (Jan 2026)

---

## ✅ COMPLETED (This Week)

### ✅ Stripe Payment Intent Creation
**Completed:** Nov 9, 2025
**Time Spent:** 6 hours

Implemented Stripe Payment Intent flow:
- Create payment intent on checkout start
- Handle 3D Secure authentication
- Confirm payment on completion

**Key Learning:**
- Payment Intents better than Charges API (better auth flow)
- Need to handle async payment methods (bank transfers)

---

### ✅ Shopping Cart State Management
**Completed:** Nov 8, 2025
**Time Spent:** 4 hours

Refactored cart to use Zustand:
- Persistent cart (localStorage)
- Real-time inventory checks
- Optimistic updates with rollback

**Key Learning:**
- Zustand middleware great for persistence
- Need to sync cart on page load (inventory may have changed)

---

## 📊 Sprint Summary

**Total Tasks:** 10
**Urgent:** 3 🔴
**High:** 2 🟠
**Normal:** 4 🟢
**Low:** 2 🔵

**Estimated Hours This Sprint:** 38 hours
**Available Hours:** 25 hours
**Overflow:** 13 hours (need to defer or get help)

**Capacity Issue:** Overcommitted this sprint. Consider:
1. Defer image optimization to next sprint (Junior can handle alone)
2. Simplify demo (skip performance metrics discussion)
3. Ask client to push demo to Friday (buy 1 extra day)

---

**Notes:**
- Webhook issue is critical blocker for demo Thursday
- Should focus 100% on Tasks #1-3 this week
- Everything else can wait
