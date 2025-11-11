# Client & Project Management Workflow

**Purpose:** Manage multiple client projects simultaneously with zero context-switching overhead

**Who it's for:** Technical founders, consultants, freelancers, agencies managing 3+ projects

**Time savings:** 3-5 hours/week (30 min/day on planning + context recovery)

---

## What This Use Case Provides

### 1. Multi-Project Overview
- **Command:** `/overview`
- **What it does:** Shows ALL tasks across ALL projects in one prioritized view
- **Time saved:** 30 min/day (no manual planning, no forgotten tasks)

**Output:**
```
🔴 URGENT (Do today)
- [Client A] OAuth hotfix (due today, blocking 10K users)
- [Client B] Deploy staging (due 5 PM, client demo tomorrow)

⚠️  IMPORTANT (This week)
- [Client A] Documentation update (due Friday)
- [Internal Product] Notifications feature (due Thursday)

🚧 BLOCKED (Follow up)
- [Client C] API integration (waiting for API keys from client)

⚡ QUICK WINS (Fill gaps, < 30 min)
- [Client A] Update README
- [Client B] Fix typo in email template

**Recommended focus today:**
1. Start with OAuth hotfix (urgent, client-impacting)
2. Deploy Client B staging by 5 PM
3. Fill remaining time with notifications feature
```

### 2. Zero-Overhead Project Switching
- **Command:** `/switch [project]`
- **What it does:** Loads full project context instantly
- **Time saved:** 5-10 min per switch × 5-10 switches/day = 45+ min/day

**What `/switch` loads:**
- Project context (client details, constraints, preferences)
- Recent activity (last 3 conversations, recent commits)
- Open tasks (what's in progress, what's blocked)
- Relevant patterns (project-specific + client-specific)
- Last conversation (pick up where you left off)

### 3. Client Context Preservation
- **Files:** `memory/semantic/clients/[client-name].md`
- **Captures:**
  - Technical constraints (must use PostgreSQL, GDPR compliant, etc.)
  - Communication preferences (prefers Loom videos, wants daily updates)
  - Project-specific patterns (this client always wants X)
  - What works/what to avoid with this client

---

## Installed Components

### Commands
- `/overview` - Daily task overview across all projects
- `/switch [project]` - Load project context
- `/new-project [name]` - Create new project structure
- `/add-task [project] [task]` - Add task to project

### Agents
- **project-coordinator** - Manages multi-project taskload, prioritization, health monitoring
  - Generates `/overview` output
  - Detects blocked tasks, stalled projects
  - Suggests batching strategies (work on similar tasks together)

- **chief-of-staff** - Orchestrates work across projects
  - Loads context when switching projects
  - Reminds you of commitments
  - Coordinates multiple agents

### Memory Structure
- **Projects:** `projects/[project-name]/`
  - `context.md` - Client details, constraints, preferences
  - `tasks.md` - Open tasks, completed tasks
  - `notes.md` - Meeting notes, decisions, ideas
  - `patterns.md` - Project-specific patterns

- **Clients:** `memory/semantic/clients/[client-name].md`
  - Client profile, communication style, technical constraints

- **Tracking:** `projects/CURRENT`
  - File containing slug of currently active project
  - Used by all agents to know context

---

## How to Use

### Morning Routine (5 min)

```bash
# See everything across all projects
/overview

# Output shows:
# - Urgent tasks (due today/overdue)
# - Important tasks (due this week)
# - Blocked tasks (waiting on others)
# - Quick wins (< 30 min, fill gaps)
# - Recommended focus for today

# Pick first task from URGENT section
# Task: [Client A] OAuth hotfix

# Switch to Client A project
/switch client-a

# Now you have:
# - Full Client A context loaded
# - Recent activity visible
# - Relevant patterns active
# - Ready to work (no mental recovery time)
```

### During the Day

**When switching between projects:**
```bash
# Finish task on Client A
# Need to switch to Client B

/switch client-b

# Context instantly loads:
# - Client B tech stack, constraints
# - Last conversation picked up
# - Open tasks visible
# - Client-specific patterns active

# NO mental overhead, start working immediately
```

**When new task comes in:**
```bash
# Client A emails: "Can you fix the email template typo?"

/add-task client-a "Fix typo in email template" priority=low quick_win=true

# Task added to Client A
# Will appear in next /overview under "Quick Wins"
```

### End of Day (2 min)

```bash
# Review what got done
/switch [project-you-worked-on]

# Update tasks.md (mark completed tasks)
# Run /learn if extracted any patterns

# Tomorrow morning: /overview will show fresh prioritized list
```

---

## Project Structure

### Creating New Project

```bash
/new-project "Client A - E-commerce Platform"

# Creates:
projects/client-a-ecommerce/
├── context.md          # Client details, constraints, goals
├── tasks.md           # Task tracking
├── notes.md           # Meeting notes, decisions
└── patterns.md        # Project-specific patterns

# Also creates:
memory/semantic/clients/client-a.md  # Client profile

# Sets as current:
projects/CURRENT → "client-a-ecommerce"
```

### Context File Example

**File:** `projects/client-a/context.md`

```markdown
# Client A - E-commerce Platform

## Client Details
- Industry: E-commerce (handmade jewelry)
- Size: 5-person startup
- Contact: Jane Doe (jane@clienta.com)
- Status: Active (started Oct 2024)

## Technical Constraints
- Must use: Python/Django, PostgreSQL, React
- Cannot use: Microservices (team too small)
- Performance: < 200ms API response, 1K concurrent users
- Compliance: GDPR (EU customers)

## Communication Preferences
- Prefers: Loom videos over written updates
- Meeting: Friday 3 PM weekly check-in
- Response time: 4-hour SLA for urgent issues

## What Works
- Show work-in-progress early (prevents rework)
- Frame technical decisions as business trade-offs
- Weekly check-ins keep client happy

## What to Avoid
- Don't use technical jargon without explanation
- Don't surprise with technical decisions (consult first)

## Project-Specific Patterns
- Always index foreign keys (learned from slow queries)
- Use select_related() for product queries (N+1 issue)
- Client prefers verbose logging for debugging
```

---

## Fibery Integration (Optional, Week 2)

**What it does:**
- Bi-directional sync between second brain and Fibery
- `/add-task` creates Fibery task automatically
- Fibery task completion syncs back to second brain

**Setup:**
```bash
# Install Fibery integration
cd use-cases/client-pm-workflow/integrations/fibery/
npm install

# Configure API key
cp .env.example .env
# Edit .env: FIBERY_API_KEY=your-key

# Test sync
node sync.js --dry-run

# Enable automatic sync (runs every 15 min)
cron: */15 * * * * node /path/to/sync.js
```

**How it works:**
```bash
# You run:
/add-task client-a "Deploy OAuth fix" priority=high due=2025-01-12

# Second brain:
# 1. Adds task to projects/client-a/tasks.md
# 2. Calls Fibery API
# 3. Creates task in Fibery (Client A workspace)

# Fibery:
# - Task appears in Client A board
# - You (or team) can update in Fibery UI
# - Mark as complete in Fibery

# Sync script (every 15 min):
# - Checks Fibery for completed tasks
# - Updates projects/client-a/tasks.md
# - Marks completed locally too

# Result: Single source of truth, work in either system
```

---

## Multi-Project Strategies

### Strategy 1: Context Batching

**Problem:** Switching between projects 10+ times/day is exhausting

**Solution:** Batch work by project (1-2 hour blocks)

**Example day:**
- 9-11 AM: Client A (OAuth fix + documentation)
- 11-12 PM: Client B (staging deploy + email fix)
- 1-3 PM: Internal Product (notifications feature)
- 3-4 PM: Client C (API integration if unblocked)

**How `/overview` helps:**
- Groups tasks by project
- Shows time estimates
- Suggests batching opportunities

### Strategy 2: Quick Win Filling

**Problem:** 15-minute gaps between meetings (too short for deep work)

**Solution:** Use `/overview` quick wins section

**Example:**
```
You have 20 min before next meeting.

⚡ QUICK WINS (< 30 min):
- [Client A] Update README (10 min)
- [Client B] Fix typo in template (5 min)
- [Internal] Review analytics dashboard (15 min)

Pick the 15-min task → makes progress → doesn't waste time
```

### Strategy 3: Blocked Task Management

**Problem:** Waiting for client input, task can't progress

**Solution:** `/add-task` with `blocked=true`, `/overview` reminds you to follow up

**Example:**
```bash
/add-task client-c "API integration" blocked=true

# /overview shows:
🚧 BLOCKED (Follow up):
- [Client C] API integration (blocked 3 days - FOLLOW UP!)
```

---

## Success Metrics

**Track in `brain-health/metrics.md`:**

### Project Health
- **Active projects:** 5
- **Stalled projects:** 0 (no activity in 7+ days)
- **Blocked tasks:** 2 (down from 5)
- **On-time delivery rate:** 90%

### Time Savings
- **Daily planning:** 30 min → 5 min (25 min saved, `/overview`)
- **Context switching:** 10 min → 30 sec per switch (47 min saved × 5 switches)
- **Total:** ~1 hour/day saved (5 hours/week)

### Weekly Tracking
```markdown
## Week of Jan 6-12

**Projects active:** 5
- Client A: 15 hours (OAuth fix, docs update)
- Client B: 10 hours (staging deploy, features)
- Client C: 5 hours (API integration - unblocked Tue)
- Internal Product: 8 hours (notifications feature)
- Learning: 2 hours (Redis caching research)

**Tasks completed:** 18
**Tasks added:** 12
**Context switches:** 35 (down from 50 last week)

**Time saved:** ~6 hours (planning + switching automation)
```

---

## Common Workflows

### Workflow 1: New Client Onboarding

1. `/new-project "Client D - Analytics Platform"`
2. Fill `context.md` (client details, constraints)
3. Copy from `memory/semantic/clients/client-template.md`
4. First meeting: capture requirements in `notes.md`
5. Create initial tasks: `/add-task client-d "Setup repo" priority=high`
6. Switch to project: `/switch client-d`
7. Start work

### Workflow 2: Daily Multi-Project Juggling

1. Morning: `/overview` (see all tasks)
2. Pick urgent task from Client A
3. `/switch client-a` → work → complete
4. Next: Important task from Client B
5. `/switch client-b` → work → complete
6. Fill gap: Quick win from Internal Product
7. `/switch internal-product` → complete in 15 min
8. End of day: Mark tasks complete, update notes

### Workflow 3: Weekly Client Check-in

1. Friday 2:45 PM: Prepare for Client A check-in (3 PM)
2. `/switch client-a`
3. Review `notes.md` (what happened this week)
4. Check `tasks.md` (what's completed, what's next)
5. 3 PM: Meeting (discuss progress, blockers)
6. After meeting: Update `notes.md` with decisions
7. Add new tasks if any: `/add-task client-a ...`

---

## Next Steps

### Week 1: Setup
- [ ] Create 2-3 active projects (`/new-project`)
- [ ] Fill context.md for each project
- [ ] Add current tasks (`/add-task`)
- [ ] Run `/overview` daily (build habit)

### Week 2: Optimize
- [ ] Set up Fibery integration (if using Fibery)
- [ ] Track time saved (planning + context switching)
- [ ] Refine project context (what's working, what's missing)

### Week 3: Scale
- [ ] Add remaining projects (if you have 5+)
- [ ] Create client profiles for all clients
- [ ] Document project-specific patterns

### Week 4: Automate
- [ ] Weekly client update automation (from process-ideas.md)
- [ ] Automated task sync (Fibery/GitLab/etc.)
- [ ] Metrics dashboard (track project health)

---

**Related use cases:**
- Dev Workflow (code review for client projects)
- Idea Capture Workflow (capture client feature requests)
- Content Chain Workflow (create case studies from client work)

**Related files:**
- `.claude/agents/project-coordinator.md` - Multi-project coordination agent
- `.claude/agents/chief-of-staff.md` - Context management orchestration
- `memory/semantic/patterns/pm-patterns.md` - Project management patterns
