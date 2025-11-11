# Learn-Extraction: Turn Experience Into Reusable Knowledge

**What it does**: Converts your work into documented patterns so future tasks are faster
**Time**: 2-3 minutes per extraction → Saves 30-45 min per future use (10-15x ROI)

---

## How to Use It

### When You're Done with Significant Work:

**Just say**: "Use learn-extraction on what I just did"

Claude will ask you **3-4 quick questions**:
1. What did you just complete?
2. What was tricky or surprising?
3. How did you solve it?
4. Is this a pattern (seen before) or one-off?

**You answer** (be specific - 1-2 sentences each)

Claude will:
- Search for similar patterns in your knowledge base
- Either CREATE new pattern or REINFORCE existing one
- Show you what was learned and when to apply it next

**That's it.** 2-3 minutes, done.

---

## Real Example: What Just Happened

**I said**: "Use learn-extraction skill to analyze recent work"

**Claude asked** (implicitly, by analyzing git):
- What work was completed? → Skills fixes, browser automation, MCP setup
- What patterns emerged? → Searched 39 files, found 5 patterns
- How were problems solved? → Read MIGRATION_SUMMARY.md, CLAUDE.md, Skills files

**Claude extracted**:
1. **Skills Frontmatter Validation** - How to format Skills correctly (saves 30-45 min/Skill)
2. **Browser Automation Hierarchy** - When to use Playwright vs browser-use (saves 45-60 min/task)
3. **MCP Configuration Conflicts** - Why servers don't load (saves 30-45 min troubleshooting)

**Result**: Next time I face similar issues, I know exactly what to do.

---

## How Claude Gives You Context

### You Need to Provide:

**Minimum info** (Claude will ask if you don't mention):
```
1. What you completed: "Fixed OAuth bug" / "Had client discovery call" / "Published LinkedIn post"
2. Work type: code / client / content / research / other
```

**Better info** (more detail = better patterns):
```
1. What you completed: "Fixed OAuth token refresh bug causing 401 errors on mobile"
2. Root cause: "Tokens expired after 60 min, mobile apps need 15 min refresh window"
3. Solution: "Changed expiry from 3600s to 900s in auth config"
4. Pattern: "Mobile OAuth needs shorter refresh windows than web"
```

### What Claude Will Do With This:

**Step 1**: Search existing patterns
```
Claude searches: "company brain/operations/knowledge base/patterns/development-patterns.md"
Looks for: Similar auth issues, token patterns, mobile-specific fixes
```

**Step 2**: Decide NEW or REINFORCEMENT
- **If NEW**: Create entry in patterns file (Confidence: LOW, Examples: 1)
- **If seen before**: Add your example, bump confidence (LOW→MEDIUM or MEDIUM→HIGH)

**Step 3**: Document the experience
```
Creates: company brain/operations/knowledge base/experiences/2025-11-10-oauth-mobile-fix.md
Links to: Pattern in development-patterns.md
Includes: Context, solution, time saved
```

**Step 4**: Show you the impact
```
Pattern Updated: "Mobile OAuth refresh windows"
Confidence: LOW → MEDIUM (2nd example)
Time Saved: ~45 min (recognized pattern vs debugging from scratch)
Next Application: "Apply when implementing any mobile auth"
```

---

## Pattern Confidence = How Much It Saves You

| Level | Examples | What It Means | Time Saved |
|-------|----------|---------------|------------|
| **LOW** | 1 | "I've seen this once" | 0 min (first time) |
| **MEDIUM** | 2-4 | "I recognize this pattern" | 30-45 min (vs debugging) |
| **HIGH** | 5+ | "I know exactly what to do" | 60-120 min (expert mode) |

**Automatic promotion**:
- 2nd time you see it → LOW becomes MEDIUM
- 5th time you see it → MEDIUM becomes HIGH

---

## Three Ways to Use It

### 1. After Completing Work (Recommended)
```
You: "I just fixed the Skills frontmatter bug"
You: "Use learn-extraction on this"

Claude: "Great! Quick questions:
1. What was wrong with the frontmatter?
2. How did you discover the fix?
3. Pattern to remember for future Skills?"

You: [Answer 3 questions]

Claude: [Creates pattern, shows impact]
```

### 2. Daily Wrap-Up
```
You: "Run learn-extraction on today's work"

Claude: [Analyzes git commits]
Claude: "I see 3 commits today. Which one had the most learning?"

You: "The browser automation migration"

Claude: [Asks focused questions about that work]
```

### 3. When Stuck (Find Existing Patterns)
```
You: "I'm debugging an MCP server that won't load"

Claude: [Searches patterns]
Claude: "I found a HIGH confidence pattern: 'MCP Configuration Conflicts'
         Last time this happened, the fix was: Remove enabledMcpjsonServers array
         Try that?"
```

---

## What You Get

### Immediate (Per Extraction)
- **Pattern documented** in knowledge base
- **Confidence level** tracked (LOW/MEDIUM/HIGH)
- **When to apply** guidance for next time

### Week 4
- 10-15 patterns captured
- Start recognizing: "I've solved this before"
- Save ~1 hour/week

### Week 12
- 20+ patterns at MEDIUM/HIGH confidence
- Expert mode: Know exactly what to do
- Save 2-4 hours/week

---

## Setup (First Time Only)

### 1. Create Pattern Storage
```
company brain/operations/knowledge base/
├── patterns/
│   ├── development-patterns.md
│   ├── client-patterns.md
│   └── content-patterns.md
└── experiences/
    └── [dated logs created automatically]
```

### 2. Optional: Add `/learn` Command
Create `.claude/commands/learn.md`:
```markdown
# Learn Command

Run learn-extraction skill on recent work.

Ask user:
1. What did you complete?
2. What was surprising or tricky?
3. How did you solve it?
4. Pattern to remember?

Then:
- Search existing patterns
- Update or create pattern entry
- Show impact and next application
```

---

## Quick Start

**Right now, try this**:

```
"Use learn-extraction on the work from today"
```

Claude will:
1. Look at recent commits or ask what you did
2. Ask 3-4 questions
3. Extract patterns
4. Show you what was learned

**Takes 2-3 minutes. Saves hours later.**

---

**Source**: Luca's Second Brain demo (`company brain/growth/active/claude code bootcamp/product/luca-second-brain/.claude/skills/learn-extraction/`)
**Demo**: This memo was created using learn-extraction on internal-brain recent work (5 patterns extracted, 2-3 hr/week potential savings)
