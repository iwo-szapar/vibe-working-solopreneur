# Notes - Dominic's C# Application

**Project:** dominic-csharp-app
**Last Updated:** 2025-11-11

**Purpose:** Capture architectural decisions, meeting notes, ideas, and miscellaneous project information.

---

## Architectural Decisions

### Decision Log

Document major architecture and design decisions here so you remember *why* you made them.

#### [Date] - [Decision Title]

**Context:**
[What situation led to this decision?]

**Decision:**
[What did you decide?]

**Reasoning:**
[Why did you choose this option?]

**Alternatives Considered:**
1. [Option 1] - Rejected because [reason]
2. [Option 2] - Rejected because [reason]

**Consequences:**
- Positive: [Benefits of this decision]
- Negative: [Trade-offs or downsides]

**Status:** Accepted / Superseded / Deprecated

---

#### Example: 2025-11-11 - Set Up Second Brain Repository

**Context:**
Building C# application while working full-time job. Experiencing frustration with context loss between Claude conversations. Need better way to preserve knowledge and speed up development.

**Decision:**
Set up Second Brain repository customized for C# development with context preservation system.

**Reasoning:**
- Solves primary pain point (context loss between conversations)
- Provides structured way to capture patterns and knowledge
- Enables 20hr/week time savings goal
- Built-in context snapshot system addresses conversation limits

**Alternatives Considered:**
1. Manual documentation - Rejected: too slow, inconsistent
2. Standard project README - Rejected: not enough structure for knowledge compounding
3. Note-taking app - Rejected: doesn't integrate with development workflow

**Consequences:**
- Positive: Systematic knowledge capture, context preservation, faster development
- Negative: Small upfront time investment to populate templates

**Status:** Accepted

---

## Meeting Notes

[If you have meetings with team members, partners, or advisors]

### [Date] - [Meeting Title]

**Attendees:** [Who was there]
**Duration:** [How long]

**Topics Discussed:**
1. [Topic 1]
2. [Topic 2]

**Decisions Made:**
- [Decision 1]
- [Decision 2]

**Action Items:**
- [ ] [Action for you]
- [ ] [Action for others]

**Follow-up:** [When is next meeting / what to do before then]

---

## Ideas & Brainstorms

### Feature Ideas

[Quick capture of feature ideas - move to tasks.md when ready to implement]

- **Idea:** [Feature description]
  - **Value:** [Why this would be useful]
  - **Complexity:** [Easy / Medium / Hard]
  - **Priority:** [Low / Medium / High]

---

### Technical Improvements

[Ideas for refactoring, optimization, or technical debt reduction]

- **Improvement:** [What to improve]
  - **Benefit:** [Why improve it]
  - **Effort:** [Time estimate]
  - **Priority:** [When to do it]

---

## Research Notes

[Notes from research into C# features, libraries, or best practices]

### [Date] - [Research Topic]

**Question:**
[What were you trying to learn?]

**Findings:**
- [Finding 1]
- [Finding 2]
- [Finding 3]

**Resources:**
- [Link 1]
- [Link 2]

**Conclusion:**
[What did you decide to do based on this research?]

**Applied:**
[Where you used this knowledge]

---

## Code Snippets & Utilities

[Useful code snippets you want to remember]

### [Snippet Name]

**Purpose:** [What it does]

**Code:**
```csharp
// Your code here
```

**Usage:**
[When/how to use this]

---

## Lessons Learned

[Document mistakes, gotchas, and lessons so you don't repeat them]

### [Date] - [Lesson Title]

**Situation:**
[What happened?]

**Problem:**
[What went wrong?]

**Root Cause:**
[Why did it happen?]

**Solution:**
[How did you fix it?]

**Prevention:**
[How to avoid this in the future?]

**Pattern Created:**
[If this led to a pattern, link to patterns.md]

---

## Dependencies & Third-Party Services

[Track external dependencies and why you chose them]

### NuGet Packages

| Package | Version | Purpose | Alternatives Considered |
|---------|---------|---------|-------------------------|
| [Package Name] | [Version] | [Why using it] | [What else you looked at] |

### External Services/APIs

| Service | Purpose | Cost | Alternative |
|---------|---------|------|-------------|
| [Service Name] | [What for] | [Price/month] | [Other options] |

---

## Performance Notes

[Performance benchmarks, optimization attempts, bottlenecks]

### [Date] - [Performance Issue]

**Symptom:**
[What was slow?]

**Measurement:**
[How slow was it?]

**Cause:**
[What was causing slowness?]

**Solution:**
[How did you speed it up?]

**Result:**
[Performance after optimization]

---

## Security Notes

[Security considerations, vulnerabilities addressed, authentication decisions]

### [Date] - [Security Topic]

**Issue:**
[What security concern?]

**Risk Level:** Low / Medium / High

**Mitigation:**
[How you're addressing it]

**Status:** Addressed / In Progress / Backlog

---

## Deployment & DevOps

[Notes about deployment, hosting, CI/CD]

### Deployment Checklist

- [ ] Database backup before deploy
- [ ] Run migrations
- [ ] Update configuration
- [ ] Test critical paths
- [ ] Monitor for errors

### Environment Configuration

**Development:**
- Database: [Connection details]
- API keys: [Where stored]

**Production:**
- Database: [Connection details]
- Hosting: [Platform, region]
- API keys: [Where stored - NEVER commit secrets!]

---

## Database Schema Notes

[Notes about database design decisions]

### Tables

**[Table Name]:**
- Purpose: [What this stores]
- Key fields: [Important columns]
- Relationships: [How it relates to other tables]
- Indexes: [What's indexed and why]

---

## Troubleshooting & Debugging Notes

[Common issues and their solutions]

### [Issue Title]

**Symptom:**
[What error or behavior?]

**Diagnosis:**
[How you figured out the cause]

**Solution:**
[How to fix it]

**Prevention:**
[How to avoid in the future]

---

## Integration Notes

[If integrating with other systems or APIs]

### [Integration Name]

**Purpose:** [Why integrating]
**Documentation:** [Link to their docs]
**Authentication:** [How auth works]
**Rate Limits:** [Any limits to be aware of]
**Gotchas:** [Things to watch out for]

---

## Version Control Notes

### Branch Strategy

[How you organize branches]

**Main branches:**
- `main` - Production-ready code
- `develop` - Integration branch
- `feature/*` - Feature branches

**Workflow:**
1. Create feature branch from develop
2. Develop and commit
3. Merge back to develop
4. Test in develop
5. Merge to main when ready for production

### Commit Message Format

[Your preferred format]

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:** feat, fix, docs, refactor, test, chore

---

## Technical Debt

[Track known technical debt]

| Issue | Impact | Effort | Priority | Created |
|-------|--------|--------|----------|---------|
| [Description] | [High/Med/Low] | [Hours] | [Priority] | [Date] |

---

## Questions & Decisions Pending

[Things you need to decide or get answers for]

- [ ] **Question:** [Your question]
  - **Options:** [Possible answers]
  - **Decision by:** [Date]
  - **Who to ask:** [If you need input]

---

## Success Stories

[Document wins and milestones - important for morale!]

### [Date] - [Achievement]

**What:**
[What did you accomplish?]

**Impact:**
[Why does it matter?]

**Lessons:**
[What did you learn?]

**Time Saved:**
[If applicable, how much time did AI assistance save?]

---

## Context for AI

**How to use this file:**
- Add notes as you work
- Don't worry about perfect organization - capture first, organize later
- Use this for anything that doesn't fit in context.md, tasks.md, or patterns.md
- Review monthly and move stale items to archive

**When AI should reference this:**
- Looking for architectural decisions
- Understanding why something was done a certain way
- Finding research conclusions
- Reviewing lessons learned

---

## Monthly Review Notes

### [Month Year]

**Completed this month:**
- [Major accomplishment 1]
- [Major accomplishment 2]

**Challenges faced:**
- [Challenge 1 and how addressed]
- [Challenge 2 and how addressed]

**Patterns learned:**
- [New patterns discovered]

**Next month focus:**
- [Priority 1]
- [Priority 2]

**Time saved with AI:**
- [Estimate of hours saved]

---

## Notes

- This file is your project journal
- Don't overthink it - just capture what's useful
- Review periodically and move important items to proper locations
- Archive old notes when they're no longer relevant
- Use `/learn` to document completed work in memory/episodic/ and @-mention to restore context
