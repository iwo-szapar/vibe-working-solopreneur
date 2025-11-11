# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

**Luca's Second Brain** - Knowledge management system using three-layer architecture: Commands → Skills → Memory.

**Core Purpose:** Persistent second brain that extracts patterns from work and applies them automatically across projects.

## User Profile: Luca

**Role & Context:**
- 41-year-old founder/CEO of 2-person IT services company
- Technical background: comfortable with automation, APIs, CLIs, MCPs
- Works hybrid (remote + on-site)
- Uses AI multiple times daily (currently saves 4-6 hr/week)

**Work Domains (Multi-Hat):**
- Software development & R&D
- Client project management (multiple simultaneous clients)
- Content creation (blog, LinkedIn, video)
- Product management & feature planning
- Sales & business development
- Learning & exploring new technologies

**Communication Style Preferences:**
- Professional, objective, technically accurate
- Challenges AI suggestions, pushes back, probes deeply
- Values final decision authority (AI augments, doesn't decide)
- Prefers evidence-based reasoning over validation
- Appreciates when AI disagrees if technically correct

**Key Pain Points:**
- Context overload across multiple projects/scopes
- Manual documentation burden (especially during calls)
- Idea loss due to capture friction
- Time spent on task tracking and prioritization (1.5hr/day)
- Feeling overwhelmed, difficulty bringing everything to completion

**Current AI Maturity:**
- 40% augmentation, 0% automation (growth opportunity)
- Primary tools: Cursor (multiple models), Claude app, Gemini
- Uses AI for: learning, technical decisions, code, content drafting
- Impact rating: 4/5 (significant, hard to work without)
- Psychological impact: +4 (faster, more confident) / -2 (sometimes detached)

**Content & Documentation Standards:**
- Never publish client-specific info without approval
- Never use swear words or attack people directly
- Code must balance DB query vs Python/backend processing
- Always explain logic via code comments
- Prefer component patterns for consistency

**Decision-Making Pattern:**
- Explores pros/cons via AI dialogue
- Never delegates final technical decisions
- Often pushes back on AI suggestions
- Probes assumptions and edge cases
- Decisions impact self primarily, team secondarily (scale: 2/5)

**6-Month Success Vision:**
- Personal AI assistant for all work domains
- Voice-based idea capture (including while driving) - 2hr/week saved
- Automated task tracking across projects - 1hr/week saved
- Morning dashboard with urgent items - 30min/day saved
- Real-time documentation updates
- Consistent content production (blog → LinkedIn → video)
- Partial customer support automation
- Feel less overwhelmed, more in control

**Integration Ecosystem:**
- Primary: Fibery (PM), GitLab (code), Google Drive (docs)
- Secondary: Pipedrive CRM, LinkedIn Sales Navigator
- Voice: macOS dictation (Fn Fn) or mobile app
- Prefers: CLI/terminal interfaces over GUI when possible

**Learning & Risk Management:**
- Experimental mindset, incremental implementation
- Spots issues early before they cascade
- Requires version control for all text-based support
- Needs confirmation for critical actions
- Prefers fallback to external tools (Fibery, GitLab) with AI as translator

**Next Workflow Priorities (Post-Foundation):**
1. Feature planning
2. Test writing
3. Content planning
4. Internal project management
5. Product documentation/wiki
6. Sales/onboarding automation

## Architecture

### Three-Layer System

```
Commands (.claude/commands/*.md)
    ↓ invoke
Skills (.claude/skills/*/SKILL.md)
    ↓ use
Memory (memory/)
```

1. **Commands**: User-facing slash commands (`/learn`, `/switch`, `/overview`)
2. **Skills**: Executable workflows with progressive disclosure, examples, and tests
3. **Memory**: Knowledge storage (semantic, episodic, procedural)

**Key Principle:** Commands are the interface, Skills are the implementation, Memory is the storage.

See `SKILLS_VS_COMMANDS.md` for detailed architecture explanation.

### Directory Structure

```
.claude/
├── commands/          # 11 slash commands
├── skills/            # 10 executable skills with references/examples/tests
├── hooks/             # user-prompt-submit.sh (context-aware micro-guidance)
└── agents/            # Specialized AI agents

memory/
├── semantic/          # What you know (facts, patterns, tech stack)
├── episodic/         # What you've done (experiences, completed work)
└── procedural/       # How you do things (workflows, processes)

projects/             # Active projects (client work, internal products)
├── INDEX.md
└── [project-name]/
    ├── context.md
    ├── tasks.md
    └── patterns.md

use-cases/            # Workflow patterns for different personas
├── dev-workflow/
├── client-pm-workflow/
├── idea-capture-workflow/
└── content-chain-workflow/
```

## Core Workflows

### Daily Commands (Most Frequently Used)

```bash
/overview              # Morning dashboard: urgent tasks across all projects
/switch [project]      # Zero-overhead context switching with auto-load
/idea [text]          # Quick idea capture with auto-categorization
/add-task [desc]      # Add task to current project with auto-prioritization
/learn                # Extract patterns from completed work
/grow                 # Brain health metrics and ROI tracking
```

### Planning System (For Complex Tasks >15 min)

```bash
/plan [goal]          # Break into ≤10 min steps, save to .claude-plan.json
/step                 # Execute next step incrementally with auto-checkpoint
/plan-status          # Show progress, time tracking, blockers
```

**When to use planning:**
- Tasks that take >15 minutes
- Multi-file changes with dependencies
- When approaching rate limits
- Complex implementations requiring checkpoints

**When NOT to use planning:**
- Simple edits (<5 minutes)
- Single-file changes
- Straightforward bug fixes

#### Plan Structure (.claude-plan.json)

Plans are saved as JSON with this structure:

```json
{
  "id": "plan-20251110-143522",
  "goal": "Add Stripe webhook idempotency to client-a-ecommerce",
  "created": "2025-11-10T14:35:22Z",
  "project": "client-a-ecommerce",
  "totalSteps": 5,
  "currentStep": 1,
  "estimatedTime": "35 minutes",
  "steps": [
    {
      "number": 1,
      "action": "Read current webhook handler code",
      "files": ["api/src/webhooks/stripe.ts"],
      "estimate": "5 min",
      "status": "pending",
      "dependencies": [],
      "verification": "Understand current flow"
    },
    {
      "number": 2,
      "action": "Create webhook_events table migration",
      "files": ["migrations/"],
      "estimate": "8 min",
      "status": "pending",
      "dependencies": [1],
      "verification": "Migration runs successfully"
    }
  ],
  "progress": {
    "completed": 0,
    "remaining": 5,
    "timeSpent": "0 min",
    "timeRemaining": "35 min"
  }
}
```

#### Step Execution Flow

Each `/step` execution:

1. **Reads plan** from `.claude-plan.json`
2. **Verifies dependencies** (checks previous steps completed)
3. **Executes current step** (reads files, makes changes)
4. **Creates git checkpoint** (`git add . && git commit -m "Step N/M: [action]"`)
5. **Updates plan status** (marks step complete, advances currentStep)
6. **Shows progress** (X/Y steps, time tracking)
7. **Previews next step** (what's coming next)

**Rate Limit Management:**
- Each step saves progress in `.claude-plan.json`
- If rate limited mid-plan, you can resume from last completed step
- Git checkpoints allow reverting to any previous state
- Incremental progress prevents wasted work

### Memory Commands

```bash
/recall [topic]       # Search all memory types for relevant patterns
/new-project [name]   # Scaffold new project with standard structure
```

## How to Work in This Repository

### 1. Check Current Context

Before making any changes:

```bash
# Read current project (created by /switch command)
Read projects/CURRENT

# Load project context
Read projects/[name]/context.md
Read projects/[name]/tasks.md
Read projects/[name]/patterns.md
```

### 2. For Complex Tasks: Plan First

If task requires >15 minutes or multiple files:

```bash
# Create plan
/plan [your goal description]

# Review the 5-step breakdown
# Each step should be ≤10 minutes

# Execute incrementally
/step
/step
/step

# Check progress anytime
/plan-status
```

### 3. For Simple Tasks: Execute Directly

For quick edits, single-file changes, or <5 minute tasks:
- Just do the work directly
- No need for planning overhead

### 4. After Completing Work: Extract Learnings

```bash
# Always run after finishing significant work
/learn

# Answer questions about:
# - What was completed
# - Root cause of issues
# - Patterns identified
# - Confidence level
```

### 5. Update Project State

```bash
# Mark tasks complete in tasks.md
Edit projects/[name]/tasks.md

# Document new patterns discovered
Edit projects/[name]/patterns.md
```

## Skills Framework

### Progressive Disclosure Pattern

Skills use a modular structure to manage context efficiently:

```
skill-name/
├── SKILL.md                  # Core instructions (~200 lines)
├── references/               # Loaded on-demand
│   ├── workflow-detailed.md
│   ├── troubleshooting.md
│   └── advanced-patterns.md
├── examples/                 # Real-world scenarios
│   ├── 01-scenario.md
│   └── 02-scenario.md
└── tests/                    # Validation cases
    └── test.md
```

**Pattern:** Read SKILL.md first (always), then load references/ only when needed.

### Skill Frontmatter Format

Only 3 fields are valid:

```yaml
---
name: skill-name
description: What it does and when to use it
allowed-tools: "Read,Write,Edit,Grep,Bash"  # Optional, comma-separated string
---
```

**Common mistakes to avoid:**
- ❌ `when_to_use` field (merge into description)
- ❌ `version` field (put in markdown content)
- ❌ Array format: `[Read, Write]` (use string: `"Read,Write"`)

## Safety System

### Permissions (settings.local.json)

The `settings.local.json` file explicitly configures safety permissions. Claude Code also has built-in defaults beyond what's explicitly listed.

**Explicitly Allowed Operations:**
- File operations: `Read`, `Write`, `Edit`, `Grep`, `Glob`
- Git operations: `Bash(git *:*)`
- Basic shell: `Bash(ls:*)`, `Bash(pwd:*)`, `Bash(echo:*)`, `Bash(cat:*)`, `Bash(mkdir:*)`, `Bash(chmod:*)`, `Bash(find:*)`, `Bash(grep:*)`
- Development tools: `Bash(npm *:*)`, `Bash(node *:*)`, `Bash(python *:*)`, `Bash(pytest *:*)`, `Bash(pip *:*)`, `Bash(go *:*)`, `Bash(make *:*)`, `Bash(docker *:*)`, `Bash(docker-compose *:*)`
- Web access (restricted): `WebFetch(https://github.com/*)`, `WebFetch(https://docs.anthropic.com/*)`, `WebFetch(https://docs.claude.com/*)`

**Explicitly Blocked Operations:**
- Destructive file operations: `Bash(rm -rf:*)`, `Bash(rm -r *:*)`, `Bash(mv * /dev/null:*)`, `Bash(dd:*)`, `Bash(mkfs:*)`
- Privilege escalation: `Bash(sudo:*)`, `Bash(su:*)`
- Network tools: `Bash(curl:*)`, `Bash(wget:*)`, `Bash(nc:*)`, `Bash(netcat:*)`
- System file writes: `Bash(>/etc/*:*)`
- Command chaining with rm: `Bash(*; rm:*)`, `Bash(*&& rm:*)`
- Unsafe permissions: `Bash(chmod 777:*)`
- All other web fetch: `WebFetch(*)` (denies all except allowed GitHub/Anthropic docs)

**Path Ignore List:**
`node_modules/**`, `.git/**`, `dist/**`, `build/**`, `.next/**`, `__pycache__/**`, `*.pyc`, `.DS_Store`, `.env`, `.env.*`, `*.log`

### User Prompt Submit Hook

Context-aware micro-guidance before every prompt (≤6 lines, ~150 chars max):

**Phases detected:**
- **setup**: Platform validation, MCP connections
- **planning**: Load context, break down tasks
- **pre-execution**: Suggest /plan for complex tasks
- **execution**: Incremental /step reminders
- **review**: Run /learn, check /grow

**Pattern:** Hook provides just-in-time reminders based on what you're doing, not generic advice.

## Memory System

### Semantic Memory (What You Know)

```
memory/semantic/
├── tech/
│   ├── architecture-decisions.md
│   ├── tech-stack.md
│   └── tools.md
├── patterns/
│   ├── dev-patterns.md
│   ├── client-patterns.md
│   └── content-patterns.md
├── products/
│   └── product-vision.md
└── ideas/
    ├── product-ideas.md
    ├── business-ideas.md
    └── technical-ideas.md
```

**Update frequency:** After completing work via `/learn`

### Episodic Memory (What You've Done)

```
memory/episodic/
├── completed-work/
│   └── YYYY-MM-DD-description.md
└── content/
    └── published/
```

**Update frequency:** Automatically during `/learn` or `/content-chain`

### Procedural Memory (How You Do Things)

```
memory/procedural/
└── workflows/
    ├── dev-workflow.md
    ├── client-workflow.md
    └── content-workflow.md
```

**Update frequency:** When discovering new efficient processes

## Project Management

### Project Structure (Standard)

Every project has:

```
projects/[name]/
├── context.md       # Tech stack, timeline, constraints, contacts
├── tasks.md         # Urgent/High/Normal/Completed sections
├── patterns.md      # Project-specific patterns learned
└── notes.md         # Meeting notes, decisions, misc notes (optional)
```

**Context Preservation:**
- Projects remain fully dormant when inactive
- Zero state loss when switching between projects
- All context reloads automatically via `/switch`
- See `projects/INDEX.md` for project tracking

### Task Format

```markdown
## Urgent (Due Today)

- [ ] Task description
  Priority: URGENT | Added: YYYY-MM-DD | Est: Xh | Due: YYYY-MM-DD

## High Priority

- [ ] Task description
  Priority: HIGH | Added: YYYY-MM-DD | Est: Xh

## Normal

- [ ] Task description
  Priority: NORMAL | Added: YYYY-MM-DD | Est: Xh

## Completed

- [x] Task description
  Priority: HIGH | Added: YYYY-MM-DD | Completed: YYYY-MM-DD
```

### Project Health Indicators

Track project status in `projects/INDEX.md`:

- ✅ **Healthy** - Tasks on track, context current, patterns documented
- ⚠️ **Warning** - Outdated context, stale tasks, or missing patterns
- 🔴 **Critical** - Needs immediate attention, blocking issues

### Switching Projects

```bash
# Always use /switch for context loading
/switch project-name

# What it does:
# 1. Reads context.md
# 2. Loads open tasks
# 3. Shows recent activity
# 4. Loads relevant patterns from memory
# 5. Updates projects/CURRENT (tracking file)
# 6. Shows project health status
```

### Multi-Project Strategies

When juggling multiple active projects:

1. **Time Blocking** - Dedicate specific hours to each project
2. **Priority-Based** - Switch based on urgency from `/overview`
3. **Batching** - Group similar tasks across projects (all meetings, all coding, etc.)

See `projects/INDEX.md` for detailed multi-project coordination strategies.

## Specialized Agents

The system includes specialized agents in `.claude/agents/` for complex orchestration:

### Core Agents

1. **chief-of-staff.md**
   - Master orchestrator for multi-agent coordination
   - Manages context across workflows
   - Extracts learnings and maintains system overview

2. **claude-code-specialist.md**
   - Expert on Claude Code architecture
   - Context engineering and second brain systems
   - Truth verification and source attribution

3. **code-reviewer.md**
   - Code quality checks
   - Security analysis
   - Best practices enforcement

4. **content-strategist.md**
   - Creates content chains (blog → LinkedIn → video)
   - Uses Luca's voice and patterns
   - Multi-format content generation

5. **idea-categorizer.md**
   - Auto-categorizes captured ideas
   - Assigns priority and urgency
   - Routes to appropriate memory location

6. **project-coordinator.md**
   - Coordinates tasks across multiple projects
   - Manages priorities and dependencies
   - Tracks progress and blockers

**When agents are used:**
- Automatically invoked when patterns detected (e.g., content-strategist for `/content-chain`)
- Can be explicitly called for complex multi-step orchestration
- Coordinate with skills for execution

See `.claude/agents/` directory for detailed agent specifications.

## Use Cases (Workflow Patterns)

### 1. Development Workflow (dev-workflow/)
- Code implementation with architecture awareness
- Bug fixing with pattern recall
- Code review with learning extraction

### 2. Client PM Workflow (client-pm-workflow/)
- Multi-project task tracking
- Client communication patterns
- Deadline management

### 3. Idea Capture Workflow (idea-capture-workflow/)
- Voice-to-text transcription
- Auto-categorization (product/business/tech)
- Priority assignment

### 4. Content Chain Workflow (content-chain-workflow/)
- Blog → LinkedIn → Video script generation
- Viral pattern application
- Quality scoring (0-100)

## Integration Points

### Fibery MCP (Requires Setup)
- **Status:** Not configured by default - requires manual setup during onboarding
- Two-way sync capabilities: tasks, projects, ideas
- Configuration: `.claude/mcp.json` (must be created with API token)
- Setup instructions: See ONBOARDING.md Phase 1, Step 4

### Voice Input
- **Option A:** Claude mobile app + dictation
- **Option B:** OS dictation (macOS: Fn Fn, Windows: Win+H)
- **Option C:** Google Drive + Whisper API (advanced)
- See ONBOARDING.md Phase 1, Step 5

### Git Integration
- Auto-checkpoints during `/step` execution
- Pattern extraction from commit history
- Merge request analysis

## Commands Reference

### Planning & Execution
- `/plan [goal]` - Break complex task into ≤10 min steps
- `/step` - Execute next step with auto-checkpoint
- `/plan-status` - Progress tracking, time estimates, blockers

### Daily Workflow
- `/overview` - Morning dashboard across all projects
- `/switch [project]` - Context switching with auto-load
- `/learn` - Extract patterns from completed work
- `/grow` - Brain health metrics, ROI tracking

### Productivity
- `/idea [text]` - Quick idea capture with categorization
- `/recall [topic]` - Search all memories
- `/new-project [name]` - Scaffold new project
- `/add-task [description]` - Add task to current project

## Best Practices for Claude Code

### When Starting a Session

1. Read `projects/CURRENT` (created by /switch) to see active context
2. Load project context: `context.md`, `tasks.md`, `patterns.md`
3. Check for active plans: `.claude-plan.json`
4. Review recent memory: `memory/episodic/completed-work/`

### When Implementing Features

1. **Complex (>15 min):** Use `/plan` → `/step` pattern
2. **Simple (<5 min):** Execute directly
3. **Multi-project impact:** Check `memory/semantic/patterns/` first
4. **New patterns discovered:** Update via `/learn`

### When Switching Contexts

1. **Always use** `/switch [project]` (don't manually read files)
2. Let skill load context automatically
3. Review loaded tasks and patterns
4. Check for urgent items

### When Completing Work

1. Mark tasks complete in `tasks.md`
2. Run `/learn` to extract patterns
3. Update `patterns.md` if project-specific
4. Create episodic memory record

## Platform-Specific Notes

### macOS (Fully Supported)
- Dictation: System Settings → Keyboard → Enable (Fn twice)
- All features work out of box
- Terminal permissions: Grant for microphone if using voice

### Windows (WSL2 Recommended)
- Use WSL2 for best compatibility
- File paths: `/mnt/c/Users/...` format (not `C:\Users\...`)
- Git config: `git config --global core.autocrlf false`
- Multiline input: `Alt+Enter`
- Image upload: Use web version (not supported in WSL)

### Linux (Fully Supported)
- All features work natively
- Uses `$EDITOR` variable for text editing
- Compatible with tmux/screen
- Speech recognition varies by distribution

## Daily/Weekly/Monthly Rituals

### Morning Ritual

```bash
1. /overview              # Urgent tasks across all projects
2. Review capacity        # Check if overloaded
3. /switch [project]      # Load first project context
4. /plan [task]           # If complex (>15 min), plan first
```

### End-of-Day Ritual

```bash
/learn                    # Extract patterns before context fades
```

Questions:
- What did you complete?
- Root cause of issues?
- Seen this pattern before?
- Confidence level? (LOW/MEDIUM/HIGH)

### Weekly Ritual

```bash
/grow                  # Brain health metrics
```

Review:
- Patterns extracted this week
- Projects needing attention
- Plan next week's priorities

### Monthly Ritual

1. Review `projects/INDEX.md` - archive completed projects
2. Update tech stack in `memory/semantic/tech/`
3. Promote patterns: MEDIUM → HIGH if repeated 3+ times
4. Clean up stale ideas in `memory/semantic/ideas/`

## System Benefits

**Planning system:**
- Prevents context drift via incremental steps
- Manages rate limits through git checkpoints
- Enables resume from any step

**Memory system:**
- Pattern reuse across projects
- Zero context switching overhead
- Confidence promotion (LOW → MEDIUM → HIGH)

**Safety system:**
- Blocks destructive operations (rm -rf, sudo, etc.)
- Restricts network access to allowed domains
- Validates permissions before execution

## Troubleshooting

### Commands Not Recognized
1. Verify `.claude/commands/` directory exists
2. Check frontmatter YAML syntax
3. Restart Claude Code
4. Try reading command file directly: `Read .claude/commands/learn.md`

### Skills Not Loading
1. Check SKILL.md frontmatter (only 3 valid fields)
2. Verify `allowed-tools` uses string format: `"Read,Write"`
3. Check file permissions: `ls -la .claude/skills/*/SKILL.md`
4. Test manually: "Use [skill-name] skill to [task]"

### Memory Not Updating
1. Verify `/learn` completed successfully
2. Check file paths match expected structure
3. Manually inspect: `Read memory/semantic/patterns/dev-patterns.md`
4. Verify Write permissions in settings.local.json

### Planning System Issues
1. Check `.claude-plan.json` exists and is valid JSON
2. Verify steps are ≤10 minutes each
3. Use `/plan-status` to debug state
4. Delete plan file to reset: `rm .claude-plan.json`

## Key Documents

- **README.md** - Quick start and overview
- **ONBOARDING.md** - 5-phase setup guide (30 minutes to fully operational)
- **SKILLS_VS_COMMANDS.md** - Architecture: when to use what
- **SECOND_BRAIN_HABITS.md** - Daily/weekly/monthly routines
- **settings.local.json** - Safety permissions and configuration
- **.claude/skills/README.md** - Skills framework deep dive

## System Principles

1. **Knowledge Compounds** - Tasks add to semantic memory via `/learn`
2. **Zero Context Switching** - `/switch` loads full project context instantly
3. **Progressive Disclosure** - Read SKILL.md first, then references/ on-demand
4. **Incremental Execution** - `/plan` breaks tasks into ≤10 min steps
5. **Safety First** - settings.local.json blocks destructive operations
6. **Pattern Reuse** - Extract patterns once, apply across all projects
7. **Evidence-Based Learning** - Patterns promoted LOW → MEDIUM → HIGH by repetition

## For Future Claude Instances

When you're instantiated to work in this repository:

### Startup Sequence

1. **First:** Read this file (CLAUDE.md) - you're reading it now
2. **Second:** Check current context (`projects/CURRENT` - created by /switch)
3. **Third:** Load active project context:
   ```bash
   Read projects/[current]/context.md
   Read projects/[current]/tasks.md
   Read projects/[current]/patterns.md
   ```
4. **Fourth:** Check for active plans (`.claude-plan.json`)
   - If exists: Resume from current step
   - If not: Ready for new tasks
5. **Fifth:** Review recent memories if relevant to task:
   ```bash
   Read memory/semantic/patterns/[relevant-patterns].md
   Read memory/episodic/completed-work/[recent-dates].md
   ```
6. **Then:** Execute the user's request

### Decision Framework for Tasks

**Simple task (<5 min)?**
→ Execute directly, no planning needed

**Complex task (>15 min)?**
→ Use `/plan [goal]` first, then `/step` incrementally

**Switching projects?**
→ Always use `/switch [project]`, never manually read files

**Completed significant work?**
→ Run `/learn` before ending session

**User asks about system?**
→ Point to relevant docs: ONBOARDING.md, SKILLS_VS_COMMANDS.md, SECOND_BRAIN_HABITS.md

### Understanding Context

**This is not a normal codebase.** It's a knowledge compounding system where:

- **Commands** are the user interface (what user types)
- **Skills** are the implementation (how things execute)
- **Memory** is the knowledge store (what persists)
- **Planning** prevents context drift and rate limit issues
- **Rituals** ensure knowledge compounds over time

### Key Behaviors

**DO:**
- Use progressive disclosure (read SKILL.md first, then references/ if needed)
- Extract patterns via `/learn` after significant work
- Create git checkpoints during `/step` execution
- Verify dependencies before executing plan steps
- Update project health in `projects/INDEX.md`
- Follow the 10-minute rule for plan steps

**DON'T:**
- Skip the `/learn` ritual (wastes all learning from session)
- Create plans for simple tasks (<5 min)
- Manually switch projects (use `/switch`)
- Put >300 lines in SKILL.md (use references/)
- Use wrong frontmatter format (only 3 fields allowed)
- Execute destructive operations (permissions will block)

### Pattern Recognition

Watch for these signals to invoke specific workflows:

- **"I have an idea..."** → Suggest `/idea [text]`
- **"Let me work on..."** → Check complexity, suggest `/plan` if >15 min
- **"Switch to..."** → Use `/switch [project]`
- **"What did I..."** → Use `/recall [topic]`
- **"I just finished..."** → Prompt for `/learn`
- **"Show me what's urgent"** → Use `/overview`

### Memory Management

**Semantic memory** (what you know):
- Update via `/learn` after extracting patterns
- Promote confidence: LOW → MEDIUM → HIGH based on repetition
- Tech stack, architecture decisions, patterns

**Episodic memory** (what you've done):
- Auto-created during `/learn`
- Captures completed work with timestamps
- Used for recall and pattern validation

**Procedural memory** (how you do things):
- Updated when discovering new efficient workflows
- Represents optimized processes
- Applied automatically when pattern matches

### Success Indicators

System is working if:
- Patterns extracted regularly via `/learn`
- Plans complete without context drift
- Rate limits managed via incremental `/step`
- Knowledge reused across projects
- Project context switches instantly via `/switch`

**Critical behaviors:**
- Run `/learn` after significant work to extract patterns
- Use `/plan` + `/step` for complex tasks (>15 min) to prevent context drift
- Use `/switch` for project changes to load full context automatically
- Check permissions in settings.local.json before destructive operations
- Promote patterns (LOW → MEDIUM → HIGH) based on repetition count
