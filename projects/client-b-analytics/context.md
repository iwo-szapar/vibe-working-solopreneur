# Client B - Analytics Dashboard Platform

**Project Type:** Client Work
**Status:** Active Development
**Timeline:** Q4 2025 - Q1 2026 (5 months)
**Last Updated:** 2025-11-10

---

## Client Overview

**Client Name:** DataFlow Analytics (Real name)
**Industry:** B2B SaaS - Marketing Analytics
**Business Size:** Series B ($20M ARR, 150 employees)

**Primary Contact:**
- Mike Rodriguez (VP Engineering)
- Bi-weekly syncs: Every other Monday 11am ET

---

## Project Scope

Building a real-time analytics dashboard platform that aggregates data from multiple marketing tools (Google Ads, Facebook Ads, LinkedIn, HubSpot) and provides unified reporting.

**Key Features:**
1. Multi-source data ingestion pipeline
2. Real-time dashboard with custom widgets
3. Automated report generation (PDF, Excel)
4. Alerting system (Slack, email) for anomalies
5. Custom metric builder (no-code interface)
6. Team collaboration (shared dashboards, comments)
7. White-label option for enterprise clients

---

## Tech Stack

**Frontend:**
- React 18 with TypeScript
- Vite (faster than CRA/Next.js for SPA)
- Recharts + D3.js (data visualization)
- TanStack Table (data tables)
- TailwindCSS + Radix UI

**Backend:**
- Python 3.11 with FastAPI
- PostgreSQL 15 (metrics storage)
- TimescaleDB (time-series extension)
- Redis (caching + rate limiting)
- Celery + RabbitMQ (async tasks)

**Data Pipeline:**
- Apache Airflow (orchestration)
- Airbyte (data ingestion connectors)
- dbt (data transformation)

**Infrastructure:**
- AWS (ECS Fargate, RDS, ElastiCache)
- Terraform (IaC)
- GitHub Actions (CI/CD)
- DataDog (monitoring)

**External Integrations:**
- Google Ads API
- Meta (Facebook) Marketing API
- LinkedIn Ads API
- HubSpot API
- Stripe (for billing)

---

## Current Phase

**Phase 1: Core Dashboard & Data Pipeline MVP**

**Completed:**
- ✅ Authentication system (Auth0)
- ✅ Google Ads connector (Airbyte)
- ✅ Basic dashboard layout
- ✅ Simple line chart widgets

**In Progress:**
- 🔄 Facebook Ads connector
- 🔄 LinkedIn Ads connector
- 🔄 Real-time data refresh
- 🔄 Custom metric builder (drag-and-drop)

**Upcoming (Phase 2):**
- ⏳ Automated report scheduling
- ⏳ Alerting system
- ⏳ Team collaboration features
- ⏳ White-label customization

---

## Constraints & Challenges

**Technical Constraints:**
- Must handle 100M+ data points per day
- Dashboard load time < 3 seconds
- 99.95% uptime (enterprise SLA)
- GDPR compliance (EU customers)

**Business Constraints:**
- Beta launch: Dec 15, 2025
- 10 design partners committed
- Fixed scope for beta (no feature creep)
- Limited budget: $180k total

**Current Blockers:**
- Facebook API rate limits causing data gaps
- TimescaleDB query performance not meeting targets (6-10s for complex queries)
- Custom metric builder UX needs design iteration

---

## Key Decisions

**Architecture Decisions:**
1. **Why TimescaleDB?** Time-series queries 10x faster than regular PostgreSQL for analytics
2. **Why Airbyte over custom connectors?** 300+ pre-built connectors, open-source, self-hosted
3. **Why FastAPI over Django?** Async support critical for real-time updates, better API performance
4. **Why Airflow?** Battle-tested for complex data pipelines, great monitoring UI

**Trade-offs:**
- Python backend (team knows Django) vs FastAPI (better performance, learning curve)
- Airbyte (complexity) vs custom connectors (simpler but more maintenance)
- Real-time updates (WebSockets) vs polling (simpler, higher latency)
- Self-hosted (control) vs managed services (easier, more expensive)

---

## Team & Communication

**My Role:** Senior Backend Engineer + Data Pipeline Lead
**Team:**
- Me (backend + data pipeline)
- 1 frontend dev (React dashboard)
- 1 data engineer (dbt models)
- 1 designer (UX for metric builder)
- 1 QA engineer (part-time)

**Communication Channels:**
- Slack: #dataflow-project
- Jira: Sprint planning and task tracking
- Bi-weekly sync with Mike (VP Eng)
- Weekly team standup: Wednesdays 10am ET

**Time Allocation:** 30 hours/week

---

## Success Metrics

**Technical KPIs:**
- Data ingestion latency < 5 minutes (from source API to dashboard)
- Dashboard query time < 2 seconds (p95)
- Pipeline success rate > 99%
- API uptime > 99.95%

**Business KPIs:**
- Beta launch: Dec 15, 2025 with 10 design partners
- All 10 partners actively using dashboard daily
- 0 critical bugs in first 2 weeks
- At least 5 partners upgrade to paid after beta

**Data Quality KPIs:**
- Data accuracy: 99.9% (vs source platforms)
- Missing data: < 0.1% of expected data points
- Refresh frequency: Every 15 minutes

---

## Resources

**Documentation:**
- System architecture: `/docs/architecture/system-overview.md`
- API docs: FastAPI auto-generated (`/docs` endpoint)
- Data pipeline: `/docs/data-pipeline/README.md`
- Airbyte connectors: `/docs/connectors/`

**Repositories:**
- Frontend: `github.com/dataflow/dashboard-ui` (private)
- Backend: `github.com/dataflow/api` (private)
- Data pipeline: `github.com/dataflow/data-pipeline` (private)
- Infrastructure: `github.com/dataflow/terraform` (private)

**Design:**
- Figma: [Link in Slack]
- Component library: Storybook deployed

**Airflow:**
- UI: `https://airflow.dataflow-staging.com`
- DAGs: `/data-pipeline/dags/`

---

## Notes & Learnings

**What's Going Well:**
- Airbyte saving massive time (would have taken months to build connectors)
- TimescaleDB compression reducing storage by 80%
- FastAPI async endpoints handling 1000s of concurrent requests easily

**What's Challenging:**
- Facebook API rate limits (200 requests/hour) too restrictive for our use case
- TimescaleDB query optimization non-trivial (different from regular PostgreSQL)
- Airbyte learning curve steep (complex configs, debugging is hard)
- Real-time dashboard updates causing high WebSocket connection costs

**Patterns Discovered:**
- Incremental data sync strategy (only fetch new data since last sync)
- TimescaleDB continuous aggregates for pre-computed metrics
- Rate limiting strategy for external APIs (exponential backoff, circuit breaker)
- WebSocket connection pooling for scalability

---

## Integration Details

### Data Sources (Current Priority)

**Google Ads:**
- Status: ✅ Live
- Connector: Airbyte (official connector)
- Sync frequency: Every 15 min
- Entities: Campaigns, Ad Groups, Ads, Keywords, Performance metrics

**Facebook Ads:**
- Status: 🔄 In Development
- Connector: Airbyte (official connector)
- Challenge: Rate limits (200 req/hr)
- Solution: Batch requests, incremental sync
- Entities: Campaigns, Ad Sets, Ads, Insights

**LinkedIn Ads:**
- Status: 🔄 In Development
- Connector: Airbyte (community connector)
- Challenge: No official Airbyte connector (using community version)
- Entities: Campaigns, Creatives, Analytics

**HubSpot:**
- Status: ⏳ Planned (Phase 2)
- Will sync: Contacts, Deals, Pipeline metrics

---

## Current Sprint (Week 7)

**Sprint Goal:** Complete Facebook + LinkedIn connectors, beta-test with 2 design partners

**Sprint Dates:** Nov 10-23, 2025

**Capacity:** 60 hours (30 hrs/week × 2 weeks)

**Key Deliverables:**
1. Facebook Ads connector live with rate limit handling
2. LinkedIn Ads connector live
3. Dashboard showing unified view (Google + Facebook + LinkedIn)
4. 2 design partners onboarded with real data
5. Data quality validation (compare to source platforms)

---

**Project Start Date:** 2025-09-15
**Beta Launch Date:** 2025-12-15
**General Availability:** 2026-02-01
**Contract Value:** $180,000
