# Skills vs Commands - Architecture Guide

**Last Updated:** 2025-11-10
**Version:** 1.0.0

This document clarifies the relationship between **Skills** and **Commands** in Luca's Second Brain system.

---

## TL;DR

```
┌──────────────┐
│   USER       │
└──────┬───────┘
       │ Types: /learn
       ↓
┌──────────────┐
│  COMMAND     │  ← User-facing interface (slash commands)
└──────┬───────┘
       │ Invokes
       ↓
┌──────────────┐
│   SKILL      │  ← Executable logic with examples & tests
└──────┬───────┘
       │ Uses
       ↓
┌──────────────┐
│  MEMORY      │  ← Knowledge storage (semantic/episodic/procedural)
└──────────────┘
```

**Key Principle:**
- **Commands** = User interface (what you type)
- **Skills** = Implementation (what actually executes)
- **Memory** = Knowledge storage (where data lives)

---

## What is a Command?

**Location:** `.claude/commands/*.md`

**Purpose:** User-facing entry points for workflows

**Characteristics:**
- ✅ Lightweight (50-300 lines typically)
- ✅ User-friendly names (`/learn`, `/overview`, `/switch`)
- ✅ Can be invoked manually by typing `/command-name`
- ✅ YAML frontmatter with `allowed-tools`
- ✅ May or may not use a skill

**Example:**
```yaml
---
name: learn
description: Extract learnings from recent work
allowed-tools: Read, Write, Edit, Grep, Bash
---

# Learning Extraction Command

This command helps you extract patterns from your work.

## How it works

1. Invokes the `learn-extraction` skill
2. Asks you questions about your work
3. Updates semantic and episodic memory
4. Shows brain growth impact

See: .claude/skills/learn-extraction/ for detailed implementation
```

**When to create a command:**
- User will invoke it manually (via `/command-name`)
- It's a frequently-used workflow
- You want a simple, memorable interface
- It may combine multiple skills or simple logic

---

## What is a Skill?

**Location:** `.claude/skills/<skill-name>/SKILL.md`

**Purpose:** Reusable, testable capabilities with examples and progressive disclosure

**Characteristics:**
- ✅ Comprehensive (200-500 lines for SKILL.md)
- ✅ Progressive disclosure (references/ for complex details)
- ✅ Examples included (examples/ directory)
- ✅ Tests included (tests/ directory - optional)
- ✅ YAML frontmatter with dependencies, success metrics
- ✅ **Can be invoked automatically by Claude** when pattern detected
- ✅ Reusable across commands and agents

**Example Structure:**
```
.claude/skills/learn-extraction/
├── SKILL.md                    # Main skill (219 lines)
├── references/                 # Loaded on-demand
│   ├── workflow-detailed.md    # Full 7-step execution
│   ├── confidence-logic.md     # Pattern confidence rules
│   └── pattern-types.md        # Memory organization
├── examples/                   # Real-world scenarios
│   ├── 01-code-fix.md
│   ├── 02-client-work.md
│   └── 03-content.md
└── tests/                      # Validation cases
    ├── confidence-promotion.test.md
    ├── episodic-memory.test.md
    └── brain-health.test.md
```

**When to create a skill:**
- Complex, multi-step workflow (7+ steps)
- Needs examples to demonstrate usage
- Should be reusable across contexts
- Benefits from testing/validation
- Should be automatically invoked when pattern detected

---

## Command vs Skill Decision Tree

```
START: I want to build a new workflow
│
├─ Will users invoke this manually?
│  │
│  ├─ YES → Is it simple (< 5 steps)?
│  │        │
│  │        ├─ YES → Command only (no skill needed)
│  │        │        Example: /help, /status
│  │        │
│  │        └─ NO → Command + Skill
│  │                 Example: /learn → learn-extraction skill
│  │
│  └─ NO → Should Claude detect and run automatically?
│           │
│           ├─ YES → Skill only (no command)
│           │        Example: pattern-matching skill
│           │
│           └─ NO → Maybe not needed?
│                    (Or create as utility function)
```

---

## Relationship Types

### Type 1: Command → Single Skill (Most Common)

**Example: `/learn` command → `learn-extraction` skill**

**Command file** (`.claude/commands/learn.md`):
```markdown
---
name: learn
description: Extract learnings from recent work
allowed-tools: Read, Write, Edit, Grep, Bash
---

# Learning Extraction Command

**Invokes:** `.claude/skills/learn-extraction/SKILL.md`

This command extracts patterns from your work and compounds knowledge.

## Usage

```bash
/learn
```

Claude will ask you questions, then invoke the learn-extraction skill
to update memory automatically.

See: `.claude/skills/learn-extraction/` for implementation details,
examples, and tests.
```

**Skill file** (`.claude/skills/learn-extraction/SKILL.md`):
```yaml
---
name: learn-extraction
description: Extract learnings and compound knowledge in semantic memory
version: 1.0.0
dependencies:
  tools: [Read, Write, Edit, Grep, Bash]
success_metrics:
  - pattern_identification_rate: 80%+
  - time_extraction: < 3 minutes
---

# Learning Extraction Skill

[... detailed 7-step workflow ...]
```

**Flow:**
```
User types: /learn
  → Command loads
    → Asks user questions
      → Invokes learn-extraction skill
        → Skill executes 7-step workflow
          → Updates memory
            → Returns summary to user
```

---

### Type 2: Command → Multiple Skills (Orchestration)

**Example: `/daily-workflow` command → overview + switch + learn skills**

**Command file:**
```markdown
---
name: daily-workflow
description: Complete morning routine (overview → switch → ready)
---

# Daily Workflow Command

This command orchestrates multiple skills for your morning routine.

## What it does

1. Runs `daily-overview` skill → Shows urgent tasks
2. Asks which project to start with
3. Runs `project-switching` skill → Loads context
4. Checks if you completed work yesterday
5. If yes, runs `learn-extraction` skill → Extracts patterns

Ready to work in < 2 minutes.
```

**Flow:**
```
User types: /daily-workflow
  → Invokes daily-overview skill (show tasks)
  → User selects project
  → Invokes project-switching skill (load context)
  → Checks for completed work
  → (Optional) Invokes learn-extraction skill
  → User ready to work
```

---

### Type 3: Skill Only (No Command) - Auto-Detection

**Example: `error-detection` skill (hypothetical)**

**No command file needed**

**Skill file** (`.claude/skills/error-detection/SKILL.md`):
```yaml
---
name: error-detection
description: Automatically detect and suggest fixes for common errors
auto_invoke:
  triggers:
    - pattern: "error:|Error:|ERROR:"
    - pattern: "failed|Failed|FAILED"
  confidence_threshold: 0.8
---

# Error Detection Skill

This skill automatically activates when Claude detects error messages
in your conversation.

## Auto-Invoke Logic

When Claude sees:
- Error messages in code output
- Build failures
- Test failures
- API errors

It automatically:
1. Analyzes error type
2. Searches memory for similar past errors
3. Suggests fixes from patterns
4. Offers to apply fix if confident

User never types a command - it just works.
```

**Flow:**
```
User: "Here's the error I'm getting: [error message]"
  → Claude detects "error:" pattern
    → Auto-invokes error-detection skill
      → Searches memory/semantic/patterns/dev-patterns.md
        → Finds similar error solved before
          → Suggests fix: "This looks like JWT token expiry issue..."
```

---

### Type 4: Command Only (No Skill) - Simple Operations

**Example: `/current-project` command**

**Command file** (`.claude/commands/current-project.md`):
```yaml
---
name: current-project
description: Show which project is currently active
allowed-tools: Read
---

# Current Project Command

Shows the currently active project.

## Execution

```bash
Read("projects/CURRENT")
```

Output:
```
📍 Current Project: client-a-ecommerce

Last worked: 2025-11-10 14:23
Tasks open: 7 (2 urgent, 3 high, 2 normal)

To switch: /switch [project-name]
```

**No skill needed** - this is simple enough (1 file read + format).
```

---

## Design Patterns

### Pattern A: "Thin Command, Thick Skill"

**When to use:** Complex workflows that need testing and examples

**Structure:**
```
Command (50 lines)
  ↓ invokes
Skill (200-500 lines + references + examples + tests)
```

**Examples:**
- `/learn` → `learn-extraction` skill (7-step workflow, 3 examples, 3 tests)
- `/overview` → `daily-overview` skill (12-step dashboard, time estimation)
- `/switch` → `project-switching` skill (15-step context loading)

**Benefits:**
- ✅ Command stays simple and readable
- ✅ Skill has full detail, examples, tests
- ✅ Skill can be reused by other commands/agents
- ✅ Progressive disclosure keeps token usage low

---

### Pattern B: "Fat Command, No Skill"

**When to use:** Simple workflows (< 5 steps, no reuse needed)

**Structure:**
```
Command (100-200 lines, all logic included)
No skill needed
```

**Examples:**
- `/help` - Lists all available commands
- `/current-project` - Shows active project
- `/list-ideas` - Displays captured ideas

**Benefits:**
- ✅ Faster to build (single file)
- ✅ No overhead for simple operations
- ✅ Easy to maintain (one place to edit)

**Drawbacks:**
- ❌ Can't reuse logic elsewhere
- ❌ No examples or tests
- ❌ If it grows, becomes messy

---

### Pattern C: "No Command, Skill Only"

**When to use:** Auto-detected patterns, background processes

**Structure:**
```
No command (nothing to type)
Skill with auto_invoke triggers
```

**Examples (hypothetical):**
- `error-detection` - Auto-suggests fixes when errors appear
- `duplicate-code-detector` - Flags code repetition automatically
- `documentation-checker` - Suggests missing docs

**Benefits:**
- ✅ Zero user friction (just works)
- ✅ Proactive assistance
- ✅ Learns from memory automatically

**Requirements:**
- Skill must have clear trigger conditions
- Must not be annoying (high confidence threshold)
- Should be helpful, not intrusive

---

## Current Implementation in Luca's Second Brain

### Commands with Skills

| Command | Skill | Relationship |
|---------|-------|-------------|
| `/learn` | `learn-extraction` | 1:1 (thin command, thick skill) |
| `/recall [topic]` | `memory-recall` | 1:1 (thin command, thick skill) |
| `/idea [text]` | `idea-capture` | 1:1 (thin command, thick skill) |
| `/add-task` | `task-management` | 1:1 (thin command, thick skill) |
| `/overview` | `daily-overview` | 1:1 (thin command, thick skill) |
| `/new-project` | `project-creation` | 1:1 (thin command, thick skill) |
| `/switch` | `project-switching` | 1:1 (thin command, thick skill) |
| `/grow` | `brain-health-reporting` | 1:1 (thin command, thick skill) |

### Commands without Skills (To Be Created)

| Command | Type | Why No Skill? |
|---------|------|---------------|
| `/help` | Simple | Lists commands (< 5 lines logic) |
| `/current-project` | Simple | Single file read + format |
| `/list-ideas` | Simple | Single file read + format |

---

## Guidelines for Future Development

### When Adding New Functionality

**Step 1: Decide What You're Building**

Ask:
- Is this for manual invocation or auto-detection?
- Is it simple (< 5 steps) or complex?
- Will it be reused in multiple places?
- Does it need examples/tests?

**Step 2: Choose Pattern**

```
If (manual + simple):
  → Create command only (no skill)

If (manual + complex):
  → Create command + skill (thin → thick)

If (manual + might grow):
  → Create command + skill (better to have structure)

If (auto-detect + complex):
  → Create skill only (with auto_invoke)

If (auto-detect + simple):
  → Add to existing skill or create utility function
```

**Step 3: Build**

**For Command:**
1. Create `.claude/commands/name.md`
2. Add YAML frontmatter with `allowed-tools`
3. Write user-facing description
4. Point to skill (if using one): "See: `.claude/skills/skill-name/`"

**For Skill:**
1. Create `.claude/skills/skill-name/SKILL.md`
2. Add comprehensive YAML frontmatter (dependencies, metrics)
3. Write ~200-line overview with workflow steps
4. Add references/ for complex logic (optional)
5. Add 1-3 examples in examples/
6. Add 1-3 tests in tests/ (optional but recommended)

---

## Migration Strategy (Existing Code)

### Current State

We have both:
- `.claude/commands/learn.md` (367 lines)
- `.claude/commands/learn-IMPROVED.md` (263 lines)
- `.claude/skills/learn-extraction/SKILL.md` (219 lines)

**Problem:** Redundancy and confusion

### Recommended Cleanup

**Option A: Thin Command + Thick Skill (Recommended)**

1. **Keep:** `.claude/skills/learn-extraction/SKILL.md` (full implementation)
2. **Update:** `.claude/commands/learn.md` → Make it thin (50 lines):
   ```markdown
   ---
   name: learn
   ---

   # Learning Extraction Command

   Invokes: `.claude/skills/learn-extraction/SKILL.md`

   [Brief description]
   [Usage example]
   [Link to skill for details]
   ```
3. **Delete:** `.claude/commands/learn-IMPROVED.md` (redundant)

**Benefit:** Clear separation, skill has full detail

---

**Option B: Consolidate to Skill Only**

1. **Keep:** `.claude/skills/learn-extraction/SKILL.md`
2. **Delete:** Both command files
3. **Users type:** `Read .claude/skills/learn-extraction/SKILL.md` to invoke

**Benefit:** One source of truth
**Drawback:** Less user-friendly (no `/learn` shortcut)

---

**Recommendation:** Use **Option A** for all 8 core workflows

---

## Best Practices

### For Commands

✅ **DO:**
- Keep simple (< 100 lines if possible)
- Use clear, memorable names (`/learn`, `/switch`, `/overview`)
- Point to skills for implementation details
- Include usage examples
- Explain what it does (not how)

❌ **DON'T:**
- Duplicate skill logic in command
- Write detailed workflows in commands (put in skills)
- Create command for every tiny operation

---

### For Skills

✅ **DO:**
- Write comprehensive SKILL.md (~200 lines)
- Use progressive disclosure (references/ for details)
- Include 1-3 examples showing real usage
- Add tests for critical logic
- Use clear YAML frontmatter
- Define success metrics
- Explain the "how" in detail

❌ **DON'T:**
- Put 500+ lines in SKILL.md (use references/)
- Skip examples (they're crucial for understanding)
- Forget to update skill when logic changes

---

## FAQ

**Q: Can a skill be invoked without a command?**
A: Yes! Either by Claude auto-detecting a pattern, or by agents invoking skills directly, or by users typing `Read .claude/skills/skill-name/SKILL.md`.

**Q: Can a command exist without a skill?**
A: Yes! For simple operations (< 5 steps, no reuse), a command-only approach is fine.

**Q: Should I create a command for every skill?**
A: Not necessarily. Only create commands for workflows users will manually invoke. Skills can exist independently for auto-detection or agent use.

**Q: What's the difference between a skill and an agent?**
A: **Skills** are capabilities (what can be done). **Agents** are orchestrators (who coordinates skills). Agents use multiple skills to accomplish complex goals.

**Q: When should I use references/ in a skill?**
A: When SKILL.md would exceed ~200-300 lines. Move detailed workflows, complex logic, or reference material to references/ and load on-demand.

---

## Summary

**Commands:**
- User interface
- Lightweight
- Manually invoked
- Points to skills (usually)

**Skills:**
- Implementation
- Comprehensive
- Auto or manual invoke
- Reusable and testable

**Together:**
- Commands provide friendly UX
- Skills provide robust execution
- Memory provides knowledge storage

**Result:**
- Clean architecture
- Testable components
- Reusable across contexts
- Easy to maintain and extend

---

*Last Updated: 2025-11-10*
*Version: 1.0.0*
*For Luca's Second Brain System*
