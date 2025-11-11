# Development Workflow Use Case

**Purpose:** Streamline software development work with code review automation, pattern learning, and technical knowledge compounding.

**Who it's for:** Developers, technical founders, CTOs, engineers

**Time savings:** 5-8 hours/week

---

## What This Use Case Provides

### 1. Automated Code Review
- **Command:** `/code-review [file-path]`
- **Agent:** code-reviewer.md
- **What it does:**
  - Security scan (SQL injection, XSS, secrets in code)
  - Quality check (DRY violations, complexity, error handling)
  - Performance check (N+1 queries, missing indexes)
  - Pattern compliance (checks against your learned patterns)
- **Time saved:** 30 min per review → 5 min

### 2. Development Pattern Extraction
- **Command:** `/learn` (after completing development task)
- **What it does:**
  - Extracts reusable patterns from work
  - Tracks confidence (low → medium → high)
  - Links to experiences
- **Example patterns:**
  - JWT token expiry for mobile apps (15-20 min)
  - PostgreSQL indexing on foreign keys
  - Error logging with user context
- **Time saved:** Next similar task is 2-5x faster

### 3. Technical Knowledge Base
- **Files:**
  - `memory/semantic/tech/architecture-decisions.md` - Tech stack, decisions
  - `memory/semantic/patterns/dev-patterns.md` - Reusable code patterns
  - `memory/episodic/code/` - Specific development experiences
- **Value:** Never forget how you solved a problem

---

## How to Use

### Daily Development Workflow

**Morning:**
1. Run `/overview` to see all coding tasks across projects
2. Pick highest priority task
3. Run `/switch [project]` to load project context

**During development:**
1. Code normally
2. Before committing: `/code-review [file]` (automated review)
3. Fix any issues flagged
4. Commit and push

**After completing task:**
1. Run `/learn` to extract patterns (2 min)
2. Answer questions about what you learned
3. Patterns automatically added to semantic memory
4. Experience logged to episodic memory

**Example:**
```bash
# Morning
/overview
# Output: Client A - OAuth bug fix (HIGH priority, due today)

/switch client-a
# Loads Client A context, recent work, relevant patterns

# Work on code...

# Before committing
/code-review backend/auth/oauth.py
# Output: 2 security issues, 1 quality issue, suggests fixes

# Fix issues, commit

# After completing
/learn
# Q: What was the issue?
# A: JWT refresh tokens expiring too fast for mobile clients

# Q: How did you fix it?
# A: Changed expiry from 5 min to 15 min for mobile

# Q: Have you seen this before?
# A: Yes, similar issue in another project

# Pattern added to dev-patterns.md with MEDIUM confidence
```

---

## Installed Components

### Commands
- `/code-review [file]` - Review code for issues
- `/learn` - Extract patterns after completing work

### Agents
- **code-reviewer** - Reviews code against security, quality, performance, patterns
  - Location: `.claude/agents/code-reviewer.md`
  - Tools: Read, Grep, code analysis

### Memory Structure
- **Semantic memory:**
  - `memory/semantic/tech/architecture-decisions.md` - Your tech stack, architectural choices
  - `memory/semantic/patterns/dev-patterns.md` - Reusable development patterns

- **Episodic memory:**
  - `memory/episodic/code/YYYY-MM-DD-description.md` - Specific development tasks
  - Format: What you did, why, how, patterns applied, learnings

- **Procedural memory:**
  - `memory/procedural/workflows/code-review-workflow.md` - Step-by-step code review process
  - `memory/procedural/workflows/deployment-workflow.md` - Deployment checklist

### Patterns Tracked

**High-confidence patterns (apply automatically):**
- Error logging with user context
- PostgreSQL indexing on foreign keys
- N+1 query prevention (select_related, prefetch_related)

**Medium-confidence patterns (apply with consideration):**
- JWT token expiry for mobile apps
- API error response structure
- Database transaction safety

**Low-confidence patterns (validate before applying):**
- New patterns with 1 example
- Early observations

---

## Integration Points

### GitLab Integration (Optional, Week 4)

**What it does:**
- Auto-run `/learn` after merge requests
- Extract patterns from commit messages
- Link commits to episodic memory

**Setup:**
```bash
# .gitlab-ci.yml
learn-from-merge:
  script:
    - claude-code run /learn --auto
  only:
    - merge_requests
```

### Local Git Hooks (Optional, Week 2)

**What it does:**
- Run code review before commit
- Prevent commits with critical issues

**Setup:**
```bash
# .git/hooks/pre-commit
#!/bin/bash
claude-code run /code-review $(git diff --cached --name-only)
if [ $? -ne 0 ]; then
  echo "Code review failed. Fix issues before committing."
  exit 1
fi
```

---

## Customization

### Add Project-Specific Patterns

Each project can have custom patterns:

**File:** `projects/[project-name]/patterns.md`

```markdown
## Client A - E-commerce Patterns

### Pattern: Product Image Optimization
- Always resize to max 800px width
- Convert to WebP format
- Lazy load below the fold
- Use CDN for delivery

**Why:** Client A has 50K+ products, image performance is critical
```

**Code reviewer will check these project-specific patterns too.**

---

### Add Custom Tech Stack Checks

Update `memory/semantic/tech/architecture-decisions.md`:

```markdown
## Client B - Must Use Patterns

### PostgreSQL
- Always use connection pooling (pgBouncer)
- Index all foreign keys
- Use EXPLAIN ANALYZE for queries > 100ms

### Django
- Use select_related() for foreign keys
- Use prefetch_related() for many-to-many
- Async views for I/O-bound operations
```

---

## Success Metrics

**Track in `brain-health/metrics.md`:**

### Code Quality Metrics
- **Critical issues per review:** Target: 0 (should trend down over time)
- **Pattern compliance rate:** Target: 90%+ (HIGH confidence patterns followed)
- **Time per code review:** Target: < 10 min (down from 30 min)

### Learning Metrics
- **Patterns extracted:** Target: 1-2 per week
- **Pattern confidence growth:** LOW → MEDIUM → HIGH
- **Time saved per pattern application:** Average: 15-30 min

### Weekly Tracking
```markdown
## Week of Jan 6-12

**Code reviews:** 8
- Critical issues: 2 (down from 5 last week)
- Average review time: 8 min (down from 12 min)
- Patterns followed: 14/15 (93%)

**Patterns extracted:** 2
- JWT token expiry (upgraded to MEDIUM confidence)
- Database transaction safety (new LOW confidence pattern)

**Time saved:** ~6 hours this week
- Code review automation: 3 hours (8 reviews × 22 min saved each)
- Pattern application: 3 hours (applied 6 patterns, ~30 min saved each)
```

---

## Common Workflows

### Workflow 1: Bug Fix

1. Identify bug (from client report or monitoring)
2. `/switch [project]` - Load project context
3. Reproduce bug locally
4. Debug (use patterns from memory)
5. Implement fix
6. `/code-review [file]` - Review fix
7. Test fix
8. Commit and deploy
9. `/learn` - Extract pattern if new bug type

**Time:** Before (2 hours), After (45 min with patterns)

---

### Workflow 2: New Feature Development

1. `/switch [project]` - Load project context
2. Read requirements
3. Design (check existing patterns for guidance)
4. Implement (apply learned patterns)
5. `/code-review [files]` - Review code
6. Fix issues
7. Test feature
8. Deploy to staging
9. `/learn` - Extract new patterns
10. Deploy to production

**Time:** Patterns save 20-30% on implementation time

---

### Workflow 3: Code Refactoring

1. Identify code smell (DRY violation, complexity)
2. `/switch [project]` - Load context
3. Plan refactor (check patterns for best approach)
4. Refactor code
5. `/code-review [files]` - Ensure quality improved
6. Run tests (no regression)
7. Commit
8. `/learn` - Extract refactoring pattern

**Time:** Patterns prevent common refactoring mistakes (save 1-2 hours)

---

## Next Steps

### Week 1: Setup
- [ ] Fill `memory/semantic/tech/architecture-decisions.md` with your tech stack
- [ ] Run `/code-review` on 2-3 files (see how it works)
- [ ] Complete 1 task and run `/learn` (extract first pattern)

### Week 2: Build Habit
- [ ] Use `/code-review` before every commit
- [ ] Run `/learn` after completing 3-5 tasks
- [ ] Track time saved in `brain-health/metrics.md`

### Week 3: Optimize
- [ ] Review patterns (which have MEDIUM/HIGH confidence?)
- [ ] Apply HIGH confidence patterns automatically
- [ ] Identify gaps (what patterns are missing?)

### Week 4: Integrate
- [ ] Set up GitLab integration (auto-learn from commits)
- [ ] Add git hooks (auto code review)
- [ ] Create team patterns (if working with others)

---

## Troubleshooting

### "Code review flagged false positive"

**Solution:** Update patterns to exclude this case
```markdown
## Pattern: Error Logging

**Exception:** Don't log in hot loops (performance impact)
**Example:** Skip logging in: data processing loops, real-time handlers
```

### "Pattern not applying to new project"

**Check:**
1. Is pattern in `memory/semantic/patterns/dev-patterns.md`?
2. Is confidence level HIGH/MEDIUM?
3. Is project context loaded? (run `/switch [project]`)
4. Does project use same tech stack? (check `memory/semantic/tech/`)

### "Code review takes too long"

**Solutions:**
- Review specific files (not entire codebase): `/code-review src/auth/login.py`
- Focus on critical paths (auth, payments, data processing)
- Batch reviews (review 5 files at once, not one-by-one)

---

**Related use cases:**
- Client PM Workflow (project management for client work)
- Idea Capture Workflow (capture technical ideas while coding)

**Related files:**
- `.claude/agents/code-reviewer.md` - Code review agent definition
- `memory/semantic/patterns/dev-patterns.md` - Your development patterns
- `memory/semantic/tech/architecture-decisions.md` - Your tech stack
