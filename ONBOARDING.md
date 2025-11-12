# 2-Hour Onboarding Session - Dominic's Second Brain for C# Development

**Session Goals:**
1. Understand your C# project and import existing documentation
2. Set up context preservation workflow (solve your #1 pain point!)
3. Learn the **Vibe Engineering** workflow: PLAN → DELEGATE → ASSESS → CODIFY
4. Create your first C# patterns from existing work
5. Build a practical workflow for tomorrow morning using essential commands

**Time:** 2 hours (10:00-12:00 GMT+2)
**Date:** 12/11/2025

---

## What is Vibe Engineering?

**Core Philosophy:** Each unit of engineering work should make subsequent units easier—not harder.

```
   PLAN          DELEGATE        ASSESS         CODIFY
    │                │              │              │
    │  /plan        │  /work       │  /review     │  /learn
    │  Break down   │  Execute     │  Quality     │  Extract
    │  into steps   │  tasks       │  checks      │  patterns
    │                │              │              │
    └──────────────► └────────────► └────────────► └──────┐
                                                            │
    ┌───────────────────────────────────────────────────────┘
    │
    └──► (Knowledge compounds - next cycle is faster)
```

**Key Principle:** Your Second Brain learns from every task, making future development faster.

---

## Pre-Session Checklist (5 minutes before we start)

- [ ] Have your C# project open in VS Code/VS Community
- [ ] Have your existing documentation accessible
- [ ] Have this repository open in another window
- [ ] Have a recent C# code file ready to discuss
- [ ] Think of 1-2 current challenges you're facing

---

## Session Structure Overview

| Time | Duration | Activity | Vibe Phase | Goal |
|------|----------|----------|------------|------|
| 10:00-10:20 | 20 min | **Discovery & Import** | Setup | Understand your C# project, import docs |
| 10:20-10:45 | 25 min | **Context Preservation** | Setup | Solve conversation limit frustration |
| 10:45-11:10 | 25 min | **Vibe Workflow Demo** | All 4 Phases | Learn PLAN→DELEGATE→ASSESS→CODIFY |
| 11:10-11:40 | 30 min | **Live Coding with Vibe** | Practice | Generate real C# code with workflow |
| 11:40-12:00 | 20 min | **Daily Routine & Tools** | Integration | Essential commands for tomorrow |

---

## Part 1: Discovery & Import (20 minutes)

### Objective: Understand your C# project and populate the Second Brain

**What we'll do:**

1. **Quick Project Overview (5 min)**
   - You share: What's your C# application? What does it do?
   - You share: Current tech stack (.NET version, database, frameworks)
   - You share: Current phase and biggest challenges
   - I'll capture this in structured format

2. **Documentation Import (10 min)**
   - You share: Location of your existing documentation
   - I'll help organize it into the Second Brain structure:
     - Architecture docs → `memory/semantic/tech/architecture-decisions.md`
     - Patterns you've discovered → `projects/dominic-csharp-app/patterns.md`
     - Project context → `projects/dominic-csharp-app/context.md`
   - We'll identify 3-5 key patterns you already use

3. **Tech Stack Documentation (5 min)**
   - Fill in your actual .NET version, database, ORM
   - Document key NuGet packages you use
   - Note any architectural decisions you've made

**Deliverable:** Your project context fully documented and ready to reference

---

## Part 2: Context Preservation Setup (25 minutes)

### Objective: Solve your #1 pain point - never lose context between conversations again

**The Problem You Described:**
> "When Claude reaches conversation limits and requires a restart from one conversation to the next - very frustrating"

**The Solution:** @memory/ Mentions

**What we'll do:**

1. **Understand @memory/ Mentions (5 min)**
   - I'll explain how the `@memory/` folder structure works
   - Show you how to use @-mentions to load context files
   - Walk through the memory types: semantic, episodic, procedural
   - Demonstrate how Claude Code automatically reads these files

2. **Set Up Your Memory Structure (15 min)**
   - We'll organize your existing documentation into `memory/`
   - Create your first memory entries:
     - `@memory/semantic/tech/csharp-patterns.md` (your coding patterns)
     - `@memory/semantic/tech/architecture-decisions.md` (project structure)
     - `@memory/episodic/completed-work/` (work you've done)
   - Practice using @-mentions to load context in conversation
   - I'll show you how this prevents context loss

   **Example scenario:**
   - Current task: "Adding email validation to user registration"
   - Document in: `@memory/episodic/completed-work/2025-11-12-email-validation.md`
   - Next conversation: Just @-mention the file to restore full context

3. **Build the Habit (5 min)**
   - When to document: After completing significant work
   - What to capture: Decisions, patterns, code examples
   - How @-mentions work: Claude Code reads files automatically
   - How to tell when you need memory: When you're repeating explanations

**Deliverable:**
- Memory structure set up with initial entries
- Confidence in using @-mentions to load context
- Never lose context again!

**Expected Time Saved:** 45 minutes per context restoration (vs re-explaining everything)

---

## Part 3: Vibe Workflow Demo (25 minutes)

### Objective: Learn the complete PLAN → DELEGATE → ASSESS → CODIFY cycle

**What we'll do:**

We'll walk through a real C# task using all four phases of vibe engineering:

### 1. PLAN Phase (8 min) - `/plan` command

Pick a small real task from your project (example: "Add email validation to user registration"):

```bash
/plan "Add email validation to UserRegistration class with proper error messages"
```

**What happens:**
- Creates detailed GitHub issue or plan file
- Researches your existing C# patterns
- Breaks down into ≤10 minute steps
- Generates code examples matching your style

**Agent used:** `best-practices-researcher` (finds C# email validation patterns)

### 2. DELEGATE Phase (8 min) - `/work` command

Execute the plan systematically:

```bash
/work path/to/plan.md
```

**What happens:**
- Creates isolated git worktree
- Breaks plan into trackable todos
- Executes each step with continuous validation
- Runs tests after changes

**Agent used:** `pattern-recognition-specialist` (applies your existing patterns)

### 3. ASSESS Phase (5 min) - `/review` command

Quality check before merging:

```bash
/review
```

**What happens:**
- Runs specialized review agents:
  - `security-sentinel` - Security vulnerabilities
  - `code-simplicity-reviewer` - Complexity check
  - `python-reviewer` - C# best practices (or C# equivalent)
- Creates trackable todos for findings
- Ensures code meets quality bar

### 4. CODIFY Phase (4 min) - `/learn` command

Extract patterns for future reuse:

```bash
/learn
```

**What happens:**
- Asks: What was completed? Root cause? Patterns identified?
- Documents in `memory/semantic/patterns/dev-patterns.md`
- Updates `projects/dominic-csharp-app/patterns.md`
- Confidence level: LOW → MEDIUM → HIGH over time

**Example Pattern Extracted:**
```markdown
### Pattern: Email Validation with Regex

**Confidence:** LOW (first use)

**When to use:** All user input fields requiring email

**Implementation:**
[Your actual validation code]

**Benefits:**
- Consistent validation
- Proper error messages
- Reusable across forms
```

**Deliverable:**
- Complete understanding of 4-phase workflow
- Real example from your C# project
- Pattern documented for future reuse

**Expected Time Saved:**
- First use: Planning overhead (~5 min investment)
- Second use: 30% faster (pattern exists)
- Fifth use: 60% faster (automatic application)

---

## Part 4: Live Coding with Vibe (30 minutes)

### Objective: Practice the vibe workflow on your real C# code

**What we'll do:**

You'll drive this session! Pick a real feature you need to implement.

### Your Turn to Drive (25 min)

**Pick a real task:**
- "Add authentication middleware to API"
- "Create repository pattern for Product entity"
- "Add unit tests for UserService"
- "Implement logging across services"

**Then execute all 4 phases:**

```bash
# 1. PLAN (5 min)
/plan "Your chosen task description"
# Review the generated plan, verify it matches your context

# 2. DELEGATE (10 min)
/work path/to/generated-plan.md
# Watch as it executes systematically with your patterns

# 3. ASSESS (5 min)
/review
# See quality checks run automatically

# 4. CODIFY (5 min)
/learn
# Extract the pattern for future reuse
```

**What you'll experience:**
- Code generated in **your style** (using documented patterns)
- Follows **your architecture** (from context.md)
- Includes **your standards** (XML docs, error handling, naming)
- Creates **git checkpoints** (can rollback any step)
- Extracts **reusable pattern** (speeds up future work)

### Workflow Integration (5 min)

**Three ways to work with generated code:**

1. **Copy/Paste** (Current)
   - Generate in browser → Copy → Paste to IDE
   - Quick for small changes

2. **Direct File Edit** (Recommended)
   - Claude Code reads/writes files directly
   - `/work` creates actual files in your project
   - Less friction, fewer errors

3. **Review & Refine** (Best)
   - Generate code with `/work`
   - Review with `/review`
   - Refine based on findings
   - Zero copy/paste

**Deliverable:**
- Real C# code generated and working
- Confidence using all 4 vibe phases
- Pattern extracted for reuse
- Reduced copy/paste friction

**Expected Time Saved:**
- Planning: 10 min saved (research done for you)
- Execution: 20 min saved (patterns applied automatically)
- Quality: 15 min saved (automatic reviews)
- **Total: 45 min per feature using vibe workflow**

---

## Part 5: Daily Routine & Essential Tools (20 minutes)

### Objective: Build your sustainable vibe workflow starting tomorrow

**What we'll do:**

### 1. Tomorrow Morning Routine (8 min)

**9:00 AM - Start with Context:**

```bash
# Load your project context
/switch dominic-csharp-app

# What happens automatically:
# - Reads context.md (tech stack, architecture)
# - Loads tasks.md (current priorities)
# - Shows patterns.md (your documented patterns)
# - Displays recent activity
```

**Then start your work:**

```bash
# For complex features (>15 min)
/plan "Add JWT authentication to API endpoints"

# For simple tasks (<15 min)
# Just describe what you need directly
"Add validation to the Email property in User class"
```

### 2. Essential Commands You'll Use Daily (8 min)

**Core Vibe Workflow:**
```bash
/plan [task]          # PLAN: Break down complex work
/work [plan-file]     # DELEGATE: Execute systematically
/review               # ASSESS: Quality checks
/learn                # CODIFY: Extract patterns
```

**Daily Operations:**
```bash
/switch [project]     # Load project context instantly
/overview             # Morning dashboard of urgent tasks
```

**NOT Essential for Day 1:**
- `/idea` - Quick idea capture (useful later)
- `/recall` - Search memory (when you have more patterns)
- `/grow` - Brain health metrics (weekly ritual)
- `/add-task` - Add tasks (use tasks.md directly for now)

**Focus on mastering:** `/plan`, `/work`, `/review`, `/learn`, `/switch`

### 3. Agents Working Behind the Scenes (4 min)

You don't call these directly - they're invoked automatically:

**During `/plan`:**
- `best-practices-researcher` - Finds C# patterns and docs
- `framework-docs-researcher` - Pulls .NET documentation

**During `/work`:**
- `pattern-recognition-specialist` - Applies your patterns

**During `/review`:**
- `security-sentinel` - Security checks
- `code-simplicity-reviewer` - Complexity analysis
- `python-reviewer` - Code quality (adaptable to C#)

**You just run the commands - agents work automatically!**

**Deliverable:**
- Clear morning routine: `/switch` → `/plan` or direct work
- 5 essential commands mastered
- Understanding which agents do what (automatically)

---

## Session Deliverables Checklist

By the end of 2 hours, you'll have:

- [ ] **Project fully documented** in `projects/dominic-csharp-app/context.md`
- [ ] **Vibe workflow mastered** - PLAN → DELEGATE → ASSESS → CODIFY
- [ ] **5 essential commands practiced** - `/plan`, `/work`, `/review`, `/learn`, `/switch`
- [ ] **First C# pattern extracted** via `/learn` command
- [ ] **Real C# code generated** using vibe workflow
- [ ] **Tomorrow morning routine** ready to execute
- [ ] **Confidence** using the system independently

---

## Key Concepts to Internalize

### 1. Vibe Engineering = Compounding Work

**Traditional Development:**
- Each feature adds complexity
- Codebase harder to work with over time
- Same problems solved repeatedly

**Vibe Engineering:**
- Each feature documents patterns (`/learn`)
- Next feature faster (`/plan` knows your patterns)
- Quality improves (`/review` automatic)
- Knowledge compounds exponentially

### 2. Four-Phase Workflow = Consistency

```
PLAN → DELEGATE → ASSESS → CODIFY
  ↓         ↓         ↓        ↓
/plan    /work    /review   /learn
```

**Every feature follows same flow:**
1. Break down the work
2. Execute systematically
3. Verify quality
4. Extract learnings

### 3. Context Preservation = Zero Friction

**Before:** 45 min re-explaining context each conversation
**After:** `/switch dominic-csharp-app` (instant context load)
**Savings:** 45 min per project switch

---

## After the Session - Your First Week

### Day 1 (Today - After Session)
- [ ] Run `/learn` on tonight's work (build the habit)
- [ ] Test `/switch dominic-csharp-app` in a new conversation
- [ ] Review the pattern that was extracted

### Day 2 - Practice PLAN Phase
- [ ] Morning: `/switch dominic-csharp-app`
- [ ] Pick a small task: `/plan "your task"`
- [ ] Review the generated plan
- [ ] End: `/learn` to extract any insights

### Day 3 - Practice DELEGATE Phase
- [ ] Morning: `/switch dominic-csharp-app`
- [ ] Use yesterday's plan: `/work path/to/plan.md`
- [ ] Watch systematic execution
- [ ] End: `/learn` on what was completed

### Day 4 - Practice ASSESS Phase
- [ ] Morning: `/switch dominic-csharp-app`
- [ ] Complete a small feature
- [ ] Run `/review` to see quality checks
- [ ] End: `/learn` to document

### Day 5 - Complete Cycle
- [ ] Full vibe workflow: `/plan` → `/work` → `/review` → `/learn`
- [ ] Track time saved compared to manual approach
- [ ] Note which patterns are being reused

### Friday (End of Week)
- [ ] Review `projects/dominic-csharp-app/patterns.md`
- [ ] Run `/overview` to see all projects
- [ ] Calculate actual time saved this week
- [ ] Promote any patterns used 2+ times: LOW → MEDIUM

---

## Questions to Ask During Session

Don't hesitate to ask:

1. **About vibe engineering:**
   - "What makes vibe engineering different from normal development?"
   - "Why does each phase matter (PLAN/DELEGATE/ASSESS/CODIFY)?"
   - "How does knowledge actually compound?"

2. **About the commands:**
   - "When should I use `/plan` vs just describing what I need?"
   - "What happens during `/work` execution?"
   - "How does `/review` know what to check?"
   - "Why is `/learn` so important?"

3. **About patterns:**
   - "How do I decide what counts as a 'pattern'?"
   - "When should I promote from LOW to MEDIUM confidence?"
   - "Can patterns from one project help another?"

4. **About the workflow:**
   - "How does `/switch` instantly load project context?"
   - "Can I use this workflow for non-C# projects?"
   - "What if I forget to run `/learn`?"
   - "How do agents know which patterns to apply?"

---

## Success Metrics - Track These

After the session and first week:

**Vibe Workflow Adoption:**
- `/plan` → `/work` → `/review` → `/learn` cycles completed: ____
- Patterns extracted via `/learn`: ____ (goal: 5+ in first week)
- Pattern confidence promotions: ____ (LOW → MEDIUM)

**Time Savings per Feature:**
- Planning time: ____ min (vs ____ min manual)
- Execution time: ____ min (vs ____ min manual)
- Review time: ____ min (vs ____ min manual)
- Total saved: ____ min per feature

**Quality Improvements:**
- Security issues caught by `/review`: ____
- Code simplifications suggested: ____
- Patterns reused automatically: ____

**Knowledge Compounding:**
- Week 1 patterns: ____
- Week 2 patterns: ____ (cumulative)
- Week 3 patterns: ____ (cumulative)
- Features built using existing patterns: ____

---

## Troubleshooting - Common Issues

### "I forgot to run `/learn` after completing work"

**Solution:**
- Pattern lost this time, but lesson learned
- Build habit: `/learn` immediately after `/review`
- Set reminder: Every feature = PLAN → DELEGATE → ASSESS → **CODIFY**
- Even quick `/learn` (2 min) better than nothing

### "When should I use `/plan` vs direct execution?"

**Solution:**
- **Use `/plan` for:**
  - Features >15 minutes
  - Multiple file changes
  - When you're not sure where to start
- **Direct execution for:**
  - Simple changes <5 minutes
  - Single file edits
  - Clear, straightforward tasks

### "The `/review` found issues - what now?"

**Solution:**
- This is working as designed! Quality gates catching problems
- Read findings carefully (security, complexity, best practices)
- Fix critical issues first (security vulnerabilities)
- Document pattern to avoid issue next time
- Re-run `/review` after fixes

### "I don't see patterns being reused yet"

**Solution:**
- First week: Extract patterns (5-10 patterns)
- Second week: Start seeing reuse
- Third week: 30-50% pattern reuse
- Month 2+: 60%+ features use existing patterns
- Trust the compound effect!

---

## Resources During Session

We'll reference these files:

1. **CLAUDE.md** - How the system works and @-mention approach
2. **memory/README.md** - Memory structure and organization
3. **projects/dominic-csharp-app/context.md** - Your project details
4. **projects/dominic-csharp-app/patterns.md** - Your C# patterns
5. **memory/semantic/tech/architecture-decisions.md** - Your tech choices

---

## Post-Session Action Plan

**Immediate (Today):**
1. Document current work in memory/episodic/
2. Test @-mention loading in new conversation
3. Add any patterns we missed during session

**This Week:**
1. Use morning routine daily with @-mentions
2. Document completed work in memory/ daily
3. Add 3-5 more patterns

**This Month:**
1. Review and promote patterns weekly
2. Measure time savings
3. Refine workflow based on experience
4. Achieve consistent 20 hr/week savings

---

## Final Thoughts

**Your Goal:** Build side business faster to achieve financial freedom

**Vibe Engineering Delivers:**
- Each feature makes the next one easier (compounding work)
- Quality improves automatically (`/review` catches issues)
- Knowledge preserved forever (`/learn` extracts patterns)
- Zero context loss (`/switch` instant loading)
- Systematic execution (`/plan` → `/work` → `/review` → `/learn`)

**The 2-Hour Investment:**

Will pay back in **Week 1** through:
- 5-10 patterns extracted (foundation for future)
- First features using vibe workflow
- Context preservation working

Will multiply in **Month 1** through:
- 30-50% pattern reuse
- 45 min saved per feature
- Quality improvements visible

Will compound in **Month 3+** through:
- 60%+ features using existing patterns
- 10+ hours/week saved consistently
- Knowledge base growing exponentially

**Remember:**
Traditional development gets harder over time.
Vibe engineering gets easier with every feature.

**Let's build your Second Brain and accelerate your path to financial freedom!**

---

## Session Checklist - For Instructor (Me)

During the session, I will:

- [ ] Ask about C# project early, understand the domain
- [ ] Help organize existing docs into Second Brain structure
- [ ] Create 1 complete context snapshot together
- [ ] Extract 3-5 real patterns from Dominic's code
- [ ] Generate real C# code demonstrating context awareness
- [ ] Document morning routine for tomorrow
- [ ] Ensure Dominic feels confident using the system
- [ ] Answer all questions thoroughly
- [ ] Keep to 2-hour timeline
- [ ] End with clear next steps

---

**Ready to start? Let's make your C# development dramatically more efficient!**
