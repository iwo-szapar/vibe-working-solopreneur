# Product Vision - [Product Name]

**Template for your internal product development**

---

## Product Overview

**Product Name:** [Your SaaS / internal product name]
**Tagline:** [One sentence - what it does and for whom]

**Example:**
"ProjectFlow - Task management for technical founders managing multiple client projects"

**Vision (3-5 years):**
[What you want this product to become in the long term]

**Example:**
"The operating system for solo technical founders and 2-5 person dev shops, managing everything from client work to internal projects with zero context-switching overhead."

---

## The Problem

### Who Has This Problem?

**Primary persona:**
- **Who:** [e.g., Technical founders, CTOs at small startups, solo consultants]
- **Size:** [e.g., 1-5 person teams]
- **Industry:** [e.g., Software consulting, SaaS, agencies]

**Example:**
- **Who:** Technical founders managing 3-5 client projects simultaneously
- **Size:** 1-3 person development teams
- **Industry:** Software consulting and product development

### What's the Problem?

**Pain points:**
1. [Pain point 1 - Specific problem]
2. [Pain point 2 - Specific problem]
3. [Pain point 3 - Specific problem]

**Example:**
1. Context switching between client projects wastes 30+ min/day
2. Forgetting project-specific details when switching contexts
3. Manually tracking tasks across multiple tools (Fibery, GitLab, Pipedrive)
4. No daily overview of all projects - hard to prioritize
5. Reinventing same solutions because patterns aren't captured

### Current Alternatives (and Why They Fail)

**Alternative 1: [Tool/approach]**
- What it does: [Description]
- Why it fails: [Gap or limitation]

**Example:**
**Alternative 1: Notion / Obsidian**
- What it does: Note-taking and knowledge management
- Why it fails: Passive storage, doesn't actively help you work. No AI agent coordination. No multi-project awareness.

**Alternative 2: Traditional project management (Jira, Asana)**
- What it does: Task tracking
- Why it fails: No context preservation, no knowledge compounding, no AI assistance

**Alternative 3: Just using Claude/ChatGPT**
- What it does: Answer questions, write code
- Why it fails: No memory across sessions, no project-specific context, can't coordinate multiple tasks

---

## The Solution

### What We're Building

**Core concept:**
[1-2 sentences explaining the solution]

**Example:**
"A 'second brain' system that preserves context across all your projects, compounds learnings into reusable patterns, and uses AI agents to coordinate multi-project work automatically."

### Key Features (MVP)

**Must-have for launch:**

1. **Multi-project context switching**
   - Description: Switch between projects with zero mental overhead
   - Value: Saves 30 min/day on context recovery
   - How: `/switch [project]` loads full context immediately

2. **Daily overview across all projects**
   - Description: See all tasks, deadlines, blockers in one view
   - Value: Saves 20 min/day on manual planning
   - How: `/overview` generates prioritized task list

3. **Knowledge compounding**
   - Description: Extract patterns from completed work
   - Value: Next time is 2-5x faster for similar tasks
   - How: `/learn` extracts patterns after each task

4. **[Add your other core features]**

### Phase 2 Features (Post-launch)

- [Feature A - e.g., Voice idea capture]
- [Feature B - e.g., Content chain generation]
- [Feature C - e.g., Automated client reporting]

---

## Target Market

### Primary Market

**Size:** [Market size estimate]
**Geography:** [Where they are]
**Characteristics:**
- [Characteristic 1]
- [Characteristic 2]
- [Characteristic 3]

**Example:**
**Size:** 100,000+ solo/small dev consultancies worldwide
**Geography:** Global (English-speaking markets first)
**Characteristics:**
- Managing 3-5 client projects simultaneously
- 1-5 person teams (technical founders + small team)
- Revenue: $100K-$500K/year
- Pain: Context switching, overwhelm, inefficiency

### How to Reach Them

**Channels:**
- [Channel 1 - e.g., LinkedIn (technical founder communities)]
- [Channel 2 - e.g., Dev.to, Hacker News (technical content)]
- [Channel 3 - e.g., Indie Hackers (product launches)]

**Content strategy:**
- [Strategy 1 - e.g., Case studies of time saved]
- [Strategy 2 - e.g., Technical tutorials on building second brain]
- [Strategy 3 - e.g., Open-source components to build community]

**Example:**
**Channels:**
- LinkedIn (share client success stories, productivity tips)
- Dev.to (technical deep-dives on Claude Code, second brain architecture)
- YouTube (screencasts showing real workflows)
- Indie Hackers (product launch, revenue transparency)

**Content strategy:**
- Blog post series: "Technical Founder Productivity"
- Video series: "Day in the life using second brain"
- Open-source: Share second brain templates on GitHub

---

## Success Metrics

### Product Metrics

**Activation:** [What counts as "activated user"]
**Example:** User creates 2+ projects and runs `/overview` 3+ times

**Engagement:** [What counts as "engaged"]
**Example:** User switches between projects 5+ times/week, runs `/learn` 2+ times/week

**Retention:** [What counts as "retained"]
**Example:** User active 3+ days/week for 4 consecutive weeks

**North Star Metric:** [Single metric that indicates success]
**Example:** Hours saved per user per week (target: 5+ hours)

### Business Metrics

**MRR Target (Year 1):** $[Amount]
**Pricing:** $[Amount]/month per user
**Churn target:** < [X]% monthly churn

**Customer acquisition:**
- CAC target: $[Amount]
- LTV target: $[Amount]
- LTV:CAC ratio: [Target ratio, e.g., 3:1]

**Growth targets:**
- Month 3: [X] users
- Month 6: [X] users
- Month 12: [X] users

---

## Competitive Landscape

### Direct Competitors

**Competitor 1: [Name]**
- **What they do:** [Description]
- **Strengths:** [What they do well]
- **Weaknesses:** [Where they fall short]
- **Our advantage:** [How we're different/better]

**Example:**
**Competitor 1: Notion AI**
- **What they do:** AI-powered workspace with note-taking, databases, tasks
- **Strengths:** Beautiful UI, flexible structure, large user base
- **Weaknesses:** Generic AI (not specialized for dev workflows), no multi-project context switching, passive not proactive
- **Our advantage:** Specialized for technical founders, active AI agents, knowledge compounding, project-specific context

### Indirect Competitors

**Category 1: Project management tools (Jira, Asana, Linear)**
- **Why people use them:** Task tracking, team collaboration
- **Why we're different:** We add AI coordination, knowledge compounding, context preservation

**Category 2: Note-taking (Obsidian, Roam, Logseq)**
- **Why people use them:** Knowledge management, second brain
- **Why we're different:** Active agents, not passive notes. Project-aware, not just personal.

**Category 3: AI coding assistants (GitHub Copilot, Cursor)**
- **Why people use them:** Code generation, autocomplete
- **Why we're different:** Full project coordination, not just code completion. Multi-project management.

---

## Product Principles

**Design principles:**

1. **[Principle 1]**
   - What it means: [Explanation]
   - In practice: [How it guides decisions]

**Example:**
1. **Invisible until needed**
   - What it means: System works in background, surfaces insights proactively
   - In practice: Don't require explicit "save" or "tag" - learn from work automatically

2. **Zero context-switching tax**
   - What it means: Switching between projects should be instant, no mental overhead
   - In practice: `/switch` loads full context - last conversation, open tasks, relevant patterns

3. **Compound, don't accumulate**
   - What it means: Knowledge should distill into patterns, not pile up as noise
   - In practice: Patterns tracked by confidence (low → medium → high), not infinite list

---

## Roadmap

### Phase 1: MVP (Months 1-3)

**Goal:** Working second brain for solo technical founder

**Features:**
- [ ] Multi-project management (`/new-project`, `/switch`)
- [ ] Daily overview (`/overview`)
- [ ] Knowledge compounding (`/learn`)
- [ ] Pattern tracking (semantic memory)
- [ ] Experience capture (episodic memory)
- [ ] Chief of Staff agent (coordination)

**Success criteria:**
- 1 power user (Luca) using daily
- Saves 5+ hours/week
- 10+ patterns extracted
- 3+ projects managed

---

### Phase 2: Integration & Automation (Months 4-6)

**Goal:** Integrate with existing tools, reduce manual work

**Features:**
- [ ] Fibery integration (bi-directional task sync)
- [ ] GitLab integration (auto-learn from commits)
- [ ] Voice idea capture (Google Drive + transcription)
- [ ] Content chain workflow (blog → LinkedIn → video)
- [ ] Code review automation (agent-driven)

**Success criteria:**
- 3 power users
- Saves 8+ hours/week per user
- Voice capture working (2 hr/week saved on idea capture)
- 1 content chain created per week

---

### Phase 3: Polish & Distribution (Months 7-9)

**Goal:** Make it easy for others to adopt

**Features:**
- [ ] Setup wizard (choose your persona: technical founder, PM, content creator)
- [ ] Pre-built use cases (10+ workflows)
- [ ] Documentation (comprehensive guides)
- [ ] Video tutorials (5-10 screencasts)
- [ ] Template marketplace (share/discover workflows)

**Success criteria:**
- 10 active users (beyond Luca)
- Public launch (Indie Hackers, Hacker News, Product Hunt)
- 100+ GitHub stars (if open-source components)
- First paying customer

---

### Phase 4: SaaS Product (Months 10-12)

**Goal:** Turn into proper SaaS product

**Features:**
- [ ] Web UI (dashboard for viewing brain health, patterns)
- [ ] Team features (shared patterns, collaborative projects)
- [ ] Analytics dashboard (time saved, pattern growth, usage stats)
- [ ] Billing integration (Stripe)
- [ ] Onboarding flow (new user setup)

**Success criteria:**
- 50 paying users
- $2,500 MRR
- < 5% monthly churn
- 5+ testimonials / case studies

---

## Key Decisions & Assumptions

### Assumptions (to validate)

1. **[Assumption 1]**
   - What we assume: [Statement]
   - How to validate: [Test/experiment]
   - Invalidation criteria: [What would prove us wrong]

**Example:**
1. **Technical founders will invest time to set up second brain**
   - What we assume: 2-hour setup is acceptable for 5+ hr/week savings
   - How to validate: Track setup completion rate with 10 beta users
   - Invalidation criteria: < 50% complete setup → need to reduce friction

2. **Knowledge compounding delivers value**
   - What we assume: Extracting patterns after work saves time next time
   - How to validate: Measure time for similar tasks over 12 weeks
   - Invalidation criteria: No measurable time savings after 10+ pattern applications

### Strategic Decisions

**Decision 1: [Decision]**
- **Date:** YYYY-MM-DD
- **Context:** [Why this decision was needed]
- **Decision:** [What was chosen]
- **Rationale:** [Why]

**Example:**
**Decision 1: Build for solo founders first, teams later**
- **Date:** 2025-01-10
- **Context:** Could build for solo or teams initially
- **Decision:** Focus on solo technical founders (1-3 person teams) for MVP
- **Rationale:**
  - Simpler use case (no permissions, sharing, collaboration features)
  - Faster validation (fewer features to build)
  - Luca is solo founder (dogfood our own product)
  - Teams features add 3-6 months (delay launch)
  - Can expand to teams in Phase 4

---

## Resources & Links

**Product documentation:**
- [Link to product requirements doc]
- [Link to design files / Figma]
- [Link to technical architecture]

**Research & Validation:**
- [Link to user interviews]
- [Link to competitive analysis]
- [Link to market sizing]

**Codebase:**
- [GitHub repository]
- [Documentation site]

**Marketing:**
- [Landing page]
- [Blog]
- [Social media]

---

**Last Updated:** YYYY-MM-DD
**Product Status:** [Concept / MVP Development / Beta / Launched]
**Next Milestone:** [What's next]

**Related files:**
- memory/semantic/patterns/ (patterns learned from using this product)
- memory/episodic/projects/ (development milestones for this product)
- projects/internal-product/ (current tasks and roadmap)
