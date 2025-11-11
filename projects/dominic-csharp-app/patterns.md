# C# Development Patterns - Dominic's Project

**Project:** dominic-csharp-app
**Last Updated:** 2025-11-11

**Purpose:** Document C#-specific patterns discovered while building this project. Patterns help speed up development by applying proven solutions.

---

## How to Use This File

### Confidence Levels

**HIGH (5+ applications):** Apply automatically without thinking
**MEDIUM (2-4 applications):** Apply with consideration
**LOW (1 application):** Validate before using

### Pattern Structure

Each pattern includes:
- **Description:** What the pattern does
- **When to use:** Situations where this applies
- **Implementation:** Code example
- **Benefits:** Why this helps
- **Confidence:** How proven this is
- **Applied:** Where you've used it

---

## C# Language Patterns

### Pattern: Async/Await for I/O Operations

**Confidence:** HIGH (assumed best practice)

**When to use:**
- Database operations
- File I/O
- HTTP requests
- Any operation that waits for external resources

**Implementation:**
```csharp
// Good - async all the way
public async Task<User> GetUserAsync(int userId)
{
    return await _context.Users
        .FirstOrDefaultAsync(u => u.Id == userId);
}

// Bad - blocking call
public User GetUser(int userId)
{
    return _context.Users
        .FirstOrDefault(u => u.Id == userId);
}
```

**Benefits:**
- Better scalability
- Non-blocking I/O
- Improved responsiveness

**Applied:**
- [To be filled in as you use this pattern]

---

### Pattern: XML Documentation Comments

**Confidence:** HIGH (project standard)

**When to use:**
- All public methods
- All public properties
- All public classes

**Implementation:**
```csharp
/// <summary>
/// Retrieves a user by their unique identifier.
/// </summary>
/// <param name="userId">The unique identifier of the user.</param>
/// <returns>The user if found; otherwise, null.</returns>
/// <exception cref="ArgumentException">Thrown when userId is less than or equal to zero.</exception>
public async Task<User> GetUserAsync(int userId)
{
    if (userId <= 0)
        throw new ArgumentException("User ID must be positive.", nameof(userId));

    return await _context.Users.FirstOrDefaultAsync(u => u.Id == userId);
}
```

**Benefits:**
- IntelliSense documentation
- Easier maintenance
- Better code understanding

**Applied:**
- [To be filled in]

---

### Pattern: Null-Conditional Operators

**Confidence:** MEDIUM (C# best practice)

**When to use:**
- Accessing properties on potentially null objects
- Avoiding null reference exceptions

**Implementation:**
```csharp
// Good - null-conditional operator
var email = user?.Email?.ToLower();

// Bad - manual null checking
string email = null;
if (user != null && user.Email != null)
{
    email = user.Email.ToLower();
}
```

**Benefits:**
- Cleaner code
- Fewer null reference exceptions
- More readable

**Applied:**
- [To be filled in]

---

## Data Access Patterns

### Pattern: Repository Pattern with Entity Framework

**Confidence:** [To be determined]

**When to use:**
- Abstracting data access logic
- Enabling unit testing
- Swapping data sources

**Implementation:**
```csharp
public interface IUserRepository
{
    Task<User> GetByIdAsync(int id);
    Task<IEnumerable<User>> GetAllAsync();
    Task AddAsync(User user);
    Task UpdateAsync(User user);
    Task DeleteAsync(int id);
}

public class UserRepository : IUserRepository
{
    private readonly AppDbContext _context;

    public UserRepository(AppDbContext context)
    {
        _context = context;
    }

    public async Task<User> GetByIdAsync(int id)
    {
        return await _context.Users.FindAsync(id);
    }

    // ... other methods
}
```

**Benefits:**
- Testable code
- Separation of concerns
- Flexible data access

**Applied:**
- [To be filled in]

---

### Pattern: Include Related Data (Eager Loading)

**Confidence:** [To be determined]

**When to use:**
- Need related data in single query
- Avoiding N+1 query problem

**Implementation:**
```csharp
// Good - eager loading with Include
var users = await _context.Users
    .Include(u => u.Roles)
    .Include(u => u.Profile)
    .ToListAsync();

// Bad - lazy loading (N+1 queries)
var users = await _context.Users.ToListAsync();
// Later accessing user.Roles causes additional queries for each user
```

**Benefits:**
- Fewer database queries
- Better performance
- Explicit data loading

**Applied:**
- [To be filled in]

---

## Error Handling Patterns

### Pattern: Custom Exception Types

**Confidence:** [To be determined]

**When to use:**
- Domain-specific errors
- Need to catch specific error types
- Better error messages

**Implementation:**
```csharp
public class UserNotFoundException : Exception
{
    public int UserId { get; }

    public UserNotFoundException(int userId)
        : base($"User with ID {userId} was not found.")
    {
        UserId = userId;
    }
}

// Usage
public async Task<User> GetUserAsync(int userId)
{
    var user = await _context.Users.FindAsync(userId);

    if (user == null)
        throw new UserNotFoundException(userId);

    return user;
}
```

**Benefits:**
- Type-safe error handling
- Better error messages
- Easier to handle specific errors

**Applied:**
- [To be filled in]

---

### Pattern: Try-Catch with Logging

**Confidence:** [To be determined]

**When to use:**
- Operations that might fail
- Need to log errors
- Graceful error handling

**Implementation:**
```csharp
public async Task<ActionResult<User>> GetUser(int id)
{
    try
    {
        var user = await _userService.GetUserAsync(id);
        return Ok(user);
    }
    catch (UserNotFoundException ex)
    {
        _logger.LogWarning(ex, "User {UserId} not found", id);
        return NotFound(new { message = ex.Message });
    }
    catch (Exception ex)
    {
        _logger.LogError(ex, "Error retrieving user {UserId}", id);
        return StatusCode(500, new { message = "An error occurred" });
    }
}
```

**Benefits:**
- Logged errors for debugging
- Graceful error responses
- Don't expose internal errors to users

**Applied:**
- [To be filled in]

---

## Dependency Injection Patterns

### Pattern: Constructor Injection

**Confidence:** HIGH (ASP.NET Core standard)

**When to use:**
- All service dependencies
- Repository dependencies
- Configuration dependencies

**Implementation:**
```csharp
public class UserService : IUserService
{
    private readonly IUserRepository _userRepository;
    private readonly ILogger<UserService> _logger;
    private readonly IConfiguration _config;

    public UserService(
        IUserRepository userRepository,
        ILogger<UserService> logger,
        IConfiguration config)
    {
        _userRepository = userRepository;
        _logger = logger;
        _config = config;
    }

    // Methods use injected dependencies
}

// In Startup.cs or Program.cs
services.AddScoped<IUserService, UserService>();
services.AddScoped<IUserRepository, UserRepository>();
```

**Benefits:**
- Testable code (easy to mock dependencies)
- Loose coupling
- Explicit dependencies

**Applied:**
- [To be filled in]

---

## Validation Patterns

### Pattern: FluentValidation (if using)

**Confidence:** [To be determined]

**When to use:**
- Validating DTOs
- Complex validation rules
- Reusable validation logic

**Implementation:**
```csharp
public class CreateUserValidator : AbstractValidator<CreateUserDto>
{
    public CreateUserValidator()
    {
        RuleFor(x => x.Email)
            .NotEmpty()
            .EmailAddress();

        RuleFor(x => x.Password)
            .NotEmpty()
            .MinimumLength(8)
            .Matches(@"[A-Z]").WithMessage("Password must contain uppercase")
            .Matches(@"[a-z]").WithMessage("Password must contain lowercase")
            .Matches(@"[0-9]").WithMessage("Password must contain number");

        RuleFor(x => x.Username)
            .NotEmpty()
            .MinimumLength(3)
            .MaximumLength(50);
    }
}
```

**Benefits:**
- Centralized validation logic
- Reusable validators
- Clear validation rules

**Applied:**
- [To be filled in]

---

## Testing Patterns

### Pattern: Arrange-Act-Assert (AAA)

**Confidence:** HIGH (universal testing pattern)

**When to use:**
- All unit tests
- Integration tests

**Implementation:**
```csharp
[Fact]
public async Task GetUserAsync_WithValidId_ReturnsUser()
{
    // Arrange
    var userId = 1;
    var expectedUser = new User { Id = userId, Name = "John" };
    var mockRepo = new Mock<IUserRepository>();
    mockRepo.Setup(r => r.GetByIdAsync(userId))
        .ReturnsAsync(expectedUser);
    var service = new UserService(mockRepo.Object);

    // Act
    var result = await service.GetUserAsync(userId);

    // Assert
    Assert.NotNull(result);
    Assert.Equal(expectedUser.Id, result.Id);
    Assert.Equal(expectedUser.Name, result.Name);
}
```

**Benefits:**
- Clear test structure
- Easy to read
- Maintainable tests

**Applied:**
- [To be filled in]

---

## Project-Specific Patterns

[Add patterns specific to your project as you discover them]

### Pattern: [Your Custom Pattern]

**Confidence:** LOW

**When to use:**
- [Description]

**Implementation:**
```csharp
// Your code example
```

**Benefits:**
- [Benefits]

**Applied:**
- [Where used]

---

## Anti-Patterns to Avoid

### Anti-Pattern: Using .Result on Async Methods

**Why it's bad:**
- Can cause deadlocks
- Blocks thread (defeats purpose of async)

**Instead, use:**
```csharp
// Bad
var user = _userService.GetUserAsync(id).Result;

// Good
var user = await _userService.GetUserAsync(id);
```

---

### Anti-Pattern: Catching and Swallowing Exceptions

**Why it's bad:**
- Hides errors
- Makes debugging difficult

**Instead, use:**
```csharp
// Bad
try
{
    var user = await GetUserAsync(id);
}
catch { } // Swallows exception!

// Good
try
{
    var user = await GetUserAsync(id);
}
catch (Exception ex)
{
    _logger.LogError(ex, "Error getting user");
    throw; // Re-throw or handle appropriately
}
```

---

## Pattern Discovery Process

When you discover a new pattern:

1. **Document it immediately** (while fresh in mind)
2. **Mark confidence as LOW** (first use)
3. **Add to this file** with code example
4. **Use `/learn` command** after completing work
5. **Update confidence** after repeated successful use:
   - 2nd use → Still LOW
   - 3rd-4th use → Promote to MEDIUM
   - 5th+ use → Promote to HIGH

---

## Pattern Application Checklist

Before applying a pattern:
- [ ] Does the pattern fit this situation?
- [ ] Have I used it successfully before?
- [ ] Do I understand why it works?
- [ ] Are there any gotchas to watch for?
- [ ] Is it appropriate for my skill level?

---

## Notes

- Patterns will be added as you discover them through development
- Use `/learn` after completing features to extract patterns
- Review this file before starting new features
- HIGH confidence patterns should be applied automatically
- MEDIUM/LOW patterns require consideration

**Remember:** Patterns are meant to speed you up, not slow you down. If a pattern feels like overkill for your situation, it might be.
