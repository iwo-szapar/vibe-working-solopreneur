# Client B - Analytics Dashboard - Task Queue

**Last Updated:** 2025-11-10
**Current Sprint:** Week 7 (Nov 10-23, 2 weeks)
**Sprint Goal:** Complete Facebook + LinkedIn connectors, onboard 2 beta partners

---

## 🔴 URGENT (This Week)

### 1. Fix Facebook API Rate Limit Handling
**Priority:** URGENT
**Status:** In Progress
**Assignee:** Luca
**Due:** Nov 13, 2025
**Estimate:** 10 hours

**Description:**
Facebook Ads API has 200 requests/hour rate limit. Current implementation hitting limits and causing sync failures (40% of syncs fail).

**Technical Approach:**
1. Implement exponential backoff with retry logic
2. Add request batching (10 requests → 1 batch request)
3. Implement circuit breaker pattern (stop syncing if rate limit hit)
4. Add rate limit monitoring dashboard

**Acceptance Criteria:**
- [ ] Sync success rate > 95%
- [ ] Batch requests implemented (reduces API calls by 80%)
- [ ] Circuit breaker prevents cascading failures
- [ ] DataDog alerts on rate limit issues

**Related Files:**
- `data-pipeline/connectors/facebook_ads.py`
- `api/src/services/sync_scheduler.py`

**References:**
- Facebook API docs: https://developers.facebook.com/docs/marketing-api/rate-limiting
- Airbyte Facebook connector: https://docs.airbyte.com/integrations/sources/facebook-marketing

---

### 2. Complete LinkedIn Ads Connector
**Priority:** URGENT
**Status:** In Progress (70% done)
**Assignee:** Luca + Data Engineer
**Due:** Nov 14, 2025
**Estimate:** 8 hours

**Description:**
Finish implementing LinkedIn Ads connector using Airbyte community connector.

**Remaining Work:**
- ✅ Connector configured in Airbyte
- ✅ Test connection successful
- 🔄 Schema mapping (LinkedIn fields → our DB)
- ⏳ Data transformation (dbt models)
- ⏳ Dashboard widget for LinkedIn data

**Blockers:**
- Community connector has bugs (missing fields: `costInLocalCurrency`)
- Need to fork and patch connector

**Acceptance Criteria:**
- [ ] LinkedIn data syncing every 15 min
- [ ] All required fields mapped correctly
- [ ] dbt models tested with sample data
- [ ] Dashboard shows LinkedIn campaigns

**Related Files:**
- `data-pipeline/airbyte/sources/linkedin-ads.yaml`
- `data-pipeline/dbt/models/linkedin/`

---

### 3. Onboard 2 Beta Design Partners
**Priority:** URGENT
**Status:** Not Started
**Assignee:** Luca + Frontend Dev
**Due:** Nov 15, 2025
**Estimate:** 6 hours

**Description:**
Get first 2 design partners live with real data:
- Partner A: Marketing agency (Google + Facebook)
- Partner B: SaaS company (Google + LinkedIn)

**Onboarding Checklist:**
1. [ ] Create accounts in staging environment
2. [ ] Connect their ad accounts (OAuth flows)
3. [ ] Trigger initial data sync (will take 2-4 hours)
4. [ ] Validate data accuracy (spot-check 10 campaigns)
5. [ ] Schedule kick-off call to walk through dashboard
6. [ ] Set up feedback channel (Slack Connect)

**Acceptance Criteria:**
- [ ] Both partners have live dashboards
- [ ] Data accuracy validated (compare to native platforms)
- [ ] Kick-off calls completed
- [ ] Feedback collected

---

## 🟠 HIGH PRIORITY (This Sprint)

### 4. Optimize TimescaleDB Query Performance
**Priority:** HIGH
**Status:** Not Started
**Assignee:** Luca
**Due:** Nov 18, 2025
**Estimate:** 12 hours

**Description:**
Dashboard queries taking 6-10 seconds (target: < 2s). Need to optimize TimescaleDB queries using continuous aggregates and better indexes.

**Current Slow Queries:**
1. Campaign performance over time (30-day range): 8.2s
2. Top 10 performing ads: 6.5s
3. Spend by day (90-day range): 9.1s

**Optimization Strategy:**
1. Create continuous aggregates for daily/hourly rollups
2. Add indexes on common filter columns (campaign_id, date, status)
3. Use TimescaleDB compression for old data (> 30 days)
4. Implement query result caching (Redis, 5-min TTL)

**Acceptance Criteria:**
- [ ] All queries < 2s (p95)
- [ ] Continuous aggregates created for daily/hourly metrics
- [ ] Compression enabled for data > 30 days old
- [ ] Redis caching implemented

**Related Files:**
- `api/src/db/queries/analytics.py`
- `data-pipeline/migrations/timescaledb/continuous_aggregates.sql`

**References:**
- TimescaleDB docs: https://docs.timescale.com/use-timescale/latest/continuous-aggregates/

---

### 5. Implement Real-Time Dashboard Updates
**Priority:** HIGH
**Status:** Not Started
**Assignee:** Frontend Dev + Luca (backend)
**Due:** Nov 20, 2025
**Estimate:** 10 hours

**Description:**
Dashboard currently requires manual refresh. Implement WebSocket-based real-time updates so metrics update automatically every 15 minutes.

**Technical Approach:**
- Backend: FastAPI WebSocket endpoint
- Frontend: Reconnecting WebSocket client
- Message format: JSON with updated metrics
- Connection pooling: Limit 1000 concurrent connections per instance

**Acceptance Criteria:**
- [ ] WebSocket connection established on dashboard load
- [ ] Metrics auto-update every 15 min (when new data synced)
- [ ] Connection resilient (auto-reconnect on disconnect)
- [ ] Load test: 100 concurrent users

**Related Files:**
- `api/src/websockets/dashboard.py`
- `dashboard-ui/src/hooks/useRealtimeData.ts`

---

### 6. Data Quality Validation Report
**Priority:** HIGH
**Status:** Not Started
**Assignee:** Data Engineer + Luca (review)
**Due:** Nov 21, 2025
**Estimate:** 8 hours

**Description:**
Build automated data quality checks to compare our data with source platforms (Google, Facebook, LinkedIn).

**Checks to Implement:**
1. Row count validation (our DB vs source API)
2. Metric validation (spend, clicks, impressions within 1% tolerance)
3. Missing data detection (gaps in time series)
4. Duplicate detection (same record synced twice)

**Output:**
- Daily report sent to Slack (#data-quality)
- Dashboard showing data quality metrics
- Alerts on quality issues

**Acceptance Criteria:**
- [ ] Automated checks run daily
- [ ] Report sent to Slack
- [ ] Dashboard shows quality metrics
- [ ] Alerts triggered on >1% discrepancy

**Related Files:**
- `data-pipeline/dags/data_quality_check.py`
- `api/src/reports/data_quality.py`

---

## 🟢 NORMAL PRIORITY (Next Sprint)

### 7. Custom Metric Builder - UX Iteration
**Priority:** NORMAL
**Status:** Blocked (waiting for designer)
**Assignee:** Designer + Frontend Dev
**Due:** Nov 25, 2025
**Estimate:** 12 hours

**Description:**
Current drag-and-drop metric builder is confusing for users. Need to simplify UX based on user testing feedback.

**User Feedback:**
- "Too many options, overwhelming"
- "Not clear what aggregation means"
- "Can't save metrics I create"

**Proposed Changes:**
1. Simplified UI with preset templates (common metrics)
2. Tooltips explaining aggregations (SUM, AVG, COUNT)
3. Save custom metrics feature
4. Share metrics with team

**Acceptance Criteria:**
- [ ] Redesign approved by design partner
- [ ] Implemented in UI
- [ ] User testing with 3 beta partners
- [ ] Save/share functionality working

---

### 8. Automated Report Scheduling (Phase 2 Prep)
**Priority:** NORMAL
**Status:** Not Started
**Due:** Nov 27, 2025
**Estimate:** 15 hours

**Description:**
Allow users to schedule automated reports (PDF/Excel) sent via email daily/weekly/monthly.

**Features:**
- Choose metrics and date range
- Select format (PDF or Excel)
- Schedule frequency (daily, weekly, monthly)
- Email recipients

**Technical Stack:**
- Celery task for report generation
- WeasyPrint (HTML → PDF)
- openpyxl (Excel generation)
- SendGrid (email delivery)

**Acceptance Criteria:**
- [ ] Report generation works (PDF + Excel)
- [ ] Scheduling UI implemented
- [ ] Celery task runs on schedule
- [ ] Email delivery confirmed

---

### 9. Add DataDog Monitoring for Data Pipeline
**Priority:** NORMAL
**Status:** Not Started
**Assignee:** Luca
**Due:** Nov 22, 2025
**Estimate:** 4 hours

**Description:**
Instrument data pipeline with DataDog metrics and traces.

**Metrics to Track:**
- Sync duration per connector
- Sync success/failure rate
- Data volume synced (rows per sync)
- API rate limit usage

**Traces:**
- End-to-end sync flow (Airbyte → dbt → API)

**Acceptance Criteria:**
- [ ] DataDog metrics visible in dashboard
- [ ] Traces showing sync flow
- [ ] Alerts configured (sync failure, slow queries)

---

## 🔵 LOW PRIORITY (Backlog)

### 10. White-Label Customization (Phase 2)
**Priority:** LOW
**Status:** Not Started
**Due:** TBD (Q1 2026)
**Estimate:** 40 hours

**Description:**
Allow enterprise clients to white-label dashboard (custom logo, colors, domain).

**Deferred Until:** After beta launch

---

### 11. Slack Alerting Integration
**Priority:** LOW
**Status:** Not Started
**Due:** TBD (Phase 2)
**Estimate:** 8 hours

**Description:**
Send alerts to Slack when metrics cross thresholds (e.g., spend > $1000/day, CTR < 1%).

**Deferred Until:** Phase 2 (beta partners requested)

---

## ✅ COMPLETED (This Sprint)

### ✅ Airbyte Google Ads Connector Live
**Completed:** Nov 7, 2025
**Time Spent:** 6 hours

Configured Google Ads connector in Airbyte, syncing every 15 min.

**Key Learnings:**
- OAuth setup tricky (requires service account + refresh token)
- Airbyte normalization slow (turned off, using dbt instead)

---

### ✅ FastAPI WebSocket Setup
**Completed:** Nov 8, 2025
**Time Spent:** 4 hours

Basic WebSocket endpoint working for real-time updates.

**Key Learnings:**
- FastAPI WebSocket support excellent (asyncio-based)
- Need connection pooling for scale (1000+ users)

---

### ✅ TimescaleDB Continuous Aggregates Research
**Completed:** Nov 9, 2025
**Time Spent:** 3 hours

Researched TimescaleDB continuous aggregates for performance.

**Key Learnings:**
- Can pre-compute metrics (daily, hourly rollups)
- Refreshes automatically on new data
- Reduces query time by 10-50x

---

## 📊 Sprint Summary

**Sprint Duration:** 2 weeks (Nov 10-23)
**Total Tasks:** 11
**Urgent:** 3 🔴
**High:** 3 🟠
**Normal:** 3 🟢
**Low:** 2 🔵

**Estimated Hours This Sprint:** 89 hours
**Available Hours:** 60 hours (30 hrs/week × 2 weeks)
**Overflow:** 29 hours

**Capacity Issue:** Overcommitted by 29 hours.

**Adjustment Plan:**
1. Defer Task #7 (Custom Metric Builder UX) to next sprint (blocked anyway)
2. Defer Task #9 (DataDog monitoring) to next sprint (nice-to-have)
3. Reduce Task #4 (Query optimization) scope: Focus only on top 2 slowest queries
4. **Adjusted hours:** 89 - 12 - 4 - 4 = 69 hours (still 9 hours over)
5. Ask frontend dev to handle Task #5 (Real-time updates) mostly solo (save 6 hours)
6. **Final adjusted:** 63 hours (within capacity with 3-hour buffer)

---

**Critical Path:**
1. Task #1 (Facebook rate limits) BLOCKS Task #3 (onboarding)
2. Task #2 (LinkedIn connector) BLOCKS Task #3 (onboarding)
3. Task #3 (onboarding) is sprint goal

**Risk:**
- If Facebook or LinkedIn tasks slip, can't onboard beta partners on time
- Mitigation: Focus 100% on Tasks #1-3 first, defer everything else if needed

---

**Notes:**
- Mike (VP Eng) expects demo with 2 live partners by Nov 16
- Facebook rate limit issue is critical blocker
- TimescaleDB optimization can wait if needed (queries slow but functional)
