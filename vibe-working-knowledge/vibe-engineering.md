# Vibe Engineering

## Philosophy

**Each unit of engineering work should make subsequent units of work easier—not harder.**

Traditional development accumulates technical debt. Every feature adds complexity. Every change increases maintenance burden. The codebase becomes harder to work with over time.

Vibe engineering inverts this. Each feature you build:
- Documents patterns for the next feature
- Creates reusable components that accelerate future work
- Establishes conventions that reduce decision fatigue
- Codifies knowledge that compounds across the team

```
   PLAN          DELEGATE        ASSESS         CODIFY
    │                │              │              │
    │  Plan it       │  Do the      │  Make sure   │  Record
    │  out in        │  work        │  it works    │  learnings
    │  detail        │              │              │
    │                │              │              │
    └──────────────► └────────────► └────────────► └──────┐
                                                            │
    ┌───────────────────────────────────────────────────────┘
    │
    └──► (cycle repeats)
```

## Core Principles

**Prefer duplication over complexity:** Simple, clear code that's easy to understand beats complex abstractions.

**Document as you go:** Generate documentation—issues, todos, review findings—that makes future work easier.

**Quality compounds:** High-quality code is easier to modify. Every change should meet your quality bar.

**Systematic beats heroic:** Consistent processes beat individual heroics. Execute plans systematically with continuous validation.

**Knowledge should be codified:** Learnings should be captured and reused. Analyze your codebase to apply your own patterns.

## The Vibe Engineering Workflow

### 1. Plan: Turn Ideas Into Structured Issues

Transform feature descriptions into comprehensive issues that make implementation easier.

**Process:**
- Research your codebase to find similar patterns and conventions
- Analyze framework documentation and best practices
- Create detailed acceptance criteria and implementation plans
- Generate code examples that follow your existing patterns

**Detail Levels:**
- **Minimal:** Quick issues for simple features
- **More:** Standard issues with technical considerations
- **A lot:** Comprehensive issues for major features

**Example:**
```bash
/plan "Add user profile avatars with S3 upload and automatic resizing"
```

**The result:** Issues that have already done the research and planning work.

### 2. Work: Execute Plans Systematically

Execute work plans with isolated worktrees and systematic task tracking.

**Process:**
- Create isolated git worktrees for clean development
- Break down plans into trackable todos
- Execute tasks systematically with continuous validation
- Run tests and quality checks after each change

**Example:**
```bash
/work path/to/plan.md
```

**The result:** Features built correctly the first time, with full test coverage and no regressions.

### 3. Review: Ensure Quality Before Merging

Perform exhaustive multi-agent code reviews before merging.

**Process:**
- Check out PR in an isolated worktree for deep analysis
- Run 12+ specialized review agents in parallel
- Identify security issues, performance problems, and architectural concerns
- Create trackable todos for every finding

**Example:**
```bash
# Review the latest PR
/review

# Review a specific PR
/review 123

# Review from a GitHub URL
/review https://github.com/user/repo/pull/123
```

**The result:** Code that meets your quality bar and documents learnings for future work.

## Available Commands

### Core Workflow Commands

**`/plan [feature description]`**
Creates detailed GitHub issues from feature descriptions. Includes research, acceptance criteria, and implementation guidance.

**`/work [plan file]`**
Executes work plans systematically with worktrees, todos, and continuous validation.

**`/review [PR number or URL]`**
Performs exhaustive multi-agent code reviews with security, performance, and architecture analysis.

**`/learn`**
Extracts patterns from completed work and updates semantic memory with learnings.

### Daily Operations Commands

**`/switch [project]`**
Switches to a different project, loading its context and showing recent activity.

**`/overview`**
Daily overview of all tasks, deadlines, and priorities across all projects.

**`/add-task [description]`**
Add a task to the current project with priority, due date, and time estimate.

**`/recall [topic]`**
Search all memories for relevant information by topic or keyword.

### Planning Commands

**`/step`**
Execute the next step in the current plan incrementally.

**`/plan-status`**
Show current plan progress, completed steps, and what's next.

### Advanced Commands

**`/triage`**
Presents findings one by one for review and converts approved items into trackable todos.

**`/resolve_todo_parallel`**
Resolves multiple todos in parallel with systematic execution and quality checks.

**`/idea [text]`**
Quick idea capture with automatic categorization.

**`/grow`**
Show brain health report with growth metrics, patterns learned, and time savings.

**`/new-project [name]`**
Create a new project with standard structure and context files.

**`/generate_command`**
Generates new Claude Code commands from descriptions.

## Specialized Agents

### Code Review Specialists

**rails-reviewer:** Strict Rails code review with focus on conventions and simplicity

**typescript-reviewer:** TypeScript code review with type safety and best practices

**python-reviewer:** Python code review with focus on clarity and conventions

**dhh-rails-reviewer:** Rails review from DHH's philosophy of simplicity

**code-simplicity-reviewer:** Identifies opportunities to simplify complex code

### Quality Guardians

**security-sentinel:** Comprehensive security audits and vulnerability detection

**performance-oracle:** Performance analysis and optimization recommendations

**data-integrity-guardian:** Database design review and data consistency checks

### Architecture & Patterns

**architecture-strategist:** System design review and architectural guidance

**pattern-recognition-specialist:** Identifies patterns and suggests improvements

### Research & Analysis

**repo-research-analyst:** Analyzes repository patterns and conventions

**best-practices-researcher:** Researches best practices for technologies

**framework-docs-researcher:** Fetches relevant framework documentation

**git-history-analyzer:** Analyzes git history for context and patterns

### Workflow & Communication

**every-style-editor:** Edits content to match style guides

**feedback-codifier:** Converts feedback into actionable improvements

**pr-comment-resolver:** Systematically resolves PR review comments

### Orchestration & Coordination

**chief-of-staff:** Master orchestrator for multi-agent coordination and context management

**claude-code-specialist:** Expert on Claude Code architecture and second brain systems

**project-coordinator:** Coordinates tasks across multiple projects with priority management

**idea-categorizer:** Auto-categorizes captured ideas and assigns priority

### Content & Communication

**content-strategist:** Creates content chains (blog → LinkedIn → video) with voice patterns

**daily-accountability-coach:** Daily planning with priority coaching and progress tracking

## How Vibe Engineering Makes Development Compound

Traditional development tools help you work faster. Vibe engineering tools make future work easier.

**Every plan you create:**
- Documents patterns that inform the next plan
- Establishes conventions that reduce planning time
- Builds institutional knowledge

**Every work execution:**
- Creates reusable components
- Refines your testing approach
- Improves your development process

**Every review you run:**
- Catches issues earlier
- Documents learnings for the team
- Raises the quality bar systematically

Over time, you're not just building features—you're building a development system that gets better with each use.

## Getting Started with Vibe Engineering

1. Use `/plan` on your next feature idea
2. Execute with `/work` to implement systematically
3. Run `/review` before merging
4. Repeat, and watch your development process compound

Each cycle makes the next cycle easier. That's vibe engineering.

## Working with This System

### When Planning Is Needed

Follow the vibe engineering process for:

1. **Plan** → Understand the change needed and its impact
2. **Delegate** → Use AI tools to help with implementation
3. **Assess** → Verify changes work as expected
4. **Codify** → Update documentation with learnings

### Testing Changes

**Validate functionality:**
```bash
# Test commands
/review
/plan "test feature"
```

**Validate code quality:**
- Run tests after every change
- Use specialized review agents
- Verify acceptance criteria

### Common Tasks

**Adding a New Agent:**
1. Create agent definition file
2. Test with sample input
3. Document use cases and capabilities
4. Update agent list in this document

**Adding a New Command:**
1. Create command definition file
2. Test with various inputs
3. Document usage and examples
4. Update command list in this document

**Updating Workflows:**
- Document new patterns discovered
- Update this file with learnings
- Share insights with team

## Commit Conventions

Follow these patterns for commit messages:

- `Add [agent/command name]` - Adding new functionality
- `Remove [agent/command name]` - Removing functionality
- `Update [file] to [what changed]` - Updating existing files
- `Fix [issue]` - Bug fixes
- `Simplify [component] to [improvement]` - Refactoring

Include the Claude Code footer:

```
🤖 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude <noreply@anthropic.com>
```

## Key Learnings

_This section captures important learnings as we work with vibe engineering._

### Focus on Simplicity

The initial approach included many complex features. We simplified to focus on core workflows:

- Required: Planning, execution, and review
- Optional: Advanced agent coordination
- Core philosophy: Each unit of work should make the next easier

**Learning:** Stick to essential workflows. Additional complexity should only be added when patterns emerge from repeated use.

### Systematic Over Heroic

Consistent, repeatable processes beat individual heroics. The systematic approach:
- Creates isolated worktrees for clean development
- Tracks todos for visibility
- Validates continuously
- Documents learnings automatically

**Learning:** Build systems that anyone can follow, not processes that depend on individual expertise.
