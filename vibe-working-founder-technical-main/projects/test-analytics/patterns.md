# Test Analytics Dashboard - Patterns

## Database Performance

### Pattern: Cache expensive aggregations
**Confidence:** HIGH
**Context:** Analytics dashboard with complex queries
**Why:** Aggregations on large datasets are slow, caching reduces load time from 10s to <1s
**When Seen:** 5+ times across analytics projects
**Last Updated:** 2025-11-11

### Pattern: Use materialized views for frequently accessed metrics
**Confidence:** MEDIUM
**Context:** Real-time dashboards
**Why:** Pre-computed results avoid repeated expensive queries
**When Seen:** 3 times
**Last Updated:** 2025-11-10

## Real-Time Data

### Pattern: Implement connection pooling for WebSockets
**Confidence:** LOW
**Context:** Real-time event streaming
**Why:** Prevents connection exhaustion under high load
**When Seen:** First time
**Last Updated:** 2025-11-10

### Pattern: Use Redis pub/sub for event distribution
**Confidence:** MEDIUM
**Context:** Multi-server WebSocket architecture
**Why:** Ensures all server instances receive events
**When Seen:** 2 times
**Last Updated:** 2025-11-09

## Data Retention

### Pattern: Implement time-based data archival
**Confidence:** HIGH
**Context:** High-volume event tracking
**Why:** Keeps database size manageable, improves query performance
**When Seen:** 5+ times, industry standard
**Last Updated:** 2025-11-09
