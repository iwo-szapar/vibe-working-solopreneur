# Luca's Second Brain
**Technical Founder Edition**

Your AI-powered second brain that compounds knowledge across all your work domains.

## What You Have

**Core Foundation:**
- Universal memory model (semantic, episodic, procedural)
- Knowledge compounding system
- Multi-project support with zero context-switching overhead
- Growth tracking with brain health metrics
- **Planning system** - Break complex tasks into ≤10 min steps
- **Safety system** - Prevents destructive operations, manages rate limits

**Installed Use Cases (Technical Founder Persona):**
1. `dev-workflow` - Code, architecture, review
2. `client-pm-workflow` - Multi-project task tracking
3. `idea-capture-workflow` - Voice/text idea capture
4. `content-chain-workflow` - Blog → LinkedIn → Video
5. `content-generation` - Multi-format content creation

**Built-in Safety:**
- ✅ Blocks destructive operations (via `permissions.deny` in settings)
- ✅ Network restricted to docs sites (via `permissions.deny` + whitelist)
- ✅ Context-aware micro-guidance (via `userPromptSubmit` hook)
- ⚠️  Incremental execution - recommended pattern, not hard-blocked

## Quick Start

### First Time? Start Here 👋
**Read ONBOARDING.md** - 5-phase setup guide (30 minutes)
- Validates your platform (Windows/Mac/Linux)
- Sets up Fibery MCP integration
- Configures voice input options
- Creates your first project
- Walks through core workflows

### Daily Workflow
```bash
# Morning: See what's urgent
/overview

# Switch to a project
/switch client-a

# For complex tasks (>15 min): Plan first
/plan Add Stripe webhook idempotency
/step              # Execute step 1
/step              # Execute step 2 (incremental progress)
/plan-status       # Check progress

# For simple tasks: Use directly
/idea "Add real-time notifications to dashboard"

# Create content
/content-chain "Building with AI" blog

# End of day: Extract learnings
/learn
/grow              # Check brain health
```

### Core Commands

**Planning & Execution:**
- `/plan [goal]` - Break complex task into 5-step plan (≤10 min each)
- `/step` - Execute next step incrementally with auto-checkpoint
- `/plan-status` - Show progress, time tracking, blockers

**Daily Workflow:**
- `/overview` - Morning dashboard across all projects
- `/switch [project]` - Zero-overhead context switching
- `/learn` - Extract patterns from recent work
- `/grow` - Brain health metrics and ROI

**Productivity:**
- `/idea [text]` - Quick idea capture
- `/recall [topic]` - Search all memories
- `/new-project [name]` - Scaffold new project
- `/add-task [description]` - Add task to current project

**Content Creation:**
- `/content-chain [topic] [format]` - Blog → LinkedIn → Video chain

## Architecture

**Commands** → **Skills** → **Memory**

This second brain uses a three-layer architecture:

1. **Commands** (`.claude/commands/`): User-facing slash commands like `/learn`, `/recall`, `/overview`
2. **Skills** (`.claude/skills/`): Executable workflows with examples, tests, and progressive disclosure (10 skills, 43 files)
3. **Memory** (`memory/`): Knowledge storage (semantic, episodic, procedural)

Each command invokes one or more skills for execution. Skills use progressive disclosure to load only what's needed.

## Structure

```
.claude/
├── commands/         # User-facing slash commands (11 commands)
│   ├── plan.md              # NEW: Break tasks into steps
│   ├── step.md              # NEW: Incremental execution
│   ├── plan-status.md       # NEW: Progress tracking
│   ├── learn.md
│   ├── overview.md
│   └── ...
├── skills/          # 10 executable skills with examples & tests
│   ├── content-generation/  # NEW: Blog → LinkedIn → Video
│   ├── learn-extraction/
│   ├── memory-recall/
│   └── ...
├── hooks/           # NEW: Safety and guidance
│   └── user-prompt-submit.sh  # Context-aware micro-guidance
└── agents/          # Specialized AI agents

memory/
├── semantic/        # What you know (facts, patterns)
├── episodic/       # What you've done (experiences)
└── procedural/     # How you do things (workflows)

projects/           # Your active projects (4 real examples)
├── client-a-ecommerce/      # NEW: Real e-commerce project
│   ├── context.md
│   ├── tasks.md
│   └── patterns.md
├── client-b-analytics/      # NEW: Real analytics project
├── internal-saas/           # NEW: Real SaaS project
└── content-ai-patterns-course/  # NEW: Real content project

settings.local.json  # NEW: Safety permissions & plan enforcement
ONBOARDING.md        # NEW: 5-phase setup guide
```

## Your Success Metrics (6 months)

**Target time savings:**
- Planning: 2.5 hr/week (incremental execution, no context drift)
- Idea capture: 2 hr/week (voice integration)
- Task tracking: 1 hr/week (multi-project overview)
- Content creation: 3 hr/week (content chains)
- **Total: 8.5 hr/week = 34 hours/month**

**Safety benefits:**
- Zero destructive operations (protected by permissions)
- No rate limit waste (incremental checkpoints)
- No context drift (step-by-step execution)

## Next Steps

### 🚀 Day 1: Setup (30 minutes)
1. **Read ONBOARDING.md** - Complete 5-phase setup
2. Validate platform (Windows/Mac/Linux)
3. Configure Fibery MCP (if using Fibery)
4. Set up voice integration (Claude mobile or OS dictation)

### 📅 Week 1: Core Workflows
1. Run `/overview` each morning
2. Try `/plan [goal]` → `/step` for complex task
3. Run `/learn` after completing work
4. Check `/grow` for brain health

### 🎯 Week 2: Project Setup
1. Create your first real project with `/new-project`
2. Populate `context.md` with project details
3. Add tasks with `/add-task`
4. Switch between projects with `/switch`

### 🔧 Week 3: Advanced Features
1. Set up Fibery integration (see ONBOARDING.md Phase 4)
2. Create your first content chain: `/content-chain [topic] blog`
3. Review 4 example projects for inspiration
4. Customize permissions in `settings.local.json`

## Platform-Specific Setup

**Windows (WSL2):** See CLAUDE.md → Platform-Specific Notes
- Use `Alt+Enter` for multiline input
- File paths: `/mnt/c/Users/...` format
- Image upload: Use web version

**macOS:** See CLAUDE.md → Platform-Specific Notes
- Dictation: Fn twice
- All features fully supported

**Linux:** See CLAUDE.md → Platform-Specific Notes
- All features work out of box

## Key Documents

- **ONBOARDING.md** - Start here! 5-phase setup guide
- **CLAUDE.md** - Complete system documentation
- **settings.local.json** - Safety permissions & configuration
- **.claude/skills/README.md** - Skills framework deep dive

## Support

- **First time?** → Read ONBOARDING.md
- **Complex task?** → Use `/plan [goal]` → `/step`
- **See all commands** → Run `/help`
- **Check progress** → Run `/grow`
- **Understand architecture** → Read CLAUDE.md
- **Platform issues?** → See CLAUDE.md Platform-Specific Notes
