# Test E-Commerce - Patterns

## Payment Processing

### Pattern: Always validate payment before inventory update
**Confidence:** MEDIUM
**Context:** E-commerce checkout flow
**Why:** Prevents inventory deduction for failed payments
**When Seen:** 3 times across different projects
**Last Updated:** 2025-11-11

### Pattern: Implement idempotency keys for webhooks
**Confidence:** HIGH
**Context:** Stripe webhook handling
**Why:** Prevents duplicate order processing on webhook retries
**When Seen:** 5+ times, industry standard
**Last Updated:** 2025-11-09

## Authentication

### Pattern: Always implement token refresh proactively
**Confidence:** LOW
**Context:** JWT authentication
**Why:** Better UX than forcing re-login
**When Seen:** First time
**Last Updated:** 2025-11-10

## Performance

### Pattern: Cache expensive product queries
**Confidence:** MEDIUM
**Context:** Product catalog with filters
**Why:** Reduce database load, improve response time
**When Seen:** 3 times
**Last Updated:** 2025-11-05
