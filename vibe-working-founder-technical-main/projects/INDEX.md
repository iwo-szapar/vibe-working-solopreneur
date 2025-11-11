# Projects Directory

**Purpose:** Track all active projects with full context preservation for zero-overhead switching

---

## Current Project

**File:** `projects/CURRENT`

This file contains the slug of the currently active project. All agents read this to know which project context to load.

**Example:**
```
client-a-ecommerce
```

**Update with:** `/switch [project-name]`

---

## Project Structure

Each project has this structure:

```
projects/
└── [project-slug]/
    ├── context.md      # Client details, constraints, preferences
    ├── tasks.md        # Task tracking (open, completed, blocked)
    ├── notes.md        # Meeting notes, decisions, ideas
    └── patterns.md     # Project-specific patterns
```

---

## Active Projects

### Client Projects

**Client A - E-commerce Platform**
- **Slug:** `client-a-ecommerce`
- **Status:** Active
- **Start date:** 2024-10-15
- **Primary contact:** jane@clienta.com
- **Tech stack:** Python/Django, PostgreSQL, React
- **Deliverable:** E-commerce platform for handmade jewelry
- **Timeline:** MVP by 2025-03-01

**Client B - Analytics Dashboard**
- **Slug:** `client-b-analytics`
- **Status:** Active
- **Start date:** 2024-11-01
- **Primary contact:** john@clientb.com
- **Tech stack:** Python/FastAPI, PostgreSQL, React
- **Deliverable:** Real-time analytics dashboard
- **Timeline:** Beta by 2025-02-15

**Client C - Mobile App**
- **Slug:** `client-c-mobile`
- **Status:** Blocked (waiting for API keys)
- **Start date:** 2024-12-10
- **Primary contact:** sara@clientc.com
- **Tech stack:** React Native, Node.js/Express, MongoDB
- **Deliverable:** Mobile app for fitness tracking
- **Timeline:** Alpha by 2025-02-28

### Internal Projects

**Internal Product - SaaS Platform**
- **Slug:** `internal-product-saas`
- **Status:** Active (dogfooding)
- **Start date:** 2024-09-01
- **Tech stack:** Python/FastAPI, PostgreSQL, React, Redis
- **Deliverable:** Second Brain SaaS product
- **Timeline:** MVP by 2025-04-01

**Learning & Experimentation**
- **Slug:** `learning-experiments`
- **Status:** Active (ongoing)
- **Start date:** 2024-08-01
- **Purpose:** Test new technologies, build skills, R&D
- **Current focus:** Redis caching, WebSockets, real-time features

**Test Command Validation**
- **Slug:** `test-command-validation`
- **Status:** Active (testing)
- **Start date:** 2025-11-10
- **Purpose:** Validate /new-project command functionality
- **Current focus:** Command testing suite

---

## Creating New Project

### Using Command

```bash
/new-project "Client D - Payment Gateway"

# Creates:
# - projects/client-d-payment-gateway/
# - All structure files (context.md, tasks.md, notes.md, patterns.md)
# - Sets as current project (updates projects/CURRENT)
# - Creates client profile: memory/semantic/clients/client-d.md
```

### Manually

1. Create project directory:
   ```bash
   mkdir -p projects/my-new-project
   ```

2. Copy template files:
   ```bash
   cp projects/_template/* projects/my-new-project/
   ```

3. Fill in `context.md`:
   - Client details
   - Technical constraints
   - Communication preferences
   - Project goals

4. Set as current:
   ```bash
   echo "my-new-project" > projects/CURRENT
   ```

5. Create client profile:
   ```bash
   cp memory/semantic/clients/client-template.md \
      memory/semantic/clients/my-client.md
   ```

---

## Switching Projects

### Using Command

```bash
# Switch to Client A
/switch client-a-ecommerce

# Loads:
# - projects/client-a-ecommerce/context.md
# - Recent activity (last 3 conversations)
# - Open tasks from tasks.md
# - Client-specific patterns
# - Updates projects/CURRENT
```

### What Happens

1. **Context loaded:** Full client details, constraints, preferences
2. **Tasks visible:** Open tasks, blocked tasks, completed tasks
3. **Patterns active:** Project-specific and client-specific patterns
4. **Agents aware:** All agents now work in Client A context
5. **Ready to work:** No mental recovery time, start immediately

---

## Project Health Monitoring

### Health Indicators

**✅ Healthy project:**
- Activity within last 3 days
- Tasks progressing
- No blockers
- Client communication active

**⚠️  Warning signs:**
- No activity in 5-7 days
- Multiple blocked tasks
- Missed deadlines
- Scope creep detected

**🔴 Critical:**
- No activity in 10+ days (stalled)
- Production issues
- Client escalation
- Contractual risks

### Automatic Health Checks

**Project Coordinator agent monitors:**
- Days since last activity
- Blocked tasks duration
- Task completion velocity
- Deadline proximity

**Alerts in `/overview`:**
```
⚠️  Project Health Alerts:
- Client C: Blocked 3 days (API keys) - FOLLOW UP
- Learning: No activity 6 days - time to schedule learning session?
```

---

## Project Templates

### Standard Web Project

**Tech stack:** Python/FastAPI or Django, PostgreSQL, React
**Structure:** Backend API + Frontend SPA
**Common patterns:**
- JWT authentication
- PostgreSQL with indexes on foreign keys
- Error logging with user context
- CI/CD via GitLab
- Deployed on AWS ECS

**Use for:** Most client web applications

### Mobile Project

**Tech stack:** React Native, Node.js/Express, MongoDB or PostgreSQL
**Considerations:**
- Longer JWT expiry (15-20 min for mobile)
- Offline support (local storage, sync)
- Push notifications
- App store deployment

**Use for:** Mobile apps (iOS/Android)

### Internal/Experimental Project

**Tech stack:** Flexible (test new technologies)
**Purpose:** Learning, R&D, prototyping
**Lower standards:** Can be messy, focus on learning

**Use for:** Skill development, technology evaluation

---

## Multi-Project Strategies

### Time Blocking by Project

**Example schedule:**
- **Mon 9-12:** Client A (deep work block)
- **Mon 1-3:** Client B (feature development)
- **Tue 9-12:** Internal Product (product development)
- **Tue 1-4:** Client C (mobile development)
- **Wed:** No meetings, split time based on priority
- **Thu-Fri:** Client A + Client B (finishing tasks)

**Benefits:**
- Reduced context switching (1-2 hour blocks)
- Deep work possible (not interrupted)
- Predictable schedule (clients know when you're available)

### Priority-Based Switching

**Use `/overview` to prioritize:**
1. **Urgent tasks** (do first, regardless of project)
2. **Important tasks** (schedule into project blocks)
3. **Quick wins** (fill gaps between meetings)
4. **Blocked tasks** (follow up, unblock)

### Batching Similar Work

**Group by activity type:**
- **Code reviews:** Review all projects' code at once (3 PM daily)
- **Client emails:** Batch responses (11 AM, 4 PM)
- **Deployment:** Deploy all projects on Friday afternoon
- **Planning:** Sunday evening `/overview` for week ahead

---

## Integration with Other Systems

### Fibery (Week 2)

**Bi-directional sync:**
- `/add-task` creates Fibery task
- Fibery completion syncs back
- Single source of truth

**Setup:** `use-cases/client-pm-workflow/integrations/fibery/`

### GitLab (Week 4)

**Auto-learning from commits:**
- Merge request triggers `/learn`
- Commit messages linked to episodic memory
- Patterns extracted from code changes

**Setup:** `.gitlab-ci.yml` in each project repo

---

## Best Practices

### 1. Context.md is King

**Always update context.md when:**
- Technical constraints change
- Communication preferences update
- New patterns discovered
- Project scope shifts

**Why:** Agents read this to understand project

### 2. Use Projects/CURRENT

**Don't work across projects without switching:**
- Always `/switch [project]` before working
- Ensures correct context loaded
- Prevents mixing up client details

### 3. Track Patterns Per Project

**Project-specific patterns in `patterns.md`:**
- Client A always wants verbose logging
- Client B prefers minimal dependencies
- Internal Product: experiment freely

**Why:** Same approach doesn't fit all clients

### 4. Weekly Project Review

**Every Friday:**
1. `/switch` to each project
2. Review `tasks.md` (what's done, what's next)
3. Update client if needed
4. Mark blocked tasks for follow-up

---

## Troubleshooting

### "I switched projects but still seeing old context"

**Check:**
1. `projects/CURRENT` file - does it contain correct slug?
2. Project directory exists? `ls projects/`
3. Context.md populated? `cat projects/[slug]/context.md`

**Fix:** Run `/switch [correct-project-name]` again

### "Tasks not showing in `/overview`"

**Check:**
1. `projects/*/tasks.md` files - are tasks formatted correctly?
2. Task priority set? `priority=high/medium/low`
3. Due dates in future? `due=YYYY-MM-DD`

**Fix:** Update `tasks.md` with proper format

### "Too many projects, overwhelmed"

**Solution:**
1. Archive completed projects (move to `projects/archive/`)
2. Identify stalled projects (no activity 14+ days)
3. Either close or schedule dedicated time
4. Keep active <= 5 projects

---

**Related files:**
- `.claude/commands/overview.md` - Multi-project overview generation
- `.claude/commands/switch.md` - Project context switching
- `.claude/commands/new-project.md` - Project creation
- `.claude/agents/project-coordinator.md` - Multi-project management
- `memory/semantic/clients/` - Client profiles for all projects
