# Development Patterns

**Purpose:** Track reusable development patterns learned from experience

**Confidence levels:**
- **HIGH** (5+ examples): Battle-tested, apply automatically
- **MEDIUM** (2-4 examples): Promising pattern, apply with consideration
- **LOW** (1 example): Early observation, validate before applying

---

## Pattern Template

```markdown
## [Pattern Name]

**Confidence:** HIGH/MEDIUM/LOW
**Based on:** X examples
**Last reinforced:** YYYY-MM-DD
**Time saved per use:** ~X minutes

**Context:**
[When does this pattern apply?]

**Pattern:**
[What's the reusable approach?]

**Why it works:**
[Why is this effective?]

**Examples:**
1. [Project/Task where applied] - [Date] - [Outcome]
2. [Project/Task where applied] - [Date] - [Outcome]
3. ...

**Code example:**
[If applicable, show before/after or implementation]

**Related patterns:**
- [Link to related pattern]

**Source experiences:**
- memory/episodic/code/YYYY-MM-DD-description.md
```

---

## Example Patterns

Below are example patterns to demonstrate structure. Replace with Luca's actual learned patterns.

---

## JWT Refresh Tokens for Mobile Apps

**Confidence:** MEDIUM
**Based on:** 2 examples
**Last reinforced:** 2025-01-09
**Time saved per use:** ~30 minutes (prevents debugging sessions)

**Context:**
When implementing authentication for mobile applications (iOS, Android, React Native)

**Pattern:**
Mobile apps require longer JWT refresh token expiry times than web apps.
- Web: 5-10 min access token, 1 day refresh token
- Mobile: 15-20 min access token, 7 day refresh token

**Why it works:**
- Mobile apps go to background frequently (network interrupts)
- Users expect "stay logged in" behavior
- Token refresh can fail due to network conditions
- Longer expiry provides better UX without security compromise

**Examples:**
1. Client A - OAuth implementation (2024-11) - Fixed login issues for mobile users
2. Internal Product - Mobile app auth (2025-01) - Prevented similar issue proactively

**Code example:**
```python
# settings.py
if client_type == 'mobile':
    ACCESS_TOKEN_EXPIRY = timedelta(minutes=15)
    REFRESH_TOKEN_EXPIRY = timedelta(days=7)
else:  # web
    ACCESS_TOKEN_EXPIRY = timedelta(minutes=5)
    REFRESH_TOKEN_EXPIRY = timedelta(days=1)
```

**Related patterns:**
- Error handling with user context logging

**Source experiences:**
- memory/episodic/code/2024-11-12-oauth-mobile-fix.md
- memory/episodic/code/2025-01-09-auth-implementation.md

---

## Database Transaction Safety for Critical Operations

**Confidence:** LOW
**Based on:** 1 example
**Last reinforced:** 2025-01-10
**Time saved per use:** ~15 minutes (prevents orphaned data issues)

**Context:**
When performing multi-step database operations that must succeed or fail together (payments, user creation with related data, status updates)

**Pattern:**
Wrap critical multi-step database operations in atomic transactions using `@transaction.atomic` (Django) or equivalent.

**Why it works:**
- Prevents partial updates (data consistency)
- Automatic rollback on errors
- Eliminates need for manual cleanup code
- Reduces debugging time for orphaned records

**Examples:**
1. Payment processing (2025-01) - Code review feedback, not yet implemented in production

**Code example:**
```python
from django.db import transaction

@transaction.atomic
def process_payment(user_id, amount):
    try:
        user = User.objects.get(id=user_id)
        payment = Payment.objects.create(
            user=user,
            amount=amount,
            status='pending'
        )
        # ... process payment logic ...
        payment.status = 'completed'
        payment.save()
        return payment
    except Exception as e:
        # Automatic rollback - no orphaned 'pending' payments
        logger.error(f"Payment failed: {e}")
        raise
```

**Related patterns:**
- Error handling with try/except
- Audit logging

**Source experiences:**
- memory/episodic/code/2025-01-10-payment-code-review.md

---

## Error Logging with User Context

**Confidence:** HIGH
**Based on:** 8 examples
**Last reinforced:** 2025-01-08
**Time saved per use:** ~20 minutes (faster debugging)

**Context:**
When logging errors or important events in production applications

**Pattern:**
Always include structured context in logs:
- `user_id` (who experienced the error)
- `request_id` (trace the full request)
- `action` (what they were trying to do)
- `extra_data` (relevant state/parameters)

**Why it works:**
- Enables filtering logs by user (reproduce issues)
- Connects errors across services (request_id)
- Provides debugging context without digging
- Faster resolution = happier clients

**Examples:**
1. Client A - Error tracking setup (2024-09)
2. Client B - Production debugging (2024-10)
3. Internal Product - Logging infrastructure (2024-11)
4. Client C - Bug investigation (2024-12)
5. Client A - Performance monitoring (2024-12)
6. Internal Product - Feature rollout (2025-01)
7. Client B - Data migration (2025-01)
8. Client D - API integration (2025-01)

**Code example:**
```python
import logging

logger = logging.getLogger(__name__)

def process_order(user_id, order_id):
    try:
        # ... processing logic ...
        logger.info(
            "Order processed successfully",
            extra={
                "user_id": user_id,
                "order_id": order_id,
                "request_id": get_request_id(),
                "action": "process_order",
                "amount": order.total_amount
            }
        )
    except Exception as e:
        logger.error(
            "Order processing failed",
            extra={
                "user_id": user_id,
                "order_id": order_id,
                "request_id": get_request_id(),
                "action": "process_order",
                "error": str(e)
            },
            exc_info=True
        )
        raise
```

**Related patterns:**
- JWT tokens for mobile
- Database transactions

**Source experiences:**
- memory/episodic/code/2024-09-15-logging-setup.md
- memory/episodic/code/2024-10-22-production-debugging.md
- memory/episodic/code/2024-11-05-monitoring-infrastructure.md
- [... and 5 more ...]

---

## N+1 Query Prevention

**Confidence:** MEDIUM
**Based on:** 4 examples
**Last reinforced:** 2024-12-20
**Time saved per use:** ~45 minutes (debugging + optimization)

**Context:**
When querying related database objects in ORMs (Django, SQLAlchemy)

**Pattern:**
Use `select_related()` for foreign key relationships and `prefetch_related()` for many-to-many or reverse foreign keys to avoid N+1 queries.

**Why it works:**
- Single query instead of N+1 queries
- Reduces database load significantly
- Faster API response times (100ms → 10ms in some cases)
- Prevents performance degradation as data grows

**Examples:**
1. Client A - Product listing page (2024-08) - 500ms → 50ms
2. Internal Product - Dashboard queries (2024-10) - 2s → 200ms
3. Client B - User profile API (2024-11) - 300ms → 30ms
4. Client C - Report generation (2024-12) - 5s → 500ms

**Code example:**
```python
# ❌ Bad: N+1 queries (1 + N)
users = User.objects.all()
for user in users:
    print(user.profile.bio)  # New query for EACH user!

# ✅ Good: 2 queries total
users = User.objects.select_related('profile').all()
for user in users:
    print(user.profile.bio)  # No additional queries

# ❌ Bad: N+1 queries for many-to-many
posts = Post.objects.all()
for post in posts:
    print(post.tags.all())  # New query for EACH post!

# ✅ Good: 2 queries total
posts = Post.objects.prefetch_related('tags').all()
for post in posts:
    print(post.tags.all())  # No additional queries
```

**Detection:**
```python
# In development, enable query logging
from django.conf import settings
settings.DEBUG = True

# Or use django-debug-toolbar
# Or check query count:
from django.db import connection
print(len(connection.queries))  # Should be minimal
```

**Related patterns:**
- Database indexing
- Caching strategies

**Source experiences:**
- memory/episodic/code/2024-08-14-performance-optimization.md
- memory/episodic/code/2024-10-03-dashboard-queries.md
- memory/episodic/code/2024-11-18-api-performance.md
- memory/episodic/code/2024-12-20-report-optimization.md

---

## PostgreSQL Index on Foreign Keys

**Confidence:** HIGH
**Based on:** 6 examples
**Last reinforced:** 2025-01-05
**Time saved per use:** ~10 minutes (prevents slow queries)

**Context:**
When creating database models with foreign key relationships in PostgreSQL

**Pattern:**
Always create indexes on foreign key columns. PostgreSQL does NOT auto-index foreign keys (unlike some other databases).

**Why it works:**
- Joins are common (almost every query)
- Foreign keys are used in WHERE clauses
- Index dramatically speeds up lookups (1000ms → 10ms)
- Prevents slow queries as data grows

**Examples:**
1. Client A - Order system (2024-07) - Forgot index, added after slowness
2. Client B - User relations (2024-08) - Added proactively
3. Internal Product - Schema design (2024-09) - Standard practice now
4. Client C - Analytics (2024-10) - Migration added indexes
5. Client D - Multi-tenant (2024-11) - tenant_id indexed
6. Client A - Notifications (2025-01) - user_id indexed

**Code example:**
```python
# Django models
class Order(models.Model):
    user = models.ForeignKey(User, on_delete=models.CASCADE, db_index=True)  # ✅ Indexed
    product = models.ForeignKey(Product, on_delete=models.CASCADE, db_index=True)  # ✅ Indexed
    created_at = models.DateTimeField(auto_now_add=True, db_index=True)  # ✅ Often filtered

# Migration to add index to existing column
class Migration(migrations.Migration):
    operations = [
        migrations.AddIndex(
            model_name='order',
            index=models.Index(fields=['user'], name='order_user_idx'),
        ),
    ]

# Raw SQL
CREATE INDEX order_user_idx ON orders(user_id);
CREATE INDEX order_product_idx ON orders(product_id);
```

**Check existing indexes:**
```sql
-- PostgreSQL: List indexes on table
\d+ orders

-- Or query directly
SELECT indexname, indexdef FROM pg_indexes WHERE tablename = 'orders';
```

**Related patterns:**
- N+1 query prevention
- Query optimization

**Source experiences:**
- memory/episodic/code/2024-07-22-slow-queries-fix.md
- memory/episodic/code/2024-08-30-db-schema-design.md
- [... and 4 more ...]

---

## API Error Response Structure

**Confidence:** MEDIUM
**Based on:** 3 examples
**Last reinforced:** 2024-12-15
**Time saved per use:** ~20 minutes (consistent error handling)

**Context:**
When designing API error responses for client applications

**Pattern:**
Use consistent error response structure across all endpoints:
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "User-friendly error message",
    "details": {
      "field": "email",
      "reason": "Email already exists"
    },
    "request_id": "req_abc123"
  }
}
```

**Why it works:**
- Frontend can handle errors consistently
- `code` enables programmatic handling (show specific UI)
- `message` can be displayed directly to users
- `details` provides debugging context
- `request_id` enables support to trace issues

**Examples:**
1. Client A - API redesign (2024-09) - Reduced frontend error handling code by 40%
2. Internal Product - API v2 (2024-11) - Built-in from start
3. Client B - Mobile API (2024-12) - Improved error UX significantly

**Code example:**
```python
# FastAPI error handler
from fastapi import FastAPI, HTTPException, Request
from fastapi.responses import JSONResponse

app = FastAPI()

class APIError(Exception):
    def __init__(self, code: str, message: str, details: dict = None, status_code: int = 400):
        self.code = code
        self.message = message
        self.details = details or {}
        self.status_code = status_code

@app.exception_handler(APIError)
async def api_error_handler(request: Request, exc: APIError):
    return JSONResponse(
        status_code=exc.status_code,
        content={
            "error": {
                "code": exc.code,
                "message": exc.message,
                "details": exc.details,
                "request_id": request.state.request_id
            }
        }
    )

# Usage
@app.post("/users")
def create_user(email: str):
    if User.objects.filter(email=email).exists():
        raise APIError(
            code="DUPLICATE_EMAIL",
            message="An account with this email already exists",
            details={"field": "email", "value": email},
            status_code=409
        )
```

**Related patterns:**
- Error logging with context
- Frontend error handling

**Source experiences:**
- memory/episodic/code/2024-09-10-api-error-redesign.md
- memory/episodic/code/2024-11-25-api-v2-launch.md
- memory/episodic/code/2024-12-15-mobile-api-errors.md

---

## Proactive JWT Token Refresh

**Confidence:** LOW
**Based on:** 1 example
**Last reinforced:** 2025-11-11
**Time saved per use:** ~30 minutes (prevents user frustration debugging)

**Context:**
When implementing JWT-based authentication systems where users stay logged in for extended periods

**Pattern:**
Implement proactive token refresh logic that renews tokens 5-10 minutes before expiration, rather than waiting for the token to expire and forcing re-authentication.

**Why it works:**
- Better UX - users never experience forced logout
- Prevents session interruption during active use
- Reduces support tickets for "random logouts"
- Background refresh is invisible to users
- Security maintained with short-lived access tokens

**Examples:**
1. test-ecommerce project (2025-11-11) - Fixed JWT expiration bug, implemented proactive refresh

**Code example:**
```javascript
// Frontend: React/TypeScript
import { useEffect } from 'react';

function useTokenRefresh() {
  useEffect(() => {
    const checkAndRefreshToken = async () => {
      const token = getAccessToken();
      const decoded = jwtDecode(token);
      const expiresAt = decoded.exp * 1000; // Convert to milliseconds
      const now = Date.now();
      const timeUntilExpiry = expiresAt - now;

      // Refresh 5 minutes before expiration
      if (timeUntilExpiry < 5 * 60 * 1000 && timeUntilExpiry > 0) {
        await refreshAccessToken();
      }
    };

    // Check every minute
    const interval = setInterval(checkAndRefreshToken, 60 * 1000);
    checkAndRefreshToken(); // Check immediately on mount

    return () => clearInterval(interval);
  }, []);
}

// Backend: Node.js/Express
app.post('/auth/refresh', async (req, res) => {
  const { refreshToken } = req.body;

  try {
    const decoded = jwt.verify(refreshToken, REFRESH_TOKEN_SECRET);
    const newAccessToken = jwt.sign(
      { userId: decoded.userId },
      ACCESS_TOKEN_SECRET,
      { expiresIn: '1h' } // Short-lived access token
    );

    res.json({ accessToken: newAccessToken });
  } catch (error) {
    res.status(401).json({ error: 'Invalid refresh token' });
  }
});
```

**Related patterns:**
- JWT Refresh Tokens for Mobile Apps (MEDIUM confidence)
- Error handling with user context

**Source experiences:**
- memory/episodic/code/2025-11-11-jwt-token-refresh-fix.md

---

## Pattern Growth Tracking

**How patterns mature:**

1. **First encounter** (LOW confidence)
   - Experience captured in episodic memory
   - Potential pattern identified
   - Added to dev-patterns.md with LOW confidence

2. **Second encounter** (MEDIUM confidence)
   - Similar situation arises
   - Pattern applied consciously
   - Confidence upgraded to MEDIUM
   - Time savings noted

3. **Repeated success** (HIGH confidence, 5+ examples)
   - Pattern applied automatically
   - Becomes second nature
   - Confidence upgraded to HIGH
   - Significant time savings accumulated

4. **Workflow creation** (Procedural memory)
   - HIGH confidence patterns become workflows
   - Codified in memory/procedural/workflows/
   - Checklists and templates created
   - Onboarding material for team

**Example evolution:**

Week 1: Notice JWT token expiry issue (LOW confidence)
Week 3: Similar issue in another project, apply pattern (MEDIUM confidence)
Week 8: Applied successfully 5+ times (HIGH confidence)
Month 3: Create "Auth Implementation Checklist" workflow (Procedural memory)

---

## How to Use This File

**When starting a development task:**
1. Check relevant HIGH confidence patterns (apply automatically)
2. Review MEDIUM confidence patterns (consider if applicable)
3. Reference code examples (copy-paste starting points)

**After completing a development task:**
1. Run `/learn` to extract new patterns
2. Update existing patterns with new examples
3. Increase confidence if pattern proven again
4. Track time saved

**When code reviewing:**
1. Check if HIGH confidence patterns were followed
2. Suggest patterns if missing
3. Validate LOW/MEDIUM patterns (do they still apply?)

---

**Last Updated:** 2025-01-11
**Total Patterns:** 7
**Confidence Breakdown:**
- HIGH: 3 patterns (43%)
- MEDIUM: 3 patterns (43%)
- LOW: 1 pattern (14%)

**Next steps:**
- Validate "Database Transaction Safety" pattern (needs more examples)
- Consider creating "Auth Implementation" workflow (JWT pattern is HIGH confidence)
- Review API Error Response pattern after 2 more uses
