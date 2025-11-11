# Architecture Decisions & Tech Stack

**Purpose:** Document your technical choices, tech stack, and architectural patterns

---

## Current Tech Stack

### Backend
- **Language:** [e.g., Python 3.11]
- **Framework:** [e.g., FastAPI, Django, Flask]
- **Database:** [e.g., PostgreSQL 15]
- **Caching:** [e.g., Redis]
- **Message Queue:** [e.g., Celery, RabbitMQ]

### Frontend
- **Language:** [e.g., TypeScript]
- **Framework:** [e.g., React 18, Vue, Svelte]
- **State Management:** [e.g., Redux, Zustand]
- **Styling:** [e.g., Tailwind CSS, styled-components]

### Infrastructure
- **Hosting:** [e.g., AWS ECS, Google Cloud Run]
- **CI/CD:** [e.g., GitLab CI, GitHub Actions]
- **Monitoring:** [e.g., Sentry, Datadog]
- **Version Control:** [e.g., GitLab]

---

## Key Architecture Decisions

### Decision 1: [Name]
**Date:** [When decided]
**Context:** [Why this decision was needed]
**Decision:** [What was chosen]
**Rationale:**
- [Reason 1]
- [Reason 2]
- [Reason 3]

**Alternatives Considered:**
- [Alternative A]: [Why not chosen]
- [Alternative B]: [Why not chosen]

**Outcome:** [How it's working, any regrets]

---

### Decision 2: Monolith vs Microservices
**Date:** Oct 2024
**Context:** Scaling internal product to handle 10K+ users
**Decision:** Start with modular monolith
**Rationale:**
- Team of 2 can't manage microservice complexity
- Faster iteration speed
- Can extract services later if needed
- All the benefits of good separation within single deployable

**Alternatives Considered:**
- Microservices: Too much operational overhead for small team
- Traditional monolith: Chose modular approach for future flexibility

**Outcome:** Working well. Deployment is simple, development is fast.

---

## Architectural Patterns

### Pattern: API Design
- RESTful endpoints for CRUD
- GraphQL for complex data fetching (if needed)
- Versioning: /api/v1/ prefix
- Authentication: JWT tokens (15-20 min expiry for mobile)

### Pattern: Error Handling
- Structured error responses (code, message, details)
- Log to Sentry with user context
- Never expose internal errors to clients
- Always include request ID for debugging

### Pattern: Database
- PostgreSQL for relational data
- Always index foreign keys
- Use migrations (Alembic/Django migrations)
- Connection pooling (pgBouncer for production)

### Pattern: Async Processing
- Use Celery for background jobs
- Keep request handlers fast (< 200ms)
- Offload: emails, reports, large data processing
- Idempotent tasks (safe to retry)

---

## Technology Choices by Project

### Client A - E-commerce
- **Stack:** Python/Django, PostgreSQL, React
- **Why:** Client's existing stack, team expertise
- **Constraints:** Must support 50K products, 1K concurrent users

### Client B - Analytics Platform
- **Stack:** Python/FastAPI, PostgreSQL, React
- **Why:** Performance critical, async endpoints needed
- **Constraints:** GDPR compliance, 10K+ concurrent users

### Internal Product - SaaS
- **Stack:** Python/FastAPI, PostgreSQL, React, Redis
- **Why:** Full control, modern stack, great DX
- **Constraints:** Multi-tenant, scalable to 100K users

---

## Conventions & Standards

### Code Organization
```
backend/
├── api/           # API routes
├── models/        # Database models
├── services/      # Business logic
├── utils/         # Shared utilities
└── tests/         # Tests (mirror structure)
```

### Naming Conventions
- **Variables:** snake_case
- **Functions:** verb_noun (get_user, create_order)
- **Classes:** PascalCase
- **Constants:** UPPER_SNAKE_CASE

### Documentation
- Docstrings for all public functions
- README.md in each major directory
- Architecture diagrams (draw.io) in /docs

### Testing
- Unit tests for business logic
- Integration tests for APIs
- Target: 80%+ coverage on critical paths
- Use pytest (Python), Jest (JavaScript)

---

## Security Patterns

### Authentication
- JWT tokens (access + refresh)
- Access token: 15-20 min expiry (mobile), 5-10 min (web)
- Refresh token: 7 days expiry
- Store refresh tokens server-side (Redis)

### Authorization
- Role-based access control (RBAC)
- Permissions checked at API layer
- Never trust client-side validation

### Data Protection
- Encrypt sensitive data at rest (PII, payments)
- TLS for all connections
- Secrets in environment variables (never in code)
- Regular security audits

---

## Performance Patterns

### Caching Strategy
- Redis for session data, API responses
- Cache-aside pattern (check cache, fetch DB, update cache)
- TTL based on data volatility (users: 1 hour, static: 1 day)

### Database Optimization
- Always index foreign keys
- Use EXPLAIN ANALYZE for slow queries
- Connection pooling (max 20 connections per instance)
- Read replicas for heavy read workloads

### Frontend Performance
- Code splitting (route-based)
- Lazy loading for non-critical components
- Image optimization (WebP, lazy load)
- Bundle size budget: < 200KB initial load

---

## Deployment Patterns

### CI/CD Pipeline
```
Commit → GitLab CI
  → Run tests
  → Build Docker image
  → Deploy to staging (auto)
  → Deploy to production (manual approval)
```

### Zero-Downtime Deployment
- Blue-green deployment for production
- Database migrations run before deployment
- Health checks before routing traffic

### Rollback Strategy
- Keep last 3 versions deployable
- Automated rollback if health checks fail
- Manual rollback via GitLab CI

---

## Lessons Learned

### What Works Well
- FastAPI for new projects (great DX, performance)
- PostgreSQL for everything (relational data is 90% of use cases)
- React for complex UIs (ecosystem is mature)
- Docker for deployment (consistent environments)

### What We Avoid
- Microservices (too early, too complex for team size)
- NoSQL unless strong reason (adds complexity)
- Custom authentication (use proven libraries)
- Premature optimization (profile first)

### What We'd Change
- Start with TypeScript from day 1 (Python + JS/TS hybrid is good)
- More upfront API design (prevents breaking changes)
- Better monitoring from start (add observability early)

---

**Last Updated:** [Date]
**Maintained By:** Luca
**Review Frequency:** Quarterly (or when major decisions made)
