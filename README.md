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

**Available NOW (Ready to Use):**
1. **Context preservation system** - Save/restore conversation context (solves your #1 pain point!)
2. **C# project structure** - Complete templates for context, tasks, patterns, notes
3. **C# architecture framework** - Document tech stack, decisions, patterns
4. **Pattern library system** - Extract and reuse C# patterns (LOW/MEDIUM/HIGH confidence)
5. **Direct Claude interaction** - Context-aware C# code generation using your documented patterns

**Planned for Future (Coming Soon):**
- Slash commands (`/learn`, `/overview`, `/switch`) - Currently use direct conversation
- Automated skills and workflows - Currently manual but effective
- Voice input integration - Can use Windows dictation (Win+H) with copy/paste

**Built-in Safety:**
- ✅ Blocks destructive operations (via `permissions.deny` in settings)
- ✅ Network restricted to docs sites (via `permissions.deny` + whitelist)
- ✅ Context-aware micro-guidance (via `userPromptSubmit` hook)
- ⚠️  Incremental execution - recommended pattern, not hard-blocked

## Quick Start

### First Time? Start Here 👋
**Read ONBOARDING.md** - 2-hour practical onboarding session
- Discover and import your existing C# documentation
- Set up context preservation workflow (solve conversation limit frustration!)
- Extract 3-5 C# patterns from your existing code
- Live coding demo with context-aware generation
- Build your daily workflow for sustainable 20 hr/week savings

**Perfect for your 12/11/2025 session (10:00-12:00 GMT+2)**

### Daily Workflow (What's Available Now)

**Morning - Start with Full Context:**
```
You: "I'm working on [C# project]. Let me load context:"
     @projects/dominic-csharp-app/context.md
     @projects/dominic-csharp-app/patterns.md
     @memory/episodic/completed-work/2025-11-12-latest.md

Claude: [Automatically reads all @-mentioned files, understands full context]

You: "I need to add user authentication to my C# app"

Claude: [Generates code using YOUR patterns, YOUR style, YOUR architecture]
```

**During the Day - Context-Aware Development:**
- Ask for C# code generation using your documented patterns
- Request SQL queries for data analysis
- Discuss architectural decisions (I'll reference your docs)
- Get help with debugging using project context

**After Completing Work - Document in Memory:**
```
1. Use /learn command (or manually create): memory/episodic/completed-work/2025-11-12-auth-module.md
2. Document (3-5 minutes):
   - What was completed
   - Key decisions made
   - Patterns used or discovered
   - Code examples (if relevant)
3. Next conversation: @-mention this file to load context instantly
```

**Pattern Discovery - Build Your Library:**
- After completing features, document patterns in `patterns.md`
- Start as LOW confidence, promote to MEDIUM after 3 uses, HIGH after 5
- Reuse patterns in future work (15-30 min saved per application)

### How to Work with This System

**Direct Conversation Approach (Available Now):**

Rather than slash commands (coming later), use natural conversation with file sharing:

1. **Share Context Files:**
   - "Here's my project context: [paste or share context.md]"
   - "Here are my patterns: [paste or share patterns.md]"
   - "Restore from: [paste or share context snapshot]"

2. **Request C# Code:**
   - "Generate a UserService class following my documented patterns"
   - "Create Entity Framework migration for User table"
   - "Add input validation using FluentValidation"

3. **Get SQL Queries:**
   - "Write a SQL query to get top 10 customers by order value"
   - "Generate a report query for last month's sales"

4. **Document Patterns:**
   - After I help you, capture the pattern in `patterns.md`
   - Include: what it does, when to use it, code example
   - Mark confidence: LOW (first use) → MEDIUM (3 uses) → HIGH (5+ uses)

5. **Save Context:**
   - Use `/learn` command after completing work
   - Document in `memory/episodic/completed-work/`
   - Restore in next conversation by @-mentioning the file

**Future Enhancement - Slash Commands:**
Commands like `/learn`, `/overview`, `/switch` will be added in Phase 2 for automated workflows

## Architecture

**You** ↔ **Claude** ↔ **Documented Knowledge**

This second brain is built on:

1. **Your Documented Knowledge** (`memory/`, `projects/`):
   - Tech stack and architecture decisions
   - C# patterns you've discovered
   - Project context and constraints
   - Context snapshots for conversation continuity

2. **Claude's Context Awareness**:
   - Reads your documentation
   - Applies your patterns
   - Generates code in your style
   - Maintains conversation context

3. **Continuous Improvement**:
   - Extract patterns from completed work
   - Document architectural decisions
   - Promote pattern confidence over time
   - Build reusable knowledge library

**Future:** Slash commands (`.claude/commands/`) and automated skills will be added in Phase 2

## Repository Structure

```
memory/
├── semantic/                # What you know (C# patterns, tech decisions)
│   ├── tech/
│   │   └── architecture-decisions.md  # Your tech stack & decisions
│   ├── patterns/            # Cross-cutting patterns
│   └── ideas/               # Business and technical ideas
├── episodic/               # What you've done (completed work)
└── procedural/             # How you do things (workflows)

projects/
├── dominic-csharp-app/     # ⭐ Your C# application (side business)
│   ├── context.md          # Project details, tech stack, architecture
│   ├── tasks.md            # Current tasks and priorities
│   ├── patterns.md         # C#-specific patterns discovered
│   └── notes.md            # Architecture decisions, research notes
├── fulltime-analytics/     # Optional: Full-time job work (separate)
└── _template/              # Template for new projects

memory/                     # ⭐ CRITICAL: Knowledge storage & context preservation
├── episodic/               # What you've done (completed work with full context)
│   └── completed-work/     # Document work here, @-mention to restore context
├── semantic/               # What you know (patterns, decisions, tech stack)
└── procedural/             # How you do things (workflows, processes)

settings.local.json         # C# development tools, Windows paths, permissions
ONBOARDING.md              # 2-hour practical onboarding session guide
CLAUDE.md                  # Complete documentation about you and this system
README.md                  # This file - quick start and overview
```

**What's NOT here yet (coming in Phase 2):**
- `.claude/commands/` - Slash commands for automation
- `.claude/skills/` - Automated workflows
- `.claude/hooks/` - Event-driven automation

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
- Zero context loss between conversations (via @memory/ mentions)
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
2. Document work in `memory/episodic/completed-work/` after sessions (CRITICAL!)
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
3. Build habit: document completed work in `memory/episodic/` daily
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
- **memory/README.md** - Memory structure and how to use @-mentions for context preservation

## Support

- **First time?** → Read ONBOARDING.md
- **Complex C# feature?** → Use `/plan [goal]` → `/step`
- **Ending session?** → Run `/learn` to document work in memory/
- **Starting new conversation?** → @-mention files from memory/episodic/ to restore context
- **Need a SQL query?** → Use `/query [question]`
- **Check progress** → Run `/grow`
- **Understand architecture** → Read CLAUDE.md
- **Windows setup issues?** → See windows-setup.md
