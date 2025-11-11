# Architecture Decisions & Tech Stack - C# Focus

**Purpose:** Document your technical choices, C# tech stack, and architectural patterns for your side business project

---

## Current Tech Stack

### Core Development
- **Language:** C# [e.g., C# 10, C# 11]
- **Framework:** [e.g., .NET 6, .NET 7, .NET Framework 4.8]
- **IDE:** VS Code / VS Community
- **Platform:** Windows 10
- **Build Tools:** MSBuild, dotnet CLI

### Backend
- **Web Framework:** [e.g., ASP.NET Core MVC, ASP.NET Web API, Minimal APIs]
- **ORM:** [e.g., Entity Framework Core 6.0, Dapper]
- **Database:** [e.g., SQL Server 2019, PostgreSQL, SQLite]
- **Authentication:** [e.g., ASP.NET Identity, JWT, OAuth]
- **Logging:** [e.g., Serilog, NLog, Microsoft.Extensions.Logging]

### Frontend (if applicable)
- **UI Technology:** [e.g., Razor Pages, Blazor Server, Blazor WebAssembly, React + WebAPI]
- **CSS Framework:** [e.g., Bootstrap, Tailwind CSS]
- **JavaScript:** [If using - e.g., vanilla JS, jQuery, TypeScript]

### Database & Data Access
- **Database Server:** [e.g., SQL Server, PostgreSQL]
- **Migration Tool:** [e.g., EF Core Migrations, FluentMigrator]
- **Connection Pooling:** [Built-in ADO.NET pooling]
- **Query Performance:** [LINQ, stored procedures]

### Testing
- **Unit Testing:** [e.g., xUnit, NUnit, MSTest]
- **Mocking:** [e.g., Moq, NSubstitute]
- **Integration Testing:** [e.g., WebApplicationFactory]
- **Coverage Tool:** [e.g., Coverlet, dotCover]

### Version Control & DevOps
- **VCS:** Git for Windows (version 2.51.2.windows.1)
- **Repository:** [GitHub, GitLab, Azure DevOps, local]
- **CI/CD:** [e.g., GitHub Actions, Azure Pipelines, GitLab CI - if using]
- **Deployment:** [e.g., IIS, Azure App Service, Docker]

### Dependencies (NuGet Packages)
[To be filled in as you add packages]
- **Entity Framework Core:** [version]
- **AutoMapper:** [if using]
- **FluentValidation:** [if using]
- **Serilog:** [if using for logging]
- [Other packages you're using]

---

## Key Architecture Decisions

### Decision Template

**Date:** [When decided]
**Context:** [Why this decision was needed]
**Decision:** [What was chosen]
**Rationale:**
- [Reason 1]
- [Reason 2]
- [Reason 3]

**Alternatives Considered:**
- [Alternative A]: [Why not chosen]
- [Alternative B]: [Why not chosen]

**Outcome:** [How it's working so far]

---

### Decision 1: [Your First Major Decision]

[To be filled in with your actual architectural decisions]

**Example - Entity Framework Core vs Dapper:**
**Date:** [Your date]
**Context:** Need ORM for data access, balancing performance vs development speed
**Decision:** Use Entity Framework Core
**Rationale:**
- Faster development with LINQ queries
- Built-in change tracking
- Easy database migrations
- Strong typing throughout
- Good enough performance for current needs

**Alternatives Considered:**
- Dapper: Rejected - more performant but requires more boilerplate code, development speed matters more at this stage
- ADO.NET: Rejected - too low-level, would slow development significantly

**Outcome:** [How it's working]

---

## Architectural Patterns (C#-Specific)

### Pattern: Project Structure

**Chosen Architecture:** [e.g., Layered Architecture, Clean Architecture, N-Tier]

**Project Organization:**
```
SolutionName/
├── ProjectName.Domain/          # Business entities, interfaces
│   ├── Entities/                # Domain models (User, Order, etc.)
│   └── Interfaces/              # Repository interfaces
├── ProjectName.Application/     # Business logic, services
│   ├── Services/                # Business services
│   ├── DTOs/                    # Data transfer objects
│   └── Validators/              # FluentValidation validators
├── ProjectName.Infrastructure/  # Data access, external services
│   ├── Data/                    # DbContext, repositories
│   ├── Migrations/              # EF Core migrations
│   └── ExternalServices/        # Third-party integrations
├── ProjectName.Web/             # Presentation layer
│   ├── Controllers/             # MVC/API controllers
│   ├── Views/                   # Razor views (if MVC)
│   ├── Models/                  # View models
│   └── wwwroot/                 # Static files
└── ProjectName.Tests/           # Tests
    ├── UnitTests/               # Unit tests
    └── IntegrationTests/        # Integration tests
```

### Pattern: Dependency Injection

**Standard:** Constructor injection for all dependencies

**Example:**
```csharp
public class UserService : IUserService
{
    private readonly IUserRepository _userRepository;
    private readonly ILogger<UserService> _logger;

    public UserService(
        IUserRepository userRepository,
        ILogger<UserService> logger)
    {
        _userRepository = userRepository;
        _logger = logger;
    }
}

// In Program.cs or Startup.cs
services.AddScoped<IUserService, UserService>();
services.AddScoped<IUserRepository, UserRepository>();
```

### Pattern: Async/Await for I/O

**Standard:** All database operations, file I/O, and HTTP requests use async/await

**Example:**
```csharp
public async Task<User> GetUserByIdAsync(int id)
{
    return await _context.Users.FindAsync(id);
}
```

### Pattern: Repository Pattern

**Usage:** [Decide whether to use repository pattern or DbContext directly]

**If using repositories:**
```csharp
public interface IUserRepository
{
    Task<User> GetByIdAsync(int id);
    Task<IEnumerable<User>> GetAllAsync();
    Task AddAsync(User user);
    Task UpdateAsync(User user);
    Task DeleteAsync(int id);
}
```

### Pattern: API Design (if applicable)

**Convention:**
- RESTful endpoints: `/api/[controller]/[action]`
- HTTP verbs: GET, POST, PUT, DELETE
- Status codes: 200 (OK), 201 (Created), 400 (Bad Request), 404 (Not Found), 500 (Server Error)
- Response format: JSON
- Authentication: [JWT, Cookie-based, etc.]

### Pattern: Error Handling

**Standard:**
- Custom exception types for domain-specific errors
- Global exception handler middleware
- Structured error responses
- Logging with Serilog/NLog

**Example:**
```csharp
public class UserNotFoundException : Exception
{
    public UserNotFoundException(int userId)
        : base($"User with ID {userId} not found.") { }
}

// In controller
try
{
    var user = await _userService.GetUserAsync(id);
    return Ok(user);
}
catch (UserNotFoundException ex)
{
    _logger.LogWarning(ex, "User not found");
    return NotFound(new { message = ex.Message });
}
```

---

## Database Design Patterns

### Pattern: Entity Configuration

**Standard:** Fluent API for entity configuration (preferred over Data Annotations)

**Example:**
```csharp
public class UserConfiguration : IEntityTypeConfiguration<User>
{
    public void Configure(EntityTypeBuilder<User> builder)
    {
        builder.HasKey(u => u.Id);
        builder.Property(u => u.Email).IsRequired().HasMaxLength(255);
        builder.HasIndex(u => u.Email).IsUnique();

        builder.HasMany(u => u.Orders)
               .WithOne(o => o.User)
               .HasForeignKey(o => o.UserId);
    }
}
```

### Pattern: Migration Strategy

**Standard:**
- Code-first migrations with EF Core
- Descriptive migration names: `dotnet ef migrations add AddUserEmailUniqueIndex`
- Review generated SQL before applying
- Always test migrations in development first

### Pattern: Database Indexing

**Standards:**
- Index all foreign keys
- Index frequently queried columns
- Consider composite indexes for common query patterns
- Use `EXPLAIN` / `INCLUDE` to analyze query plans

---

## Code Quality & Standards

### Naming Conventions

**C# Standards:**
- **Classes:** PascalCase (e.g., `UserService`, `OrderRepository`)
- **Interfaces:** IPascalCase (e.g., `IUserService`)
- **Methods:** PascalCase with verb (e.g., `GetUserById`, `CreateOrder`)
- **Properties:** PascalCase (e.g., `UserId`, `EmailAddress`)
- **Private fields:** _camelCase (e.g., `_userRepository`, `_logger`)
- **Local variables:** camelCase (e.g., `userId`, `userName`)
- **Constants:** PascalCase or UPPER_SNAKE_CASE (e.g., `MaxRetryCount` or `MAX_RETRY_COUNT`)

### Documentation Standards

**Requirements:**
- XML documentation comments for all public APIs
- Inline comments for complex logic
- README.md in project root

**Example:**
```csharp
/// <summary>
/// Retrieves a user by their unique identifier.
/// </summary>
/// <param name="userId">The unique identifier of the user.</param>
/// <returns>The user if found; otherwise, null.</returns>
/// <exception cref="ArgumentException">Thrown when userId is less than or equal to zero.</exception>
public async Task<User> GetUserAsync(int userId)
{
    // Implementation
}
```

### Testing Standards

**Goals:**
- 70%+ code coverage on critical business logic
- 100% coverage on data access layer
- Integration tests for API endpoints

**Pattern: AAA (Arrange-Act-Assert)**
```csharp
[Fact]
public async Task GetUserAsync_WithValidId_ReturnsUser()
{
    // Arrange
    var userId = 1;
    var service = CreateServiceWithMocks();

    // Act
    var result = await service.GetUserAsync(userId);

    // Assert
    Assert.NotNull(result);
    Assert.Equal(userId, result.Id);
}
```

---

## Security Patterns

### Authentication & Authorization

**Standards:**
- [Your auth strategy: e.g., ASP.NET Identity, JWT, OAuth]
- Password hashing: [e.g., BCrypt, PBKDF2, built-in Identity hasher]
- Token expiry: [e.g., 60 minutes for access tokens]
- HTTPS only in production

### Data Protection

**Standards:**
- Never store sensitive data in code or config files
- Use User Secrets for development
- Use environment variables or Azure Key Vault for production
- Encrypt sensitive data at rest
- Validate and sanitize all user input

### SQL Injection Prevention

**Standard:** Use parameterized queries or LINQ (EF Core)

**Good:**
```csharp
var user = await _context.Users
    .Where(u => u.Email == email)
    .FirstOrDefaultAsync();
```

**Bad (never do this):**
```csharp
var query = $"SELECT * FROM Users WHERE Email = '{email}'"; // SQL injection risk!
```

---

## Performance Patterns

### Database Query Optimization

**Standards:**
- Use `.AsNoTracking()` for read-only queries
- Eager load related data with `.Include()` to avoid N+1 queries
- Use pagination for large result sets
- Monitor query performance with SQL Profiler

**Example:**
```csharp
// Good - eager loading
var users = await _context.Users
    .Include(u => u.Orders)
    .AsNoTracking()
    .ToListAsync();

// Bad - lazy loading (N+1 queries)
var users = await _context.Users.ToListAsync();
// Accessing user.Orders later causes additional queries
```

### Caching Strategy

[If using caching]
- **Memory Cache:** For frequently accessed, slowly changing data
- **Distributed Cache:** [e.g., Redis] for multi-instance scenarios
- **Cache Duration:** Based on data volatility

### Async Best Practices

**Standards:**
- Use async/await for all I/O operations
- Never use `.Result` or `.Wait()` (can cause deadlocks)
- Use `ConfigureAwait(false)` in library code (not usually needed in ASP.NET Core)

---

## Deployment & DevOps Patterns

### Build & Release

**Process:**
1. Local development & testing
2. Commit to feature branch
3. [CI/CD pipeline - if configured]
4. Deploy to staging
5. Manual testing
6. Deploy to production

### Configuration Management

**Development:**
- Use `appsettings.Development.json`
- User Secrets for sensitive data

**Production:**
- Environment variables
- [Azure App Settings, AWS Parameter Store, or similar]

### Database Migrations

**Process:**
1. Create migration: `dotnet ef migrations add [Name]`
2. Review generated SQL
3. Test in development
4. Apply to staging
5. Backup production database
6. Apply to production

---

## Technology Decisions by Need

[Document specific technology choices and why]

### [Decision Area - e.g., "Validation"]

**Options Considered:**
1. **FluentValidation:** Powerful, flexible, separates validation from models
2. **Data Annotations:** Simpler, built-in, but less flexible
3. **Manual validation:** Most control, but most work

**Decision:** [Your choice]
**Reason:** [Why you chose it]

---

## Lessons Learned

### What's Working Well

[To be filled in as you discover what works]
- [Pattern or tool that's working well]
- [Why it's helping]

### What's Challenging

[To be filled in as you encounter challenges]
- [Challenge you're facing]
- [How you're addressing it]

### What You'd Change

[Future improvements based on experience]
- [What you'd do differently]
- [Why]

---

## Evolution & Updates

**Review Frequency:** Monthly or when making major architectural decisions

**Update Process:**
1. Make architectural decision
2. Document in this file
3. Update relevant patterns
4. Share learnings via `/learn` command

---

**Last Updated:** 2025-11-11
**Maintained By:** Dominic
**Project:** dominic-csharp-app
