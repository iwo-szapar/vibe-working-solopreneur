# Product Management & Client Work Patterns

**Purpose:** Track patterns for managing client projects, prioritizing features, and running product development

**Confidence levels:**
- **HIGH** (5+ examples): Battle-tested, apply automatically
- **MEDIUM** (2-4 examples): Promising pattern, test further
- **LOW** (1 example): Early observation, validate

---

## Pattern Template

```markdown
## [Pattern Name]

**Confidence:** HIGH/MEDIUM/LOW
**Based on:** X examples
**Last reinforced:** YYYY-MM-DD
**Time/value saved:** ~X hours or $X

**Context:**
[When does this pattern apply?]

**Pattern:**
[What's the reusable approach?]

**Why it works:**
[Why is this effective?]

**Examples:**
1. [Project/Client] - [Date] - [Outcome]
2. [Project/Client] - [Date] - [Outcome]

**Template/Checklist:**
[If applicable, show reusable structure]

**Source experiences:**
- memory/episodic/projects/YYYY-MM-DD-description.md
```

---

## Example Patterns

Below are example patterns. Replace with Luca's actual learned patterns from client and product work.

---

## Document Technical Constraints Upfront

**Confidence:** MEDIUM
**Based on:** 3 examples
**Last reinforced:** 2024-12-10
**Time saved:** ~2 hours per project (prevents scope creep)

**Context:**
When starting a new client project or feature development

**Pattern:**
Document technical constraints, existing stack, and non-negotiables in the first meeting. Add to project context.md immediately.

**What to capture:**
- **Existing tech stack:** What must we use? What can we replace?
- **Performance requirements:** Response times, load capacity, uptime
- **Integration constraints:** What systems must we integrate with?
- **Data constraints:** GDPR, data residency, retention policies
- **Team constraints:** Who maintains this? What's their skill level?
- **Budget constraints:** What's the ceiling? What's the MVP budget?

**Why it works:**
- Prevents "we can't use X" surprises mid-project
- Sets realistic expectations from day 1
- Guides technology choices early
- Avoids costly refactors later
- Creates shared understanding with client

**Examples:**
1. Client A (Oct 2024) - Documented PostgreSQL requirement → avoided MongoDB pitch
2. Client B (Nov 2024) - Identified GDPR constraints → chose EU hosting early
3. Internal Product (Dec 2024) - Documented team skill gaps → chose familiar stack

**Template:**

```markdown
## Project: [Client Name] - [Project Name]

### Technical Constraints

**Must use:**
- [ ] Python 3.11+ (team expertise)
- [ ] PostgreSQL 15 (existing infrastructure)
- [ ] Deploy on AWS (company standard)

**Cannot use:**
- [ ] NoSQL databases (team unfamiliar)
- [ ] Microservices (team too small)
- [ ] Real-time features (budget constraint)

**Integration requirements:**
- [ ] Existing CRM via REST API
- [ ] Payment provider: Stripe
- [ ] Email: SendGrid

**Performance requirements:**
- [ ] API response < 200ms (95th percentile)
- [ ] Support 1,000 concurrent users
- [ ] 99.9% uptime SLA

**Compliance:**
- [ ] GDPR compliant (EU data residency)
- [ ] HTTPS everywhere
- [ ] Data retention: 90 days

**Team constraints:**
- [ ] 2 developers (full-stack)
- [ ] 10 hours/week available
- [ ] Python/React experience, no DevOps expertise

**Budget:**
- [ ] MVP: $15K
- [ ] Monthly hosting: $500 max
- [ ] 3 month timeline
```

**Source experiences:**
- memory/episodic/projects/2024-10-15-client-a-kickoff.md
- memory/episodic/projects/2024-11-22-client-b-gdpr-setup.md
- memory/episodic/projects/2024-12-10-internal-product-planning.md

---

## Weekly Client Check-ins Prevent Scope Creep

**Confidence:** LOW
**Based on:** 1 example
**Last reinforced:** 2024-12-20
**Value:** Prevented $5K scope creep, maintained client relationship

**Context:**
When managing ongoing client projects (not one-off builds)

**Pattern:**
Schedule 15-minute weekly check-ins (Friday preferred) to:
1. Review what was shipped this week
2. Preview what's planned for next week
3. Surface any blockers or concerns
4. Confirm priorities haven't shifted

**Why it should work:**
- Prevents "surprise" feature requests at month-end
- Keeps client informed (reduces anxiety)
- Surfaces priority changes early
- Builds trust through transparency
- Creates natural checkpoint for feedback

**Examples:**
1. Client A (Dec 2024) - Caught priority shift early, avoided building wrong feature

**Template email:**

```markdown
Subject: [Project Name] - Week of [Date] Update

Hi [Client Name],

Quick 15-min check-in for Friday at [Time]?

**This week we shipped:**
- Feature A (user can now X)
- Bug fix B (resolved Y issue)
- Performance improvement C (Z is 2x faster)

**Next week we're planning:**
- Feature D (implement ABC)
- Feature E (first version of XYZ)
- Code review & testing

**Any blockers?**
- Waiting on [X] from your team (needed by Tuesday)

**Anything changed priority-wise?**
Let me know if we should adjust the plan.

See you Friday!
```

**Needs validation:**
- Test with 2-3 more clients
- Measure: scope creep reduction, client satisfaction, surprise requests
- Refine: Maybe bi-weekly for smaller projects?

**Source experiences:**
- memory/episodic/projects/2024-12-20-client-a-check-in.md

---

## MVP Feature Selection: "Must Have to Launch"

**Confidence:** MEDIUM
**Based on:** 4 examples
**Last reinforced:** 2025-01-08
**Time saved:** ~10 hours per project (avoids building non-essential features)

**Context:**
When defining MVP scope for new project or feature

**Pattern:**
For each proposed feature, ask: "Can we launch without this?"
- Yes → Move to "Phase 2" or "Nice to have"
- No → Keep in MVP, but challenge again

Run this exercise twice:
1. First pass: Move obvious non-essentials to Phase 2
2. Second pass: Challenge remaining features harder

**Why it works:**
- Launches happen faster (fewer features = less time)
- Validates core value prop first (not distracted by bells/whistles)
- Preserves budget for iteration (don't spend everything on MVP)
- Reduces complexity (easier to test, easier to maintain)
- Forces prioritization decisions (can't have everything)

**Examples:**
1. Client A - E-commerce platform (Sep 2024): Cut 40% of features, launched 6 weeks early
2. Client B - Analytics dashboard (Oct 2024): Cut advanced filters, launched in 4 weeks
3. Internal Product - SaaS MVP (Nov 2024): Cut team features, focused on single-user
4. Client C - Mobile app (Jan 2025): Cut social features, launched with core functionality

**Exercise template:**

```markdown
## MVP Feature Selection: [Project Name]

### Round 1: "Can we launch without this?"

| Feature | Must Have? | Rationale |
|---------|------------|-----------|
| User login | ✅ YES | Can't use app without account |
| Password reset | ✅ YES | Users will forget passwords |
| User profile edit | ❌ NO | Can add after launch |
| Avatar upload | ❌ NO | Not core to value prop |
| Product listing | ✅ YES | Core feature |
| Product search | ❌ NO | Can browse initially |
| Product filters | ❌ NO | Nice to have, not critical |
| Shopping cart | ✅ YES | Can't buy without it |
| Wishlist | ❌ NO | Not essential for purchase |
| Checkout | ✅ YES | Core feature |
| Payment | ✅ YES | Can't complete purchase otherwise |
| Order history | ✅ YES | Users need to track orders |
| Email notifications | ✅ YES | Confirm order, shipping updates |
| Push notifications | ❌ NO | Email is sufficient for MVP |

### Round 2: Challenge remaining "Must Have" features

| Feature | Still Must Have? | Could we...? |
|---------|------------------|--------------|
| Password reset | ✅ YES | Manual reset = bad UX, support burden |
| Product search | 🤔 MAYBE | Start with categories only? Add search in Week 2? |
| Email notifications | 🤔 MAYBE | Manual for first 10 customers? No - doesn't scale |

### Final MVP Scope

**In scope (Must Have):**
1. User login / registration
2. Password reset
3. Product listing (with categories)
4. Shopping cart
5. Checkout + payment (Stripe)
6. Order history
7. Email notifications (order confirmation, shipping)

**Phase 2 (Nice to Have):**
1. User profile editing
2. Avatar upload
3. Product search
4. Advanced filters
5. Wishlist
6. Push notifications

**Estimated time savings:** 4-6 weeks (Phase 2 features would have added 60-80 hours)
```

**Source experiences:**
- memory/episodic/projects/2024-09-10-client-a-mvp-scoping.md
- memory/episodic/projects/2024-10-22-client-b-dashboard-scope.md
- memory/episodic/projects/2024-11-15-internal-product-mvp.md
- memory/episodic/projects/2025-01-08-client-c-mobile-scope.md

---

## Client Onboarding Checklist Prevents Miscommunication

**Confidence:** LOW
**Based on:** 1 example
**Last reinforced:** 2024-11-20
**Time saved:** ~3 hours (prevented misalignment)

**Context:**
When starting a new client project

**Pattern:**
Use standardized onboarding checklist to ensure nothing is missed.

**Checklist:**

```markdown
## Client Onboarding: [Client Name]

### Business Context
- [ ] What problem are we solving? (In client's words)
- [ ] Who are the end users? (Demographics, tech-savviness)
- [ ] What's the success metric? (How do we know it works?)
- [ ] What's the timeline? (Hard deadline or flexible?)
- [ ] What's the budget? (Total and monthly)

### Technical Context
- [ ] Existing tech stack (if any)
- [ ] Technical constraints (compliance, performance, integrations)
- [ ] Access to systems (credentials, API keys, staging environments)
- [ ] Team structure (who's on client side? technical expertise?)

### Communication
- [ ] Primary contact (who makes decisions?)
- [ ] Preferred communication (Slack, email, calls?)
- [ ] Meeting cadence (weekly, bi-weekly, ad-hoc?)
- [ ] Timezone (if distributed team)

### Deliverables
- [ ] What are we building? (MVP scope defined)
- [ ] What format? (Deployed app, source code handoff, both?)
- [ ] Documentation required? (Technical docs, user docs, both?)
- [ ] Training needed? (Client team needs to maintain?)

### Legal / Admin
- [ ] Contract signed
- [ ] Payment terms (upfront, milestones, monthly?)
- [ ] IP ownership (who owns the code?)
- [ ] NDA if needed

### Project Setup
- [ ] Git repository created
- [ ] Project management tool setup (Fibery, Jira, etc.)
- [ ] CI/CD pipeline configured
- [ ] Staging environment provisioned
- [ ] Communication channels created (Slack channel, etc.)

### First Meeting
- [ ] Schedule kickoff call (1 hour)
- [ ] Send pre-meeting questionnaire (gather context)
- [ ] Review questionnaire before call
- [ ] Take notes during call
- [ ] Send summary after call (confirm understanding)
```

**Why this should work:**
- Prevents "I thought you were..." misunderstandings
- Creates shared reference (everyone on same page)
- Surfaces blockers early (missing access, unclear requirements)
- Professional impression (client sees you're thorough)
- Scalable (same process for every client)

**Examples:**
1. Client B (Nov 2024) - Caught missing staging environment access before starting work

**Needs validation:**
- Use with next 2-3 clients
- Refine checklist based on what's missing
- Measure: miscommunications, rework due to misalignment

**Source experiences:**
- memory/episodic/projects/2024-11-20-client-b-onboarding.md

---

## User Feedback Loop: "Show Early, Show Often"

**Confidence:** MEDIUM
**Based on:** 2 examples
**Last reinforced:** 2024-12-15
**Value:** Avoided 20+ hours of rework

**Context:**
When building features for clients or internal products

**Pattern:**
Show work-in-progress early and frequently, even if incomplete.
- Day 2-3: Show wireframes or rough UI mockup
- Day 5-7: Show working prototype (ugly, but functional)
- Day 10-12: Show polished version for feedback
- Before "done": Final review with client/users

**Why it works:**
- Catches misunderstandings early (before investing too much time)
- Clients appreciate transparency (not a black box)
- Builds trust (see progress, not just invoices)
- Reduces rework (course-correct early)
- Manages expectations (client sees evolution, not just final product)

**Examples:**
1. Client A - Dashboard redesign (Nov 2024): Showed sketch on Day 2, client wanted different layout (saved 15 hours)
2. Internal Product - Notifications (Dec 2024): Showed prototype Day 5, users wanted different UX (saved 10 hours)

**Communication template:**

```markdown
Subject: [Feature Name] - Early WIP for feedback

Hi [Client],

Quick update on [Feature Name].

**What I built so far:**
[Screenshot or Loom video showing current state]

**What works:**
- [X functionality]
- [Y interaction]

**What's still missing/ugly:**
- [Z isn't styled yet]
- [A needs real data]

**Questions for you:**
1. Is this the right direction?
2. Any concerns with [specific choice]?
3. Should I proceed or pivot?

No rush - whenever you have 5 min to review.

Thanks!
```

**Frequency guidelines:**
- Complex features: Show every 2-3 days
- Standard features: Show at 50% done
- Simple features: Show when 80% done
- Critical features: Daily updates

**Source experiences:**
- memory/episodic/projects/2024-11-18-client-a-dashboard-feedback.md
- memory/episodic/projects/2024-12-15-internal-product-notifications.md

---

## Feature Priority Matrix: Effort vs Impact

**Confidence:** MEDIUM
**Based on:** 3 examples
**Last reinforced:** 2025-01-05
**Time saved:** ~5 hours per planning session (clear prioritization)

**Context:**
When prioritizing features for roadmap or sprint planning

**Pattern:**
Plot features on 2×2 matrix: Effort (low/high) × Impact (low/high)

**Matrix:**
```
High Impact │ ⚠️  High Effort      │ ✅ Low Effort
            │ High Impact        │ High Impact
            │ (Plan carefully)   │ (DO FIRST!)
────────────┼────────────────────┼──────────────
Low Impact  │ ❌ High Effort      │ 🤔 Low Effort
            │ Low Impact         │ Low Impact
            │ (AVOID!)           │ (Maybe/Later)
            └────────────────────┴──────────────
               High Effort         Low Effort
```

**Prioritization:**
1. **Quick wins** (Low effort, high impact) - Do first
2. **Strategic** (High effort, high impact) - Plan carefully, allocate time
3. **Fill-ins** (Low effort, low impact) - Do if time permits
4. **Avoid** (High effort, low impact) - Don't do unless business reason

**Why it works:**
- Visual clarity (everyone sees priorities)
- Prevents bikeshedding (low-impact debates)
- Focuses on ROI (maximize impact per hour)
- Manages expectations (client sees why X is prioritized over Y)
- Creates alignment (shared prioritization framework)

**Examples:**
1. Client A - Q4 roadmap (Oct 2024): Identified 3 quick wins, delivered in 2 weeks
2. Internal Product - Feature backlog (Nov 2024): Deprioritized 5 low-impact features
3. Client B - Sprint planning (Jan 2025): Sequenced 2 strategic features across 3 sprints

**Exercise template:**

```markdown
## Feature Prioritization: [Project Name]

### Features to Prioritize

1. Real-time collaboration (Effort: High, Impact: High)
2. Dark mode (Effort: Low, Impact: Medium)
3. Export to PDF (Effort: Medium, Impact: High)
4. Custom themes (Effort: High, Impact: Low)
5. Keyboard shortcuts (Effort: Low, Impact: High)
6. Animated transitions (Effort: Medium, Impact: Low)

### Matrix Placement

**✅ Quick Wins (Low Effort, High Impact) - DO FIRST**
- Keyboard shortcuts (2 days, major UX improvement)
- Dark mode (3 days, frequently requested)

**⚠️  Strategic (High Effort, High Impact) - PLAN CAREFULLY**
- Real-time collaboration (3-4 weeks, competitive feature)
- Export to PDF (1 week, client requirement)

**🤔 Fill-Ins (Low Effort, Low Impact) - MAYBE LATER**
- Animated transitions (2 days, nice polish)

**❌ Avoid (High Effort, Low Impact) - DON'T DO**
- Custom themes (2-3 weeks, rare request, complex to maintain)

### Recommended Sequence

**Sprint 1 (2 weeks):**
- Keyboard shortcuts (quick win)
- Dark mode (quick win)
- Start export to PDF (strategic, 50% done)

**Sprint 2 (2 weeks):**
- Finish export to PDF (strategic)
- Plan real-time collaboration architecture

**Sprint 3-5 (6 weeks):**
- Implement real-time collaboration (strategic)

**Backlog:**
- Animated transitions (if time permits)
- Custom themes (not doing unless client pays extra)
```

**Source experiences:**
- memory/episodic/projects/2024-10-15-client-a-roadmap-planning.md
- memory/episodic/projects/2024-11-28-internal-product-backlog.md
- memory/episodic/projects/2025-01-05-client-b-sprint-planning.md

---

## Pattern Growth Tracking

**How PM patterns mature:**

1. **First project** (LOW confidence)
   - Try new approach
   - Document what happened
   - Note what worked/didn't work

2. **Second project** (MEDIUM confidence)
   - Apply pattern consciously
   - Refine based on context
   - Measure results

3. **Repeated success** (HIGH confidence, 5+ examples)
   - Pattern becomes default approach
   - Saves time (don't rethink each time)
   - Can teach to others

4. **Workflow creation** (Procedural memory)
   - HIGH confidence patterns become checklists
   - Onboarding templates
   - Client playbooks

---

**Last Updated:** 2025-01-11
**Total Patterns:** 6
**Confidence Breakdown:**
- HIGH: 0 patterns (0%)
- MEDIUM: 4 patterns (67%)
- LOW: 2 patterns (33%)

**Next steps:**
- Validate "Weekly check-ins" pattern (test with 2 more clients)
- Validate "Client onboarding checklist" (use on next 2 projects)
- Test "Feature priority matrix" 2 more times (move to HIGH confidence)
- Consider creating "Client Onboarding" workflow (procedural memory)
