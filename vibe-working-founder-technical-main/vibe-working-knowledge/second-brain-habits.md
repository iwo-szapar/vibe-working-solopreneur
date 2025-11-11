# Second Brain Habits: Daily, Weekly, Monthly Rituals
**For**: Luca's Second Brain (Technical Founder Edition)
**Purpose**: Compound knowledge systematically through consistent rituals
**Time Investment**: 15-30 min/day → 8.5 hours/week saved
**ROI**: 10-15x return on time invested

---

## Philosophy: Small Habits, Compounding Returns

Your second brain gets smarter **ONLY** through consistent use. These rituals ensure:
- ✅ **Knowledge captured** before it's forgotten
- ✅ **Patterns extracted** from every significant experience
- ✅ **Context maintained** across projects
- ✅ **Growth tracked** for ROI visibility

**Critical insight**: Missing daily /learn means losing 30-45 min of time savings per future task.

---

## Daily Rituals

### Morning Ritual (5-10 minutes)

**When**: Start of workday, before opening any project
**Goal**: Get oriented, prioritize, decide where to start
**Time**: 5-10 minutes

#### 1. Check Overall Status

**Command**: `/overview`
**What it does**:
- Shows urgent tasks across ALL projects (due today/overdue)
- Identifies blocked tasks (waiting on others)
- Suggests quick wins (< 30 min tasks)
- Estimates total time needed today
- Recommends focus order

**Decision flow**:
```
/overview
  → See 3 urgent tasks (Client A due today, Internal feature blocked, Content draft ready)
  → Decide: Start with Client A (urgent + unblocked)
  → Run: /switch client-a
```

#### 2. Switch to Priority Project

**Command**: `/switch [project-name]`
**What it does**:
- Loads project context (zero context-switching overhead)
- Shows recent activity (last 3 tasks)
- Surfaces open tasks and blockers
- Displays relevant patterns from semantic memory
- Suggests where to start

**Example**:
```
/switch client-a-ecommerce

Output:
- Last worked on: 2 days ago (Stripe webhook integration)
- Open tasks: 3 (1 urgent, 2 this week)
- Blockers: None
- Relevant pattern: "JWT refresh tokens need 15-min expiry" (confidence: HIGH)
- Suggestion: Continue with webhook idempotency (Step 3 of 5)
```

#### 3. Plan Complex Work (If Needed)

**Command**: `/plan [goal]` (for tasks > 15 min)
**What it does**:
- Breaks task into 5-7 steps (≤10 min each)
- Creates `.claude-plan.json` with dependencies
- Enables incremental execution with auto-checkpoints
- Prevents context drift and rate limit waste

**When to use**:
- New feature implementation
- Complex bug investigation
- Multi-file refactoring
- Content creation (blog → LinkedIn → video chain)

**When NOT to use**:
- Quick fixes (< 15 min)
- Single-file edits
- Simple queries

**Example**:
```
/plan Add Stripe webhook idempotency

Output:
Created 5-step plan:
1. Read current webhook handler (5 min)
2. Create webhook_events table (8 min)
3. Add idempotency check (10 min)
4. Write test for duplicates (8 min)
5. Update documentation (5 min)

Total estimated: 36 minutes
Run /step to start
```

---

### During Work (Continuous)

#### 4. Execute Incrementally

**Command**: `/step` (if using /plan) OR work directly (for simple tasks)
**What it does**:
- Executes ONE step from plan
- Creates git checkpoint after each step
- Shows progress and next action
- Allows resume from any point

**Pattern**:
```
/step
  → Step 1/5 complete: Read webhook handler (5 min)
  → Checkpoint saved: git commit
  → Next: Step 2 (Create webhook_events table)

/step
  → Step 2/5 complete: Table created (8 min)
  → Checkpoint saved
  → Next: Step 3 (Add idempotency check)
```

**Benefits**:
- ✅ No lost work if rate limit hit
- ✅ Clear stopping points for breaks
- ✅ Resume exactly where left off

---

### End of Day Ritual (5-10 minutes)

**When**: End of workday, before closing laptop
**Goal**: Extract learnings, update context, track growth
**Time**: 5-10 minutes

#### 5. Extract Learnings

**Command**: `/learn`
**What it does**:
- Prompts 3-4 quick questions (2 min max)
- Searches semantic memory for similar patterns
- Updates patterns (NEW or REINFORCEMENT)
- Creates episodic memory record
- Updates brain health metrics
- Shows ROI impact

**Questions asked** (varies by work type):
```
Code work:
- "What was the root cause?"
- "How did you solve it?"
- "What's the pattern to remember?"

Client work:
- "What was unique or surprising?"
- "What worked well?"
- "What would you do differently?"

Content creation:
- "What resonated with audience?"
- "What was your process?"
- "What made this effective?"
```

**Example session**:
```
/learn

Claude: "What did you just complete?"
You: "Fixed Stripe webhook idempotency"

Claude: "What was the root cause?"
You: "Duplicate webhook events caused double-charges"

Claude: "How did you solve it?"
You: "Created webhook_events table with event_id as primary key, check before processing"

Claude: "Have you seen similar issues before?"
You: "Yes, OAuth duplicate token refreshes had same pattern"

Claude: "What's the pattern to remember?"
You: "Event-driven systems need idempotency keys stored in DB"

Output:
✅ Pattern REINFORCED: "Event idempotency pattern"
   - Confidence: MEDIUM → HIGH (3 examples now)
   - Time saved: 30 min (vs. re-learning each time)
   - Examples: OAuth tokens, webhooks, job queues
   - Next application: Implement for payment retry logic

📊 Brain Growth:
   - Total patterns: 47 (↑1 this week)
   - High confidence: 18 (↑1 today)
   - Time saved this month: 12.5 hours
```

**Critical**: Don't skip this. 2 minutes now = 30-45 min saved next time.

#### 6. Update Project Status (Optional)

**Command**: `/add-task [description]` OR manual edit of `tasks.md`
**What it does**:
- Adds new tasks to current project
- Updates task status
- Tracks completion for `/overview`

**When to use**:
- Discovered new task during work
- Need to capture TODOs before signing off

---

### Quick Captures (As Needed)

#### 7. Capture Ideas

**Command**: `/idea [text]`
**What it does**:
- Quick idea capture (voice or text)
- Auto-categorizes (product, content, business, tech)
- Stores in `memory/semantic/ideas/`
- Assigns priority
- Links to relevant projects

**Use cases**:
- Walking dog: "Voice note - Add real-time notifications to dashboard"
- In meeting: "Client wants AI-powered search in their app"
- Reading article: "Blog idea: How we reduced API latency by 60%"

**Example**:
```
/idea Add real-time notifications to dashboard

Output:
✅ Idea captured:
   - Category: product-feature
   - Priority: HIGH (aligns with 3 client requests)
   - Related projects: client-a-ecommerce, internal-saas
   - Estimated effort: 2-3 days
   - Next step: Create feature spec in next planning session
```

#### 8. Recall Patterns

**Command**: `/recall [topic]`
**What it does**:
- Searches ALL memory (semantic, episodic, procedural)
- Shows relevant patterns with confidence levels
- Displays past examples
- Suggests application

**Use cases**:
- "How did I solve X before?"
- "What was that pattern about Y?"
- "Which client had Z requirement?"

**Example**:
```
/recall webhook idempotency

Output:
Found 3 patterns:

1. **Event idempotency pattern** (HIGH confidence)
   - Description: Use DB table with event_id as primary key
   - Examples: Stripe webhooks, OAuth tokens, job queues
   - Time savings: 30 min per application
   - Last used: Today (client-a-ecommerce)

2. **Idempotency key generation** (MEDIUM confidence)
   - Description: UUID v4 + timestamp for unique keys
   - Examples: Payment retries, API requests
   - Time savings: 15 min per application
   - Last used: 3 weeks ago (internal-saas)

3. **Testing idempotency** (LOW confidence)
   - Description: Send duplicate requests, verify single processing
   - Examples: Webhook tests
   - Time savings: 20 min per application
   - Last used: 2 months ago
```

---

## Weekly Rituals

### End of Week Review (15-20 minutes)

**When**: Friday afternoon or Monday morning
**Goal**: Assess progress, track ROI, plan next week
**Time**: 15-20 minutes

#### 9. Brain Health Check

**Command**: `/grow`
**What it does**:
- Shows growth metrics (patterns learned, confidence levels)
- Calculates time saved (actual ROI)
- Identifies knowledge gaps
- Recommends focus areas

**Example output**:
```
/grow

🧠 Brain Health Report (Week 45, 2025)

**Knowledge Growth:**
- Patterns identified: 6 this week (47 total)
- High confidence patterns: 18 (38% of total)
- New domains: Added "content-distribution" pattern
- Confidence promotions: 2 (MEDIUM → HIGH)

**Time Savings (Measured):**
- This week: 8.2 hours saved
- This month: 34 hours saved
- Since start: 128 hours saved (ROI: 12x)

**Pattern Application:**
- "Event idempotency": Applied 3 times (saved 90 min)
- "Discovery process template": Applied 2 times (saved 120 min)
- "Blog → LinkedIn chain": Applied 1 time (saved 180 min)

**Top Performers (Most ROI):**
1. Content chain pattern: 180 min/week
2. Discovery process: 120 min/week
3. Event idempotency: 90 min/week

**Recommendations:**
✅ Keep: Content chain pattern (high ROI, frequent use)
⚠️  Practice: Database indexing pattern (low confidence, needs more examples)
🔄 Review: OAuth pattern (medium confidence, ready to promote)

**Next Week Goals:**
- Aim for 7-8 patterns learned
- Promote 2 patterns to HIGH confidence
- Target: 10 hours saved
```

#### 10. Project Portfolio Review

**Manual review** of `projects/` directory:
- Which projects are active vs. stale?
- Which projects need context updates?
- Any projects ready to archive?

**Actions**:
```bash
# Archive completed projects
mkdir projects/archive/
mv projects/old-project projects/archive/

# Update active project contexts
/switch client-a
# Review context.md, update if needed

/switch internal-saas
# Review context.md, update if needed
```

#### 11. Cleanup and Maintenance

**Tasks**:
- Clear old episodic memories (archive older than 6 months)
- Review LOW confidence patterns (delete if not reinforced in 3 months)
- Update `brain-health/growth-log.md` with weekly summary
- Git commit all changes with descriptive message

```bash
# Archive old episodic memories
mkdir memory/episodic/archive/2025-q1/
mv memory/episodic/code/2025-01-* memory/episodic/archive/2025-q1/

# Git commit
git add .
git commit -m "Week 45 cleanup: Archive old memories, update growth metrics"
```

---

## Monthly Rituals

### End of Month Review (30-45 minutes)

**When**: Last Friday of month
**Goal**: Strategic review, ROI calculation, system optimization
**Time**: 30-45 minutes

#### 12. Comprehensive ROI Analysis

**Command**: `/grow` + manual analysis

**Metrics to track**:
```markdown
## Monthly ROI Report (November 2025)

**Time Investment:**
- Daily /learn: 10 min × 20 workdays = 200 min (3.3 hours)
- Weekly /grow: 15 min × 4 weeks = 60 min (1 hour)
- Setup/maintenance: 60 min (1 hour)
- **Total: 5.3 hours invested**

**Time Saved:**
- Pattern applications: 34 hours
- Context switching reduction: 8 hours
- Idea capture efficiency: 4 hours
- Content chain automation: 12 hours
- **Total: 58 hours saved**

**ROI: 11x** (58 hours saved / 5.3 hours invested)

**Top 5 Patterns by ROI:**
1. Content chain: 12 hours/month
2. Discovery process: 8 hours/month
3. Event idempotency: 6 hours/month
4. Database indexing: 4 hours/month
5. OAuth refresh: 4 hours/month
```

#### 13. Pattern Portfolio Optimization

**Questions to answer**:
- Which patterns have HIGH confidence but low usage? (candidates for archiving)
- Which patterns have MEDIUM confidence with frequent use? (candidates for promotion)
- Which patterns need more examples? (focus for next month)

**Actions**:
```markdown
## Pattern Optimization (November 2025)

**Promote to HIGH:**
- "Database indexing pattern" (4 examples now, consistent success)
- "Content distribution checklist" (3 examples, high impact)

**Archive (unused for 3 months):**
- "Legacy API migration" (LOW confidence, not relevant anymore)
- "WordPress theme customization" (no longer doing WordPress work)

**Focus for December:**
- Reinforce "AI prompt engineering patterns" (only 1 example, high potential)
- Create "Video production workflow" pattern (new domain)
```

#### 14. System Health Check

**Check**:
- CLAUDE.md still accurate? (update if business context changed)
- Skills still relevant? (add new, archive unused)
- Commands working? (test each command)
- Hooks functioning? (check `.claude/hooks/` scripts)
- Git repo healthy? (run cleanup, check size)

**Actions**:
```bash
# Test all commands
/overview
/switch client-a
/learn
/recall webhooks
/grow

# Check git repo size
du -sh .git/
# If > 500 MB, consider cleaning up

# Update CLAUDE.md if needed
# (business mission, new projects, new tools)
```

#### 15. Strategic Planning

**Questions**:
- What new domains to learn? (new skills, new patterns)
- What workflows to automate? (repetitive tasks > 30 min/week)
- What integrations to add? (new tools, new MCPs)
- What patterns to share? (blog posts, case studies)

**Output**: Update `brain-health/roadmap.md` with:
```markdown
## Q1 2026 Roadmap

**New Skills to Build:**
1. Video editing workflow (target: 3 hours/week saved)
2. AI agent orchestration patterns (emerging domain)
3. Database performance optimization (advanced patterns)

**New Integrations:**
1. Figma MCP (design workflow automation)
2. Linear MCP (task management sync)
3. Gamma API (visual insights generation)

**Content to Create:**
1. Blog: "How I saved 200+ hours with a Second Brain"
2. Video: "Knowledge Compounding for Technical Founders"
3. Case study: "Real-world ROI from pattern extraction"

**Experiments:**
1. Subagent parallelization for content chains (speed test)
2. Voice-first idea capture (mobile app + transcription)
3. Automated weekly reports (insight generation from patterns)
```

---

## Quarterly Rituals

### End of Quarter Deep Dive (2-3 hours)

**When**: Last week of quarter (March, June, September, December)
**Goal**: Major system overhaul, strategic direction
**Time**: 2-3 hours (block time)

#### 16. Comprehensive System Audit

**Full review**:
- All patterns (semantic memory)
- All projects (active vs. archive)
- All skills (usage stats)
- All commands (effectiveness)
- All integrations (ROI)

**Questions**:
```
1. Which patterns delivered most value? (keep, expand)
2. Which patterns never used? (archive, delete)
3. Which projects most active? (prioritize)
4. Which projects stalled? (archive, reprioritize)
5. Which skills used most? (refine, optimize)
6. Which skills never used? (delete)
7. Which integrations working? (expand)
8. Which integrations broken? (fix, remove)
```

#### 17. Knowledge Compounding Metrics

**Calculate**:
```markdown
## Q4 2025 Knowledge Compounding Report

**Pattern Growth:**
- Start: 35 patterns (22 LOW, 10 MEDIUM, 3 HIGH)
- End: 58 patterns (15 LOW, 25 MEDIUM, 18 HIGH)
- Growth: +23 patterns (66% increase)
- Confidence progression: 51% now HIGH/MEDIUM (vs. 37% start)

**Time Savings:**
- Q4 total: 234 hours saved
- Average per week: 18 hours
- Average per day: 3.6 hours
- ROI: 14x (17 hours invested, 234 hours saved)

**Top Domains:**
1. Content creation: 96 hours saved
2. Client work: 72 hours saved
3. Code development: 48 hours saved
4. Learning/research: 18 hours saved

**Breakthrough Moments:**
1. Content chain automation (saved 8 hours/week)
2. Discovery process template (saved 6 hours/week)
3. Event idempotency pattern (saved 4 hours/week)

**Compounding Evidence:**
- Average time per pattern application: 30 min (vs. 3 hours before)
- Pattern reuse rate: 4.2 applications per pattern
- New domain learning time: -40% (faster due to transfer learning)
```

#### 18. Strategic Direction Setting

**Update long-term vision**:
```markdown
## 2026 Vision for Second Brain

**Mission:**
Transform from "productivity tool" to "strategic intelligence system"

**Goals:**
1. 500+ hours saved per year (current: 234 hours in Q4)
2. 100+ high-confidence patterns (current: 18)
3. Cross-domain pattern transfer (apply code patterns to content, etc.)
4. Automated insight generation (weekly reports from patterns)

**New Capabilities:**
1. Multi-agent orchestration (parallel content creation)
2. Predictive pattern suggestion (AI recommends which pattern to use)
3. Knowledge graph visualization (see pattern relationships)
4. Automated retrospectives (extract learnings from git commits)

**Experiments:**
1. Voice-first interface (Claude mobile app integration)
2. Real-time pattern application (suggestions as you work)
3. Team collaboration (shared semantic memory)
4. Public knowledge base (blog posts from patterns)
```

---

## Habit Stack Summary

### Daily (15-20 min total)
```
Morning:
├── /overview (2 min)
├── /switch [project] (1 min)
└── /plan [goal] if needed (2 min)

During work:
├── /step (continuous if using plan)
└── /idea [text] (as needed)

End of day:
├── /learn (5 min) ← CRITICAL, never skip
└── /add-task (2 min, optional)
```

### Weekly (15-20 min)
```
End of week:
├── /grow (5 min)
├── Project portfolio review (5 min)
└── Cleanup and maintenance (10 min)
```

### Monthly (30-45 min)
```
End of month:
├── ROI analysis (15 min)
├── Pattern optimization (15 min)
└── Strategic planning (15 min)
```

### Quarterly (2-3 hours)
```
End of quarter:
├── System audit (60 min)
├── Knowledge compounding metrics (30 min)
└── Strategic direction (30 min)
```

---

## Success Metrics (6 months)

**Target outcomes**:
- ✅ **Time saved**: 200+ hours (vs. 5 hours invested)
- ✅ **ROI**: 10-15x return on time invested
- ✅ **Patterns**: 50+ total (30% high confidence)
- ✅ **Projects**: 5-10 active (zero context-switching overhead)
- ✅ **Quality**: Every project easier than the last

**Leading indicators** (track weekly):
- Daily /learn completion rate: > 80%
- Weekly /grow reviews: 100%
- Pattern application rate: > 3 per week
- New pattern identification: > 1 per week

---

## Common Pitfalls to Avoid

**1. Skipping /learn** ❌
- Feels like "extra work" but is the HIGHEST ROI activity
- 2 min invested = 30-45 min saved per future application
- Missing /learn = losing 10-15x ROI

**2. Not using /plan for complex work** ❌
- Trying to "save time" by working directly
- Results in context drift, rate limit waste, lost work
- 5 min planning = 30 min saved in execution

**3. Letting projects go stale** ❌
- Not updating context.md for weeks
- Causes context-switching overhead to return
- Weekly review prevents this

**4. Hoarding patterns** ❌
- Keeping LOW confidence patterns that never get reinforced
- Clutters semantic memory
- Archive unused patterns quarterly

**5. Not tracking ROI** ❌
- Can't prove value of the system
- Makes it easy to skip habits
- Weekly /grow keeps motivation high

---

## Quick Reference Card

**Copy-paste this to your desk:**

```
┌─────────────────────────────────────────────┐
│ 🧠 SECOND BRAIN QUICK REFERENCE             │
├─────────────────────────────────────────────┤
│ MORNING (5 min):                            │
│   /overview → See what's urgent             │
│   /switch [project] → Load context          │
│   /plan [goal] → Break down complex work    │
├─────────────────────────────────────────────┤
│ DURING WORK:                                │
│   /step → Execute one step at a time        │
│   /idea [text] → Capture quick insights     │
│   /recall [topic] → Find past patterns      │
├─────────────────────────────────────────────┤
│ END OF DAY (5 min):                         │
│   /learn ← NEVER SKIP THIS                  │
├─────────────────────────────────────────────┤
│ END OF WEEK (15 min):                       │
│   /grow → Check brain health & ROI          │
├─────────────────────────────────────────────┤
│ ROI REMINDER:                               │
│   2 min /learn = 30 min saved next time     │
│   5 min /plan = 30 min saved in execution   │
│   Target: 10-15x return on time invested    │
└─────────────────────────────────────────────┘
```

---

**Remember**: Your second brain compounds. Every pattern extracted makes the next project easier. Every habit skipped loses compounding potential. Start small, stay consistent, track ROI.

---

**Questions? Check**:
- README.md for system overview
- CLAUDE.md for technical details
- ONBOARDING.md for setup guide
- SKILLS_VS_COMMANDS.md for architecture
