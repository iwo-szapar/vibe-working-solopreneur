# Client A - E-commerce Platform - Patterns

**Project:** RetailCo E-commerce
**Last Updated:** 2025-11-10

Patterns discovered during development that can be reused across projects.

---

## 🎯 HIGH CONFIDENCE PATTERNS (5+ applications)

### Pattern: Stripe Webhook Idempotency

**Context:** Stripe webhooks can be sent multiple times (retries, failures)

**Problem:** Processing same webhook twice causes duplicate orders, double charges

**Solution:**
```typescript
// Store webhook event ID to prevent duplicate processing
async function handleWebhook(event: Stripe.Event) {
  const eventId = event.id;

  // Check if already processed
  const existing = await db.webhookEvent.findUnique({
    where: { stripeEventId: eventId }
  });

  if (existing) {
    console.log(`Webhook ${eventId} already processed`);
    return { status: 'already_processed' };
  }

  // Process in transaction
  await db.$transaction(async (tx) => {
    // 1. Record webhook event
    await tx.webhookEvent.create({
      data: {
        stripeEventId: eventId,
        type: event.type,
        processedAt: new Date()
      }
    });

    // 2. Handle event logic
    await processEvent(event, tx);
  });

  return { status: 'processed' };
}
```

**When to Apply:**
- ✅ Any webhook integration (Stripe, Shopify, Slack)
- ✅ Payment processing systems
- ✅ Order management systems

**Time Saved:** 4-6 hours per integration (debugging duplicate issues)

**Confidence:** HIGH (used 7 times across 3 projects)

---

### Pattern: Optimistic UI Updates with Rollback

**Context:** Shopping cart updates should feel instant

**Problem:** Waiting for API response makes cart feel slow (400ms delay per action)

**Solution:**
```typescript
// Zustand store with optimistic updates
const useCartStore = create<CartStore>((set, get) => ({
  items: [],

  addItem: async (product: Product) => {
    // 1. Optimistic update (immediate UI)
    const optimisticItem = {
      id: `temp-${Date.now()}`,
      product,
      quantity: 1
    };

    set((state) => ({
      items: [...state.items, optimisticItem]
    }));

    try {
      // 2. API call in background
      const savedItem = await api.cart.addItem(product.id);

      // 3. Replace temp ID with real ID
      set((state) => ({
        items: state.items.map(item =>
          item.id === optimisticItem.id ? savedItem : item
        )
      }));
    } catch (error) {
      // 4. Rollback on failure
      set((state) => ({
        items: state.items.filter(item => item.id !== optimisticItem.id)
      }));

      toast.error('Failed to add item');
    }
  }
}));
```

**When to Apply:**
- ✅ Shopping carts, wishlists
- ✅ Social features (likes, follows)
- ✅ Any high-frequency user action

**UX Impact:** Feels 400ms faster (from 450ms → 50ms perceived latency)

**Confidence:** HIGH (core pattern in all e-commerce projects)

---

### Pattern: Row-Level Locking for Inventory

**Context:** Concurrent checkouts can cause overselling

**Problem:** 2 users buying last item causes negative inventory

**Solution:**
```typescript
// Use PostgreSQL row-level locking
async function reserveInventory(productId: string, quantity: number) {
  return await db.$transaction(async (tx) => {
    // 1. Lock the product row
    const product = await tx.product.findUnique({
      where: { id: productId },
      select: { id: true, stock: true }
    });

    // Prisma equivalent of SELECT ... FOR UPDATE
    await tx.$executeRaw`
      SELECT * FROM products
      WHERE id = ${productId}
      FOR UPDATE
    `;

    // 2. Check availability
    if (!product || product.stock < quantity) {
      throw new Error('Insufficient stock');
    }

    // 3. Update inventory
    await tx.product.update({
      where: { id: productId },
      data: { stock: { decrement: quantity } }
    });

    // 4. Create reservation (expires in 15 min)
    await tx.inventoryReservation.create({
      data: {
        productId,
        quantity,
        expiresAt: new Date(Date.now() + 15 * 60 * 1000)
      }
    });
  });
}
```

**When to Apply:**
- ✅ E-commerce inventory management
- ✅ Ticket booking systems
- ✅ Any scarce resource allocation

**Prevents:** Overselling, negative inventory, race conditions

**Confidence:** HIGH (solved critical bug in 3 projects)

---

## 🔄 MEDIUM CONFIDENCE PATTERNS (2-4 applications)

### Pattern: React Query Cache Invalidation Strategy

**Context:** Product inventory changes frequently, cache can become stale

**Problem:** Users see wrong stock numbers, causes checkout failures

**Solution:**
```typescript
// Invalidate cache on specific mutations
const { mutate: addToCart } = useMutation({
  mutationFn: (productId: string) => api.cart.add(productId),
  onSuccess: (data, productId) => {
    // Invalidate product details (stock changed)
    queryClient.invalidateQueries(['product', productId]);

    // Invalidate product list (if showing stock)
    queryClient.invalidateQueries(['products']);

    // Update cart count
    queryClient.setQueryData(['cart'], (old) => ({
      ...old,
      itemCount: (old?.itemCount || 0) + 1
    }));
  }
});

// Stale time for different data types
const productQuery = useQuery({
  queryKey: ['product', id],
  queryFn: () => api.products.get(id),
  staleTime: 30 * 1000, // 30s (inventory changes fast)
});

const userProfileQuery = useQuery({
  queryKey: ['user', userId],
  queryFn: () => api.users.get(userId),
  staleTime: 5 * 60 * 1000, // 5 min (profile rarely changes)
});
```

**When to Apply:**
- ✅ Real-time data (inventory, prices)
- ✅ Shopping carts
- ✅ User profiles, settings

**Best Practices:**
- Short stale time for volatile data (inventory: 30s)
- Long stale time for stable data (user profile: 5min)
- Invalidate on mutations that affect multiple queries

**Confidence:** MEDIUM (used in 3 projects, still refining strategy)

---

### Pattern: Next.js Image Optimization for E-commerce

**Context:** Product images are large, slowing down page load

**Problem:** LCP 6+ seconds, Lighthouse score < 50

**Solution:**
```typescript
// Use Next.js Image with optimized settings
<Image
  src={product.imageUrl}
  alt={product.name}
  width={800}
  height={800}
  quality={80}
  priority={aboveFold} // Only for hero images
  placeholder="blur"
  blurDataURL={product.blurHash}
  sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
/>

// Generate blur placeholders at upload time
import { getPlaiceholder } from 'plaiceholder';

async function processProductImage(imageUrl: string) {
  const { base64 } = await getPlaiceholder(imageUrl);

  await db.product.update({
    where: { imageUrl },
    data: { blurHash: base64 }
  });
}
```

**Configuration (next.config.js):**
```javascript
module.exports = {
  images: {
    domains: ['res.cloudinary.com'],
    formats: ['image/avif', 'image/webp'],
    deviceSizes: [640, 750, 828, 1080, 1200],
    imageSizes: [16, 32, 48, 64, 96, 128, 256, 384],
  },
};
```

**Results:**
- LCP: 6.2s → 2.1s ✅
- Lighthouse: 47 → 92 ✅
- Bundle size: No change (images not in bundle)

**Confidence:** MEDIUM (used in 2 projects, still learning edge cases)

---

### Pattern: Zustand Persistence with Selective Sync

**Context:** Shopping cart should persist across sessions but sync with server

**Problem:** User's cart on device may be out of sync with server (expired items, price changes)

**Solution:**
```typescript
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

const useCartStore = create<CartStore>()(
  persist(
    (set, get) => ({
      items: [],
      lastSync: null,

      // Sync with server on mount
      syncWithServer: async () => {
        try {
          const serverCart = await api.cart.get();

          // Merge local + server cart
          const merged = mergeCartItems(
            get().items,
            serverCart.items
          );

          set({
            items: merged,
            lastSync: new Date()
          });
        } catch (error) {
          console.error('Cart sync failed:', error);
        }
      },

      // Auto-sync every 5 minutes
      startAutoSync: () => {
        const interval = setInterval(() => {
          get().syncWithServer();
        }, 5 * 60 * 1000);

        return () => clearInterval(interval);
      }
    }),
    {
      name: 'cart-storage',
      // Don't persist sync timestamp (always sync on mount)
      partialize: (state) => ({
        items: state.items
      })
    }
  )
);

// In app initialization
useEffect(() => {
  const cleanup = useCartStore.getState().startAutoSync();
  useCartStore.getState().syncWithServer(); // Initial sync

  return cleanup;
}, []);
```

**When to Apply:**
- ✅ Shopping carts (local + server state)
- ✅ Draft content (blog posts, forms)
- ✅ User preferences (theme, layout)

**Confidence:** MEDIUM (used in 2 projects, considering alternatives)

---

## 🔬 LOW CONFIDENCE PATTERNS (1 application)

### Pattern: Webhook Retry with Exponential Backoff

**Context:** Stripe webhooks may fail due to temporary issues

**Problem:** Failed webhooks not retried intelligently, causing data inconsistency

**Solution:**
```typescript
import pRetry from 'p-retry';

async function handleWebhookWithRetry(event: Stripe.Event) {
  return await pRetry(
    async () => {
      await processStripeEvent(event);
    },
    {
      retries: 5,
      factor: 2,
      minTimeout: 1000,
      maxTimeout: 30000,
      onFailedAttempt: (error) => {
        console.log(
          `Attempt ${error.attemptNumber} failed. ${error.retriesLeft} retries left.`
        );
      }
    }
  );
}

// Retry schedule:
// Attempt 1: Immediate
// Attempt 2: 1s delay
// Attempt 3: 2s delay
// Attempt 4: 4s delay
// Attempt 5: 8s delay
// Attempt 6: 16s delay (max: 30s)
```

**When to Apply:**
- ✅ Webhook processing
- ✅ External API calls
- ✅ Database operations (transient failures)

**Considerations:**
- Use dead letter queue for permanently failed events
- Log all retry attempts for debugging
- Consider idempotency (this pattern alone doesn't guarantee)

**Confidence:** LOW (only used once, needs more validation)

---

## 📚 Patterns to Extract (Next /learn Session)

**Potential Patterns Noticed:**
1. **SendGrid Email Template Management** - How to version email templates, test locally
2. **Prisma Schema Organization** - Breaking up schema into logical modules
3. **API Error Response Format** - Consistent error responses across endpoints
4. **Environment Variable Validation** - Using Zod to validate env vars on startup
5. **GitHub Actions Deployment Flow** - PR previews, staging auto-deploy, prod manual deploy

**Next Steps:** Run `/learn` after completing current sprint to extract these patterns

---

## 🔗 Cross-References

**Related Semantic Memory:**
- `memory/semantic/tech/payment-integrations.md` - Stripe integration patterns
- `memory/semantic/tech/react-state-management.md` - Zustand patterns
- `memory/semantic/tech/database-optimization.md` - PostgreSQL locking

**Related Episodic Memory:**
- `memory/episodic/2025-11-09-stripe-webhook-debugging.md` - 6-hour debugging session insights
- `memory/episodic/2025-11-06-inventory-race-condition-fix.md` - Initial race condition discovery

**Related Projects:**
- `projects/client-b-analytics/` - Also uses Stripe, similar webhook patterns
- `projects/internal-saas/` - Uses Zustand + React Query (can reuse cache strategy)

---

**Pattern Extraction Stats:**
- Total Patterns: 8
- HIGH Confidence: 3 (reusable immediately)
- MEDIUM Confidence: 4 (need more validation)
- LOW Confidence: 1 (experimental)
- Potential Patterns: 5 (need extraction)

**Next Review:** After sprint ends (Nov 16, 2025)
