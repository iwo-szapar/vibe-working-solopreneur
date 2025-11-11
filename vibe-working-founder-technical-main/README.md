# Dominic's Second Brain
**C# Developer & Analytics Edition**

Your AI-powered second brain that compounds knowledge across all your work domains, with special focus on C# development, data analysis, and context preservation across conversations.

## What You Have

**Core Foundation:**
- Universal memory model (semantic, episodic, procedural)
- Knowledge compounding system
- Multi-project support with zero context-switching overhead
- Growth tracking with brain health metrics
- **Planning system** - Break complex tasks into ≤10 min steps
- **Safety system** - Prevents destructive operations, manages rate limits

**Installed Use Cases (C# Developer & Analytics Persona):**
1. `dev-workflow` - C# development, architecture, code review
2. `data-analysis-workflow` - SQL queries, data analysis, insights generation
3. `project-workflow` - Complex project management and task tracking
4. `idea-capture-workflow` - Voice/text idea capture for business and technical ideas
5. **Context preservation** - Save and restore conversation context to combat conversation limits

**Built-in Safety:**
- ✅ Blocks destructive operations (via `permissions.deny` in settings)
- ✅ Network restricted to docs sites (via `permissions.deny` + whitelist)
- ✅ Context-aware micro-guidance (via `userPromptSubmit` hook)
- ⚠️  Incremental execution - recommended pattern, not hard-blocked

## Quick Start

### First Time? Start Here 👋
**Read ONBOARDING.md** - 5-phase setup guide (30 minutes)
- Validates your Windows platform and development environment
- Configures C# development tools (.NET SDK, VS Code/VS Community)
- Sets up voice input options (Windows dictation or Claude mobile)
- Creates your C# project structure
- Walks through core workflows for code generation and context preservation

### Daily Workflow
```bash
# Morning: See what's urgent
/overview

# Switch to your C# project
/switch dominic-csharp-app

# For complex C# features (>15 min): Plan first
/plan Add user authentication module to C# application
/step              # Execute step 1
/step              # Execute step 2 (incremental progress)
/plan-status       # Check progress

# For data analysis tasks
/query "Get top 10 customers by order value last month"

# Capture ideas on the go
/idea "Implement caching layer for frequently accessed data"

# CRITICAL: Save context before hitting conversation limit
/context-save      # Preserve current work state

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
- `/overview` - Morning dashboard for your project
- `/switch [project]` - Zero-overhead context switching
- `/learn` - Extract patterns from recent work
- `/grow` - Brain health metrics and ROI

**Context Preservation (Critical for Conversation Limits):**
- `/context-save` - Save current conversation state before hitting limit
- `/context-restore [snapshot]` - Restore previous conversation context
- `/recall [topic]` - Search all memories

**Productivity:**
- `/idea [text]` - Quick idea capture
- `/add-task [description]` - Add task to current project

**C# & Data Analysis:**
- `/query [question]` - Generate SQL queries for data analysis
- `/code-review [file]` - Review C# code for issues

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

projects/           # Your active projects
├── dominic-csharp-app/      # Your C# application (side business)
│   ├── context.md           # Project details, tech stack, architecture
│   ├── tasks.md             # Current tasks and priorities
│   ├── patterns.md          # C#-specific patterns discovered
│   └── notes.md             # Architecture decisions, meeting notes
├── fulltime-analytics/      # Optional: Full-time job work (kept separate)
└── _template/               # Template for new projects

context-snapshots/  # CRITICAL: Conversation context preservation
└── YYYY-MM-DD-HHMM-[topic].md  # Saved contexts to restore after conversation limits

settings.local.json  # NEW: Safety permissions & plan enforcement
ONBOARDING.md        # NEW: 5-phase setup guide
```

## Your Success Metrics (6 months)

**Target time savings:** 20 hours/week (Currently achieved!)
- C# code generation: 10 hr/week (faster, more accurate code with less copy/paste)
- Data analysis & SQL queries: 5 hr/week (automated query generation)
- Context preservation: 3 hr/week (no time lost re-explaining context)
- Documentation generation: 2 hr/week (automated from code and patterns)
- **Total: 20 hr/week = 80+ hours/month**

**Quality improvements:**
- Better, more accurate C# code generation
- Consistent patterns across your growing codebase
- Zero context loss between conversations (via context-snapshots)
- Less copy/paste friction (direct IDE integration workflow)
- Smarter end results with context-aware assistance

**Safety benefits:**
- Zero destructive operations (protected by permissions)
- No rate limit waste (incremental checkpoints)
- No context drift (step-by-step execution + context preservation)

## Next Steps

### 🚀 Day 1: Setup (30 minutes)
1. **Read ONBOARDING.md** - Complete setup guide
2. Validate Windows environment (Git, .NET SDK, VS Code/VS Community)
3. Set up voice integration (Windows dictation: Win+H, or Claude mobile)
4. Import your existing C# project documentation

### 📅 Week 1: Core Workflows & Context Preservation
1. Run `/overview` each morning to see your tasks
2. Practice `/context-save` before conversation limits (CRITICAL!)
3. Try `/plan [goal]` → `/step` for complex C# features
4. Run `/learn` after completing work to extract patterns
5. Check `/grow` for brain health

### 🎯 Week 2: Build Your C# Knowledge Base
1. Populate `projects/dominic-csharp-app/context.md` with architecture details
2. Add current tasks with `/add-task`
3. Document C# patterns you've already discovered in `patterns.md`
4. Use `/query` for data analysis and SQL generation

### 🔧 Week 3: Advanced Features & Optimization
1. Customize permissions in `settings.local.json` for your tools
2. Set up regular `/learn` ritual after coding sessions
3. Create context snapshot habit before hitting conversation limits
4. Review and refine your C# patterns (LOW → MEDIUM → HIGH confidence)

## Platform-Specific Setup

**Windows (Primary Platform):** See CLAUDE.md → Platform-Specific Notes
- **Voice Input:** Windows dictation (Win+H) or Claude mobile app
- **Development:** VS Code or VS Community for C# development
- **.NET SDK:** Ensure latest version installed (currently using .NET with C#)
- **Git:** Git for Windows (version 2.51.2.windows.1)
- **Node.js:** Version 22.14.0 (for any tooling needs)
- **Python:** Version 3.11.9 (for data analysis scripts)
- **File paths:** Windows native paths (e.g., `C:\Users\dominica\...`)
- **Alt+Enter:** For multiline input in Claude Code CLI

**For more details:** See `windows-setup.md` guide

## Key Documents

- **ONBOARDING.md** - Start here! Setup guide for C# development
- **CLAUDE.md** - Complete system documentation
- **windows-setup.md** - Windows-specific setup and configuration
- **settings.local.json** - Safety permissions & C# development tools
- **context-snapshots/README.md** - How to preserve context between conversations

## Support

- **First time?** → Read ONBOARDING.md
- **Complex C# feature?** → Use `/plan [goal]` → `/step`
- **Hit conversation limit?** → Use `/context-save` BEFORE limit
- **Starting new conversation?** → Use `/context-restore` to resume
- **Need a SQL query?** → Use `/query [question]`
- **Check progress** → Run `/grow`
- **Understand architecture** → Read CLAUDE.md
- **Windows setup issues?** → See windows-setup.md
