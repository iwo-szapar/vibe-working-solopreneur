# 2-Hour Onboarding Session - Dominic's Second Brain for C# Development

**Session Goals:**
1. Understand your C# project and import existing documentation
2. Set up context preservation workflow (solve your #1 pain point!)
3. Create your first C# patterns from existing work
4. Build a practical workflow for tomorrow morning
5. Demonstrate Claude Code efficiency gains

**Time:** 2 hours (10:00-12:00 GMT+2)
**Date:** 12/11/2025

---

## Pre-Session Checklist (5 minutes before we start)

- [ ] Have your C# project open in VS Code/VS Community
- [ ] Have your existing documentation accessible
- [ ] Have this repository open in another window
- [ ] Have a recent C# code file ready to discuss
- [ ] Think of 1-2 current challenges you're facing

---

## Session Structure Overview

| Time | Duration | Activity | Goal |
|------|----------|----------|------|
| 10:00-10:20 | 20 min | **Discovery & Import** | Understand your C# project, import docs |
| 10:20-10:45 | 25 min | **Context Preservation Setup** | Solve conversation limit frustration |
| 10:45-11:10 | 25 min | **Pattern Extraction** | Document 3-5 C# patterns you use |
| 11:10-11:40 | 30 min | **Live Coding Demo** | Generate real C# code with context |
| 11:40-12:00 | 20 min | **Daily Workflow & Next Steps** | Create your tomorrow morning routine |

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

**The Solution:** Context Snapshots

**What we'll do:**

1. **Understand Context Snapshots (5 min)**
   - I'll explain how `context-snapshots/` works
   - Walk through the template: `context-snapshots/TEMPLATE.md`
   - Show you the example: `context-snapshots/examples/2025-11-12-quick-example.md`

2. **Create Your First Context Snapshot (15 min)**
   - We'll pick a real task you're working on (or recently completed)
   - Together, we'll create a complete context snapshot
   - You'll see how much detail to include
   - We'll practice the "restore" process

   **Example scenario:**
   - Current task: "Adding email validation to user registration"
   - Create snapshot with: code in progress, decisions made, next steps
   - I'll show you how to restore this context in a new conversation

3. **Build the Habit (5 min)**
   - When to save: Every 50-75 messages, before breaks, before limits
   - Quick save vs detailed save (when to use each)
   - Naming convention: `YYYY-MM-DD-HHMM-topic.md`
   - How to tell when you're approaching limit (conversation feels slower)

**Deliverable:**
- One complete context snapshot created
- Confidence in creating snapshots independently
- Never lose context again!

**Expected Time Saved:** 45 minutes per context restoration (vs re-explaining everything)

---

## Part 3: Pattern Extraction (25 minutes)

### Objective: Document 3-5 C# patterns you already use, enabling faster development

**What we'll do:**

1. **Review Your Code (10 min)**
   - You share a recent C# file you're proud of
   - We'll identify patterns you're using:
     - Async/await usage
     - Dependency injection
     - Error handling approach
     - Repository pattern (if using)
     - Validation strategy
     - Naming conventions

2. **Document Patterns (10 min)**
   - I'll help you write these into `projects/dominic-csharp-app/patterns.md`
   - For each pattern, we'll capture:
     - **What** it does
     - **When** to use it
     - **Code example** from your actual project
     - **Confidence level** (start as LOW, promote over time)

   **Example Pattern We Might Extract:**
   ```markdown
   ### Pattern: Service Layer with Dependency Injection

   **Confidence:** MEDIUM (you've used it 3+ times)

   **When to use:** All business logic classes

   **Implementation:**
   [Your actual code example]

   **Benefits:**
   - Testable
   - Follows SOLID principles
   - Consistent across project
   ```

3. **Pattern Application Strategy (5 min)**
   - How patterns speed you up: 2nd use = 30% faster, 5th use = 60% faster
   - Promotion path: LOW → MEDIUM → HIGH confidence
   - When to apply automatically vs consider carefully

**Deliverable:**
- 3-5 documented patterns from your actual code
- Framework for adding more as you discover them
- Understanding of how patterns compound knowledge

**Expected Time Saved:** 15-30 minutes per pattern application in future work

---

## Part 4: Live Coding Demo (30 minutes)

### Objective: See Claude Code in action generating real C# code with context

**What we'll do:**

1. **Pick a Real Task (5 min)**
   - You choose something you need to build (or recently built)
   - Examples:
     - "Add input validation to a specific model"
     - "Create a new repository for Product entity"
     - "Add authentication middleware"
     - "Generate unit tests for UserService"

2. **Context-Aware Code Generation (20 min)**
   - Using the project context we documented earlier
   - Using the patterns we extracted
   - I'll generate C# code that:
     - Follows your naming conventions
     - Uses your established patterns
     - Matches your architecture style
     - Includes XML documentation comments
     - Handles errors your way

   **Demo Flow:**
   - You describe what you need
   - I ask clarifying questions using your documented context
   - I generate code in your style
   - We refine together
   - You copy to IDE (or I can help optimize this workflow)

3. **Less Copy/Paste Workflow (5 min)**
   - Discuss how to minimize browser→IDE friction
   - Claude Code can read/write files directly
   - Show you file editing capabilities
   - Establish preferred workflow for tomorrow

**Deliverable:**
- Real, usable C# code generated
- Understanding of context-aware generation
- Improved workflow (less copy/paste)

**Expected Time Saved:** 10 hours/week on code generation (your current metric)

---

## Part 5: Daily Workflow & Next Steps (20 minutes)

### Objective: Create your sustainable routine starting tomorrow morning

**What we'll do:**

1. **Tomorrow Morning Routine (8 min)**

   **9:00 AM - Start New Conversation:**
   ```
   You: "I'm working on [your C# project]. Let me restore context from my last session."

   You: [Share relevant files]:
   - projects/dominic-csharp-app/context.md
   - projects/dominic-csharp-app/patterns.md
   - context-snapshots/[latest-snapshot].md

   Claude: [Reads files, understands full context]

   You: "I need to [today's task]"

   Claude: [Generates code using your patterns, style, and context]
   ```

2. **During the Day Workflow (5 min)**

   **For New Features:**
   - Describe what you need
   - I apply your documented patterns
   - Review and refine
   - Copy to IDE (or direct file operations)

   **For Questions:**
   - Reference your architecture decisions
   - Leverage documented patterns
   - Quick answers based on your context

   **For Data Analysis:**
   - Generate SQL queries for your database
   - Analysis and insights
   - Documentation of findings

3. **Before Conversation Limit (3 min)**

   **Save Context:**
   - Create snapshot in `context-snapshots/`
   - 3-5 minutes investment
   - Saves 45 minutes later
   - Template makes it easy

   **What to Include:**
   - Current task and progress
   - Code in progress (actual snippets)
   - Decisions made
   - Next steps

4. **Weekly Pattern Review (4 min)**

   **Every Friday (10 minutes):**
   - Review `projects/dominic-csharp-app/patterns.md`
   - Promote patterns: LOW → MEDIUM → HIGH
   - Add new patterns discovered this week
   - Update architecture decisions
   - Quick wins add up to 20 hr/week savings!

**Deliverable:**
- Clear morning routine documented
- Confidence in daily workflow
- Sustainable system you'll actually use

---

## Session Deliverables Checklist

By the end of 2 hours, you'll have:

- [ ] **Project fully documented** in `projects/dominic-csharp-app/context.md`
- [ ] **Tech stack captured** in `memory/semantic/tech/architecture-decisions.md`
- [ ] **3-5 C# patterns documented** in `projects/dominic-csharp-app/patterns.md`
- [ ] **First context snapshot created** in `context-snapshots/`
- [ ] **Real C# code generated** using your patterns and style
- [ ] **Tomorrow morning routine** documented and ready
- [ ] **Confidence** in the system and workflow

---

## Key Concepts to Internalize

### 1. Context Preservation = Time Savings

**Before:** 30-60 min re-explaining context each conversation
**After:** 2 min to restore context from snapshot
**Savings:** 45 min per conversation restart

### 2. Patterns = Velocity

**Before:** Reinventing solutions each time
**After:** Apply proven patterns automatically
**Savings:** 15-30 min per pattern application

### 3. Documentation = Compounding Knowledge

**Before:** Solving same problems repeatedly
**After:** Solutions documented and reusable
**Savings:** Exponential over time

---

## After the Session - Your First Week

### Day 1 (Today - After Session)
- [ ] Try creating a context snapshot for tonight's work
- [ ] Test restoring context in a new conversation
- [ ] Document 1-2 more patterns you notice

### Day 2
- [ ] Use morning routine to start with full context
- [ ] Generate 1-2 pieces of C# code with pattern application
- [ ] Save context before ending work

### Day 3
- [ ] Reference your architecture decisions during development
- [ ] Add a new pattern you discover
- [ ] Practice quick context snapshots (under 3 min)

### Day 4-5
- [ ] Build habit: start with context, end with snapshot
- [ ] Measure time saved (track 1-2 examples)
- [ ] Refine your patterns (promote confidence levels)

### Friday (End of Week)
- [ ] Pattern review session (10 min)
- [ ] Calculate time saved this week
- [ ] Plan Week 2 improvements

---

## Questions to Ask During Session

Don't hesitate to ask:

1. **About your project:**
   - "Can you show me how you currently structure your C# projects?"
   - "What patterns are you already using successfully?"
   - "What's your biggest code generation challenge?"

2. **About context preservation:**
   - "How do I know when I'm close to conversation limit?"
   - "How much detail should I include in snapshots?"
   - "Can you show me how to restore context?"

3. **About patterns:**
   - "How do I decide what counts as a 'pattern'?"
   - "When should I promote from LOW to MEDIUM confidence?"
   - "How do patterns speed up my development?"

4. **About the workflow:**
   - "What's the fastest way to get code from Claude to my IDE?"
   - "How can I minimize context loss?"
   - "What should I document vs what can I skip?"

---

## Success Metrics - Track These

After the session and first week:

**Time Savings:**
- Context restoration time: ____ minutes (goal: < 2 min)
- Pattern application time: ____ minutes saved per use
- Code generation time: ____ hours/week saved

**Quality Improvements:**
- Code consistency: Better / Same / Worse
- Fewer bugs: Yes / No / Same
- Faster development: Yes / No / Same

**System Usage:**
- Context snapshots created: ____ (goal: 1 per day)
- Patterns documented: ____ (goal: 10+ by end of month)
- Conversations with full context: ____% (goal: 100%)

---

## Troubleshooting - Common Issues

### "I forgot to save context and hit the limit"

**Solution:**
- In new conversation, try to remember key points
- Create a snapshot from memory (better than nothing)
- Build habit: save every 50 messages as safety

### "My context snapshot is too long/detailed"

**Solution:**
- Start with quick template (1 min)
- Add details only for complex tasks
- Focus on: current task, next steps, key decisions

### "I don't know when to document a pattern"

**Solution:**
- If you've done something 2+ times = document it
- Even tentative patterns help (mark as LOW confidence)
- Review weekly and remove what doesn't help

### "The workflow feels slow at first"

**Solution:**
- First week is learning curve
- Week 2 you'll see speed increase
- Week 3+ is where 20 hr/week savings kicks in
- Trust the process!

---

## Resources During Session

We'll reference these files:

1. **context-snapshots/README.md** - How context preservation works
2. **context-snapshots/TEMPLATE.md** - Template for creating snapshots
3. **projects/dominic-csharp-app/context.md** - Your project details
4. **projects/dominic-csharp-app/patterns.md** - Your C# patterns
5. **memory/semantic/tech/architecture-decisions.md** - Your tech choices

---

## Post-Session Action Plan

**Immediate (Today):**
1. Create one more context snapshot for current work
2. Test restore process in new conversation
3. Add any patterns we missed during session

**This Week:**
1. Use morning routine daily
2. Create context snapshots daily
3. Add 3-5 more patterns

**This Month:**
1. Review and promote patterns weekly
2. Measure time savings
3. Refine workflow based on experience
4. Achieve consistent 20 hr/week savings

---

## Final Thoughts

**Your Goal:** Build side business faster to achieve financial freedom

**This System Enables:**
- 20 hours/week time savings (you're already there!)
- Zero context loss (new capability!)
- Compounding knowledge (patterns grow over time)
- Consistent quality (documented standards)
- Reduced frustration (no more conversation limit pain!)

**The 2-Hour Investment:**
Will pay back in the first week through:
- Faster code generation
- Context preservation
- Pattern reuse
- Reduced copy/paste friction

**Let's build your Second Brain and accelerate your path to financial freedom! 🚀**

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
