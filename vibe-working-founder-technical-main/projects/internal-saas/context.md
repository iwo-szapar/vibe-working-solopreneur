# Internal SaaS - AI-Powered Project Management Tool

**Project Type:** Internal Product / Startup
**Status:** Pre-Launch MVP Development
**Timeline:** Sep 2025 - Jan 2026 (5 months)
**Last Updated:** 2025-11-10

---

## Product Overview

**Product Name:** FlowStack (working name)
**Tagline:** "Project management that learns from your workflow"

**Elevator Pitch:**
AI-powered project management tool that automatically suggests tasks, detects blockers, and learns from your team's patterns to optimize workflows.

**Target Market:**
- Small technical teams (5-20 people)
- Startups and agencies
- Remote-first teams

**Differentiation:**
- AI-driven task suggestions (GPT-4 analyzes project context)
- Automatic blocker detection (analyzes task dependencies + velocity)
- Learning system (improves suggestions based on team patterns)
- Clean, fast UI (no feature bloat like Jira)

---

## Business Model

**Pricing:**
- Free: 1 project, 5 users
- Pro: $15/user/month - Unlimited projects, AI features
- Team: $25/user/month - Advanced analytics, integrations

**Revenue Goals:**
- Month 1: $500 MRR (10 paying teams)
- Month 6: $5,000 MRR (100 paying teams)
- Year 1: $50,000 MRR (1000 paying teams)

**Launch Plan:**
- Beta: Dec 2025 (Product Hunt launch)
- General Availability: Jan 2026
- First 100 users free Pro forever (beta discount)

---

## Tech Stack

**Frontend:**
- React 18 with TypeScript
- Next.js 14 (App Router + Server Actions)
- TailwindCSS + Radix UI
- Zustand (state management)
- React Query (data fetching)

**Backend:**
- Node.js 20 with Fastify
- PostgreSQL 15 (Supabase hosted)
- Redis (Upstash - serverless)
- Prisma ORM

**AI/ML:**
- OpenAI GPT-4 API (task suggestions)
- OpenAI Embeddings (semantic task search)
- Pinecone (vector database for embeddings)

**Infrastructure:**
- Vercel (frontend hosting)
- Railway (backend hosting)
- Supabase (PostgreSQL + Auth + Realtime)
- Upstash (Redis)
- PostHog (product analytics)

**Payments:**
- Stripe (subscriptions)
- Lemon Squeezy (alternative, simpler tax handling)

---

## Current Phase

**Phase: MVP Development (80% complete)**

**Completed:**
- ✅ Authentication (Supabase Auth)
- ✅ Project CRUD
- ✅ Task CRUD with drag-and-drop kanban
- ✅ Team management (invite, roles)
- ✅ Basic task comments
- ✅ File uploads (Supabase Storage)

**In Progress:**
- 🔄 AI task suggestions (GPT-4 integration)
- 🔄 Blocker detection algorithm
- 🔄 Email notifications (task assignments, mentions)
- 🔄 Stripe subscription flow

**Upcoming (Pre-Launch):**
- ⏳ AI learning system (analyze completed tasks)
- ⏳ Task templates
- ⏳ Time tracking
- ⏳ Product Hunt launch assets

---

## Constraints & Challenges

**Technical Constraints:**
- Keep costs low (< $500/month pre-revenue)
- Response time < 1 second (fast UX)
- AI suggestions < 3 seconds
- Realtime updates (collaborative editing)

**Business Constraints:**
- Solo founder (no team, wearing all hats)
- Nights & weekends only (20 hours/week)
- Limited marketing budget ($1000 for Product Hunt)
- Need to launch by Jan 2026 (runway concern)

**Current Blockers:**
- AI suggestion quality inconsistent (GPT-4 prompt needs tuning)
- Realtime updates causing race conditions
- Stripe webhook testing difficult locally
- Need beta testers (only 3 friends testing so far)

---

## Key Decisions

**Architecture Decisions:**
1. **Why Supabase?** PostgreSQL + Auth + Realtime + Storage all in one, generous free tier
2. **Why Fastify over Express?** 2x faster, better TypeScript support, built-in validation
3. **Why Vercel + Railway?** Easy deployment, generous free tiers, scale later if needed
4. **Why GPT-4 over GPT-3.5?** Better reasoning for complex task suggestions (worth $$ cost)

**Product Decisions:**
1. **Focus on AI features** - Differentiation, not just another PM tool
2. **No mobile app yet** - Desktop-first, mobile web responsive
3. **Opinionated workflow** - Kanban only (no Gantt, calendar views)
4. **Async-first** - No real-time chat (use Slack/Discord, we integrate)

**Trade-offs:**
- Using managed services (Supabase, Railway) vs self-hosted (cheaper long-term, more control)
- AI features (differentiation) vs simple PM tool (faster to build, lower costs)
- Subscription model (recurring revenue) vs one-time (easier sell)

---

## Solo Founder Reality

**Time Allocation (20 hrs/week):**
- Development: 14 hours (70%)
- Marketing/Content: 3 hours (15%)
- Customer support/testing: 2 hours (10%)
- Admin (taxes, legal): 1 hour (5%)

**Weekly Schedule:**
- Mon/Wed/Fri: 2 hours before day job (6am-8am)
- Sat: 6 hours (focused dev)
- Sun: 4 hours (polish, testing, content)

**Energy Management:**
- High-energy work (AI features, complex logic): Saturday mornings
- Low-energy work (styling, docs): Weekday mornings
- Creative work (marketing, content): Sunday afternoons

**Challenges:**
- Context switching (3 client projects + this)
- Motivation dips (working alone is hard)
- Shipping speed (20 hrs/week feels slow)
- Burnout risk (60+ hour weeks total)

---

## Success Metrics

**Technical KPIs:**
- Page load time < 1s (Lighthouse score > 90)
- AI suggestion time < 3s
- Task creation → saved < 500ms
- 99.9% uptime (Vercel + Railway)

**Product KPIs:**
- 100 beta signups by Dec 1
- 10 paying customers by Jan 1
- 50% weekly active users (beta)
- 80% feature satisfaction (NPS > 40)

**Business KPIs:**
- $500 MRR by Month 1
- $5,000 MRR by Month 6
- Churn < 5% monthly
- CAC < $50 (organic growth focus)

**Personal KPIs:**
- Launch by Jan 2026 (non-negotiable)
- Work-life balance maintained (< 25 hrs/week)
- No burnout (weekly check-ins with accountability buddy)

---

## Resources

**Documentation:**
- Product spec: `/docs/product-spec.md`
- API docs: FastAPI auto-generated (`/docs`)
- Architecture: `/docs/architecture.md`
- AI system: `/docs/ai-suggestions.md`

**Repositories:**
- Frontend: `github.com/luca/flowstack-ui` (private)
- Backend: `github.com/luca/flowstack-api` (private)
- Landing page: `github.com/luca/flowstack-marketing` (private)

**Design:**
- Figma: Design system + mockups
- Logo: Designed by freelancer on Fiverr

**Community:**
- Discord: Beta tester community (3 members)
- Twitter: @flowstackpm (80 followers)

---

## Notes & Learnings

**What's Going Well:**
- Supabase is amazing (Auth + DB + Realtime + Storage in one)
- Next.js Server Actions reducing API boilerplate
- GPT-4 task suggestions better than expected (when prompt is good)
- Early beta users love the speed and simplicity

**What's Challenging:**
- AI prompt engineering is an art (inconsistent results)
- Realtime collaborative editing is hard (race conditions, conflicts)
- Solo founder loneliness (no one to bounce ideas off)
- Balancing client work + this (context switching exhausting)

**Patterns Discovered:**
- GPT-4 prompt structure for task suggestions
- Supabase Realtime optimistic updates pattern
- Stripe webhook handling (idempotency, retries)
- Next.js Server Actions for mutations (simpler than API routes)

**Personal Insights:**
- Early mornings (6am-8am) most productive for me
- Shipping imperfect features > waiting for perfect
- Talking to users weekly keeps me motivated
- Need to batch tasks (1 day UI, 1 day backend, not mixed)

---

## AI Features Details

### 1. AI Task Suggestions

**How it works:**
1. User creates project with description
2. GPT-4 analyzes project context + existing tasks
3. Suggests 5-10 tasks to add
4. User can accept, edit, or ignore suggestions

**Prompt Template:**
```
You are a project management assistant. Analyze this project and suggest tasks.

Project: {project_name}
Description: {project_description}
Existing tasks: {task_list}

Suggest 5-10 tasks that would help complete this project. Be specific and actionable.
Format: JSON array with {title, description, priority}
```

**Current Accuracy:** 70% accepted (need to improve)

---

### 2. Blocker Detection

**How it works:**
1. Analyze task dependencies (A blocks B)
2. Detect tasks stuck in "In Progress" > 7 days
3. Check velocity (if task usually takes 2 days, now 8 days = blocker)
4. Alert team + suggest remedies

**Detection Rules:**
- Task "In Progress" > 2x average completion time
- Task with dependencies but dependency not started
- Task with no activity (comments, updates) in 5+ days

**Current Status:** 60% implemented (detection works, remedies needed)

---

### 3. AI Learning System (Planned)

**How it works:**
1. Track completed tasks (context, time taken, who completed)
2. Build embeddings for tasks (OpenAI Embeddings API)
3. Store in Pinecone vector DB
4. When suggesting new tasks, find similar past tasks
5. Use past data to improve suggestions (time estimates, dependencies)

**Expected Impact:**
- More accurate time estimates (based on team's past performance)
- Better task suggestions (learns team's patterns)
- Smarter blocker detection (learns what usually blocks this team)

**Launch Timeline:** Phase 2 (post-launch, Feb 2026)

---

## Current Sprint (Week 10)

**Sprint Goal:** Complete AI task suggestions + Stripe integration

**This Week (Nov 10-16):**
1. Improve GPT-4 prompt for task suggestions (accuracy 70% → 85%)
2. Implement Stripe subscription flow (checkout, webhooks)
3. Build pricing page
4. Email notifications (task assignments)
5. Find 10 more beta testers (Reddit, Indie Hackers)

**Next Week (Nov 17-23):**
1. Blocker detection algorithm completion
2. Product Hunt launch assets (graphics, video)
3. Beta testing with 10 users (collect feedback)
4. Fix top 5 bugs from beta feedback

**Pre-Launch (Dec 1-15):**
1. Final polish (animations, empty states, error handling)
2. Documentation (help center, video tutorials)
3. Product Hunt launch prep (hunter outreach, launch day plan)
4. Press outreach (TechCrunch, Indie Hackers)

---

## Competitors

**Direct Competitors:**
- Linear (too expensive, too complex)
- Asana (feature bloat, slow)
- ClickUp (overwhelming UI)
- Notion (not purpose-built for PM)

**Why We'll Win:**
1. AI-first (they're bolting on AI, we're building for it)
2. Speed (no feature bloat, fast load times)
3. Price (cheaper than Linear/Asana)
4. Solo founder agility (ship fast, listen to users)

---

## Personal Context

**Why I'm Building This:**
- Frustrated with existing PM tools (slow, bloated)
- Opportunity: AI + PM unexplored (most tools = basic ChatGPT integration)
- Financial: Need revenue stream beyond client work
- Learning: Want to build end-to-end product (design → dev → marketing → sales)

**Success Definition:**
- $5k MRR by Month 6 (replace 1 client, work less)
- 1000+ users finding value (impact)
- Proven I can ship a product solo (confidence boost)
- Exit strategy: Acquire customers, sell to bigger player (Motion, Linear) or keep as cash flow business

**If It Fails:**
- Learnings: AI integration, SaaS business, marketing
- Portfolio: Proof I can ship (helps with future projects)
- Network: Connections with other founders, beta users

---

**Project Start Date:** 2025-09-01
**Beta Launch:** 2025-12-15 (Product Hunt)
**General Availability:** 2026-01-15
**Personal Investment:** $5,000 (dev tools, freelance design, marketing)
