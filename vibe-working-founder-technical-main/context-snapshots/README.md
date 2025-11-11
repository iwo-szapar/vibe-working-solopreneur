# Context Snapshots - Combat Conversation Limits

**Purpose:** Save your current conversation context before hitting Claude's conversation limits, so you can restore it perfectly in a new conversation.

**Your #1 Pain Point SOLVED:** No more frustrating context loss between conversations!

---

## Why This Matters for You

Based on your questionnaire:
- You use Claude **multiple times per day**
- Your biggest frustration: **"When Claude reaches conversation limits and requires a restart from one conversation to the next - very frustrating"**
- You save **20 hours/week** with Claude - losing context wastes precious time
- Your C# project is **increasing in complexity** - context is critical

**This system eliminates that frustration.**

---

## How It Works

### When to Save Context

Save a context snapshot when you notice:
- ⚠️ Approaching 100+ messages in the conversation
- ⚠️ Claude's responses getting slower or less accurate
- ⚠️ Complex C# feature that will take multiple conversations
- ⚠️ Important work in progress that you can't lose
- ⚠️ Before taking a break for the day

**Pro Tip:** Save context every 50-75 messages as a safety habit.

### How to Save Context

**Option 1: Manual Save (Always Works)**

1. Create a new file in this directory: `YYYY-MM-DD-HHMM-[topic].md`
   - Example: `2025-11-12-1430-user-authentication-module.md`

2. Use the template below (copy and fill in):

```markdown
# Context Snapshot: [Brief Description]

**Date:** YYYY-MM-DD HH:MM
**Project:** dominic-csharp-app (or fulltime-analytics)
**Session Duration:** X hours / Y messages

---

## Current Task

**What I'm working on:**
[e.g., "Adding user authentication module to C# application"]

**Goal:**
[e.g., "Implement JWT-based authentication with role-based access control"]

**Progress so far:**
- [x] Created User and Role models
- [x] Set up database tables
- [ ] Implement JWT token generation
- [ ] Add authentication middleware
- [ ] Write unit tests

---

## Code in Progress

**Files being modified:**
- `Models/User.cs` - User entity model
- `Models/Role.cs` - Role entity model
- `Services/AuthService.cs` - Authentication logic (IN PROGRESS)
- `Controllers/AuthController.cs` - API endpoints (NOT STARTED)

**Current code snippet (important context):**

```csharp
// AuthService.cs - Current implementation
public class AuthService
{
    private readonly IConfiguration _config;
    private readonly UserDbContext _context;

    public AuthService(IConfiguration config, UserDbContext context)
    {
        _config = config;
        _context = context;
    }

    // TODO: Implement GenerateJwtToken method
    // TODO: Implement ValidateCredentials method
}
```

**Next code to write:**
```csharp
// Need to implement JWT token generation with these claims:
// - User ID
// - Username
// - Roles
// - Expiration: 60 minutes
```

---

## Recent Decisions Made

1. **Decision:** Use JWT tokens instead of session-based auth
   - **Reason:** Stateless, better for scaling, works with mobile apps

2. **Decision:** Store passwords using BCrypt hashing
   - **Reason:** Industry standard, secure, built-in salt

3. **Decision:** Roles stored in separate table with many-to-many relationship
   - **Reason:** Flexible, allows users to have multiple roles

---

## Patterns Being Applied

- C# naming convention: PascalCase for public members
- Dependency injection for services
- Repository pattern for data access
- Async/await for database operations

---

## Current Challenges / Blockers

1. **Challenge:** JWT secret key configuration
   - **Status:** Decided to store in appsettings.json (not hardcoded)
   - **Next step:** Create configuration section

2. **Question:** Token expiration time for mobile vs web?
   - **Tentative answer:** 60 min for both, use refresh tokens later

---

## Next Steps (In Order)

1. Implement `GenerateJwtToken()` method
2. Implement `ValidateCredentials()` method
3. Create `AuthController` with Login and Register endpoints
4. Add authentication middleware to startup
5. Test with Postman
6. Write unit tests

---

## References / Links

- JWT documentation: https://learn.microsoft.com/en-us/aspnet/core/security/authentication/
- BCrypt library: BCrypt.Net-Next NuGet package
- Similar implementation: [link to example if you found one]

---

## Context for AI (What AI should know)

**Architecture context:**
- .NET 6 / .NET 7 application
- Using Entity Framework Core
- SQL Server / PostgreSQL database
- RESTful API architecture
- Following repository pattern

**Code style preferences:**
- XML documentation comments for public methods
- Async/await for all database operations
- Meaningful variable names
- Single Responsibility Principle

**What we've discussed so far:**
[Summarize key points from conversation:
- Discussed authentication approaches
- Decided on JWT over sessions
- Reviewed security best practices
- Planned database schema
]

---

## Resume Command

**To restore this context in a new conversation, say:**

"Please restore my context from the snapshot: context-snapshots/2025-11-12-1430-user-authentication-module.md

Quick summary:
- Working on JWT authentication for C# app
- Created User/Role models, DB tables done
- Need to implement AuthService with JWT generation
- Next: GenerateJwtToken() and ValidateCredentials() methods"

---

## Notes

[Any other important context, gotchas, or reminders]
```

**Option 2: Future `/context-save` Command (Coming Soon)**
```bash
/context-save user-authentication-module
# Will automatically create snapshot with all relevant context
```

### How to Restore Context

**In your new conversation:**

1. Say: "Please read context-snapshots/[your-file].md and restore that context"

2. Claude will:
   - Read the snapshot file
   - Understand where you left off
   - Know what you're working on
   - Recall all decisions made
   - Resume exactly where you left off

3. Verify by asking: "What am I currently working on?"

**Example:**
```
You: "Please read context-snapshots/2025-11-12-1430-user-authentication-module.md and restore my context"

Claude: "I've read your context snapshot. You're implementing JWT-based authentication
for your C# application. You've completed the User and Role models and database tables.
Your next task is to implement the GenerateJwtToken() and ValidateCredentials() methods
in the AuthService class. You decided to use 60-minute token expiration and BCrypt for
password hashing. Ready to continue with the JWT token generation?"

You: "Perfect! Let's implement GenerateJwtToken()"
```

---

## Best Practices

### When to Create Snapshots

**High Priority (Always save):**
- Multi-hour coding sessions
- Complex features spanning multiple conversations
- Important architectural decisions in progress
- Before taking a break when work is unfinished

**Medium Priority (Consider saving):**
- Debugging session with multiple theories tested
- Code reviews with many issues to address
- Database schema changes in progress

**Low Priority (Optional):**
- Simple bug fixes
- One-off questions
- Quick data queries

### Naming Convention

Use this format: `YYYY-MM-DD-HHMM-brief-topic.md`

**Examples:**
- `2025-11-12-1430-user-authentication.md`
- `2025-11-12-0900-database-migration.md`
- `2025-11-13-1600-payment-integration.md`
- `2025-11-14-1000-debugging-null-reference.md`

### What to Include

**Essential:**
- Current task and goal
- Code in progress (actual snippets)
- Recent decisions made
- Next steps (ordered list)

**Important:**
- Files being modified
- Patterns being applied
- Current challenges
- Architecture context

**Nice to Have:**
- References/links used
- Questions to resolve later
- Alternative approaches considered

### What NOT to Include

- ❌ Sensitive data (passwords, API keys, secrets)
- ❌ Personal identifying information
- ❌ Proprietary business logic (if under NDA)
- ❌ Complete file contents (use snippets instead)
- ❌ Unrelated topics

---

## Template Files

### Quick Template (Minimum Viable Snapshot)

```markdown
# Context: [Topic]
Date: [Date]
Project: dominic-csharp-app

## What I'm doing:
[Brief description]

## Progress:
- [x] Done
- [ ] Doing
- [ ] To do

## Code:
[Key snippet]

## Next:
1. [Step 1]
2. [Step 2]
```

### Full Template (Comprehensive)

See the complete template above in the "How to Save Context" section.

---

## Tips for Maximum Effectiveness

1. **Save Early, Save Often**
   - Don't wait until you hit the conversation limit
   - Save at natural breakpoints (feature complete, tests written, etc.)

2. **Be Specific in "Next Steps"**
   - Vague: "Finish authentication"
   - Specific: "Implement GenerateJwtToken() method with these claims: userId, username, roles. Use 60-min expiration."

3. **Include Code Snippets**
   - Context is easier to restore with actual code
   - Include the exact state of work-in-progress code
   - Show what's done vs. what's next

4. **Document Decisions**
   - Why did you choose approach A over B?
   - This saves re-deliberation time

5. **Update Patterns**
   - After restoring context and completing work, run `/learn`
   - Patterns extracted will help in future sessions

---

## Measuring Success

**Before using context snapshots:**
- Lost 30-60 minutes re-explaining context
- Forgot decisions made in previous conversation
- Repeated the same deliberations
- Frustration level: HIGH

**After using context snapshots:**
- Restore context in < 2 minutes
- Zero information loss
- Continue immediately where you left off
- Frustration level: ZERO

**Track your savings:**
- Each snapshot that successfully restores context = ~45 minutes saved
- If you use this 3x per week = 2.25 hours saved/week
- Over a month = 9 hours saved
- **That's 45% of your 20 hr/week time savings!**

---

## Future Enhancements (Phase 2)

- [ ] `/context-save` command - automatic snapshot creation
- [ ] `/context-restore [snapshot]` command - automatic restoration
- [ ] `/context-list` - show all snapshots for current project
- [ ] Auto-detect approaching conversation limit and prompt to save
- [ ] Git integration - link snapshots to commits
- [ ] Diff view - show what changed between snapshots

---

## Examples

See `examples/` directory for real snapshot examples:
- `examples/2025-11-12-authentication-example.md` - Complete auth module context
- `examples/2025-11-12-debugging-example.md` - Debugging session context
- `examples/2025-11-12-quick-example.md` - Quick template example

---

## Questions?

**Q: How long does it take to create a snapshot?**
A: 3-5 minutes for a comprehensive one, 1 minute for a quick one. Worth it to save 45 minutes later!

**Q: What if I forget to save context?**
A: Try to remember key points and create a snapshot from memory. Better than nothing! But build the habit of saving before hitting limits.

**Q: Can I have multiple snapshots for the same feature?**
A: Yes! Use timestamps in filenames. Helpful for tracking evolution of complex features.

**Q: Should I delete old snapshots?**
A: Keep them for at least a month. They're small text files and useful for reference. Archive to `context-snapshots/archive/` after feature is complete.

---

**Remember: 2 minutes to save context = 45 minutes saved later. Always worth it!**
