# Internal SaaS - FlowStack - Patterns

**Project:** FlowStack (AI-powered PM tool)
**Last Updated:** 2025-11-10

Patterns discovered while building SaaS product solo.

---

## 🎯 HIGH CONFIDENCE PATTERNS (5+ applications)

### Pattern: Next.js Server Actions for Mutations

**Context:** Building CRUD operations for tasks, projects, comments

**Problem:** Traditional API routes require separate endpoint + client fetch code (lots of boilerplate)

**Solution:**
```typescript
// Before: API route + client fetch
// api/tasks/route.ts
export async function POST(req: Request) {
  const body = await req.json();
  const task = await prisma.task.create({ data: body });
  return Response.json(task);
}

// TaskForm.tsx
const createTask = async (data) => {
  const res = await fetch('/api/tasks', {
    method: 'POST',
    body: JSON.stringify(data)
  });
  return res.json();
};

// After: Server Action (single file)
// actions/tasks.ts
'use server'

export async function createTask(data: CreateTaskInput) {
  // Runs on server, no API route needed
  const task = await prisma.task.create({ data });
  revalidatePath('/projects/[id]'); // Auto-refresh UI
  return task;
}

// TaskForm.tsx
import { createTask } from '@/actions/tasks';

const handleSubmit = async (data) => {
  await createTask(data); // Direct function call
  // UI auto-refreshes via revalidatePath
};
```

**Benefits:**
- Less code (no API route boilerplate)
- Type-safe (TypeScript end-to-end)
- Auto-revalidation (revalidatePath refreshes UI)
- Better DX (feels like calling a function)

**When to Apply:**
- ✅ CRUD operations (create, update, delete)
- ✅ Form submissions
- ✅ Any server mutation

**Confidence:** HIGH (using for all mutations, 20+ actions)

---

### Pattern: Supabase Realtime Optimistic Updates

**Context:** Drag-and-drop kanban board (move tasks between columns)

**Problem:** Waiting for server response makes drag-and-drop feel laggy

**Solution:**
```typescript
import { useOptimistic } from 'react';
import { updateTaskStatus } from '@/actions/tasks';

function KanbanBoard({ initialTasks }) {
  const [optimisticTasks, setOptimisticTasks] = useOptimistic(
    initialTasks,
    (state, newTask) => {
      return state.map(task =>
        task.id === newTask.id ? newTask : task
      );
    }
  );

  async function handleDrop(taskId: string, newStatus: string) {
    // 1. Optimistic update (immediate UI change)
    setOptimisticTasks({ id: taskId, status: newStatus });

    try {
      // 2. Server update (background)
      await updateTaskStatus(taskId, newStatus);

      // Supabase Realtime broadcasts change to all clients
    } catch (error) {
      // 3. Rollback on failure
      toast.error('Failed to move task');
      // useOptimistic auto-reverts on error
    }
  }

  return (
    <DragDropContext onDragEnd={handleDrop}>
      {optimisticTasks.map(task => (
        <TaskCard key={task.id} task={task} />
      ))}
    </DragDropContext>
  );
}

// Supabase Realtime subscription (all clients sync)
useEffect(() => {
  const channel = supabase
    .channel('tasks')
    .on('postgres_changes', {
      event: '*',
      schema: 'public',
      table: 'tasks'
    }, (payload) => {
      // Update UI when other users make changes
      setTasks(payload.new);
    })
    .subscribe();

  return () => supabase.removeChannel(channel);
}, []);
```

**When to Apply:**
- ✅ Drag-and-drop interfaces
- ✅ Collaborative editing
- ✅ Any high-frequency mutations

**Results:**
- Perceived latency: 0ms (optimistic) vs 300ms (waiting for server)
- Feels instant, even on slow connections

**Confidence:** HIGH (core pattern for all realtime features)

---

### Pattern: GPT-4 Prompt Engineering for Task Suggestions

**Context:** AI feature that suggests tasks for a project

**Problem:** Generic suggestions ("Create documentation", "Set up project") not helpful

**Solution:**
```typescript
const TASK_SUGGESTION_PROMPT = `
You are an expert project manager analyzing a software project.

# Project Context
Name: {project_name}
Description: {project_description}
Tech Stack: {tech_stack}
Timeline: {timeline}
Team Size: {team_size}

# Existing Tasks
{existing_tasks_json}

# Task Categories
Choose from: Feature, Bug Fix, Documentation, DevOps, Testing, Design, Research

# Your Task
Suggest 5-10 specific, actionable tasks to complete this project.

# Requirements
1. Be specific (not "Set up backend" but "Create Express server with auth middleware")
2. Consider dependencies (suggest tasks in logical order)
3. Assign priority (URGENT, HIGH, NORMAL, LOW) based on:
   - Blocking other tasks = URGENT
   - Core features = HIGH
   - Nice-to-haves = NORMAL
   - Future improvements = LOW
4. Include time estimate (hours)
5. Avoid duplicating existing tasks

# Output Format (strict JSON)
{
  "tasks": [
    {
      "title": "string (max 100 chars)",
      "description": "string (2-3 sentences)",
      "category": "Feature|Bug Fix|Documentation|...",
      "priority": "URGENT|HIGH|NORMAL|LOW",
      "estimated_hours": number,
      "dependencies": ["task_title_if_any"]
    }
  ]
}

# Examples
{few_shot_examples}
`;

// Few-shot examples (3 high-quality examples)
const FEW_SHOT_EXAMPLES = [
  {
    project: "E-commerce checkout",
    tasks: [
      {
        title: "Implement Stripe Payment Intent flow",
        description: "Integrate Stripe Payment Intent API for secure payments. Handle 3D Secure authentication and payment confirmation webhooks.",
        category: "Feature",
        priority: "URGENT",
        estimated_hours: 8,
        dependencies: []
      },
      // ... 2 more examples
    ]
  }
];

// Usage
async function suggestTasks(projectId: string) {
  const project = await getProject(projectId);
  const existingTasks = await getTasks(projectId);

  const prompt = TASK_SUGGESTION_PROMPT
    .replace('{project_name}', project.name)
    .replace('{project_description}', project.description)
    .replace('{tech_stack}', project.techStack)
    .replace('{timeline}', project.timeline)
    .replace('{team_size}', project.teamSize.toString())
    .replace('{existing_tasks_json}', JSON.stringify(existingTasks))
    .replace('{few_shot_examples}', JSON.stringify(FEW_SHOT_EXAMPLES));

  const response = await openai.chat.completions.create({
    model: 'gpt-4',
    messages: [{ role: 'user', content: prompt }],
    temperature: 0.7, // Balanced creativity
    response_format: { type: 'json_object' } // Force JSON output
  });

  return JSON.parse(response.choices[0].message.content);
}
```

**Key Principles:**
1. **Context is king**: More context = better suggestions
2. **Few-shot examples**: Show GPT what "good" looks like
3. **Strict output format**: JSON schema prevents parsing errors
4. **Temperature tuning**: 0.7 for creative but reasonable suggestions

**Results:**
- Acceptance rate: 70% → 85% after adding context + examples
- Response time: ~2 seconds
- Cost: $0.03 per suggestion (GPT-4 pricing)

**Confidence:** HIGH (refined over 50+ tests)

---

## 🔄 MEDIUM CONFIDENCE PATTERNS (2-4 applications)

### Pattern: Prisma Schema Organization for SaaS

**Context:** SaaS with multi-tenancy (teams, projects, users)

**Problem:** Single schema.prisma file becomes messy with 20+ models

**Solution:**
```prisma
// schema.prisma - Organized by domain

// ============================================
// AUTHENTICATION & USERS
// ============================================

model User {
  id        String   @id @default(cuid())
  email     String   @unique
  name      String?
  avatarUrl String?
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  // Relationships
  memberships TeamMembership[]
  assignedTasks Task[] @relation("AssignedTasks")
  createdTasks  Task[] @relation("CreatedTasks")
  comments     Comment[]

  @@map("users")
}

// ============================================
// TEAM & ORGANIZATION
// ============================================

model Team {
  id        String   @id @default(cuid())
  name      String
  slug      String   @unique
  plan      Plan     @default(FREE)
  createdAt DateTime @default(now())

  // Relationships
  members  TeamMembership[]
  projects Project[]

  @@map("teams")
}

enum Plan {
  FREE
  PRO
  TEAM
}

model TeamMembership {
  id     String @id @default(cuid())
  role   Role   @default(MEMBER)
  userId String
  teamId String

  user User @relation(fields: [userId], references: [id], onDelete: Cascade)
  team Team @relation(fields: [teamId], references: [id], onDelete: Cascade)

  @@unique([userId, teamId])
  @@map("team_memberships")
}

enum Role {
  OWNER
  ADMIN
  MEMBER
}

// ============================================
// PROJECTS & TASKS
// ============================================

model Project {
  id          String   @id @default(cuid())
  name        String
  description String?
  status      ProjectStatus @default(ACTIVE)
  teamId      String
  createdAt   DateTime @default(now())

  team  Team   @relation(fields: [teamId], references: [id], onDelete: Cascade)
  tasks Task[]

  @@map("projects")
}

enum ProjectStatus {
  ACTIVE
  ARCHIVED
  COMPLETED
}

model Task {
  id          String     @id @default(cuid())
  title       String
  description String?
  status      TaskStatus @default(TODO)
  priority    Priority   @default(NORMAL)
  projectId   String
  assigneeId  String?
  creatorId   String
  createdAt   DateTime   @default(now())
  updatedAt   DateTime   @updatedAt

  project  Project @relation(fields: [projectId], references: [id], onDelete: Cascade)
  assignee User?   @relation("AssignedTasks", fields: [assigneeId], references: [id])
  creator  User    @relation("CreatedTasks", fields: [creatorId], references: [id])
  comments Comment[]

  @@index([projectId, status]) // Query optimization
  @@index([assigneeId])
  @@map("tasks")
}

enum TaskStatus {
  TODO
  IN_PROGRESS
  IN_REVIEW
  DONE
}

enum Priority {
  URGENT
  HIGH
  NORMAL
  LOW
}

// ============================================
// BILLING (Stripe)
// ============================================

model Subscription {
  id                 String            @id @default(cuid())
  teamId             String            @unique
  stripeCustomerId   String            @unique
  stripeSubscriptionId String          @unique
  status             SubscriptionStatus
  priceId            String
  currentPeriodEnd   DateTime

  @@map("subscriptions")
}

enum SubscriptionStatus {
  ACTIVE
  CANCELLED
  PAST_DUE
  TRIALING
}
```

**Benefits:**
- Organized by domain (easier to find models)
- Comments separate sections
- Cascading deletes prevent orphaned data
- Indexes for query optimization

**Confidence:** MEDIUM (works well, still learning Prisma best practices)

---

### Pattern: Stripe Webhook Idempotency (SaaS-specific)

**Context:** Stripe subscription webhooks (created, updated, cancelled, payment_failed)

**Problem:** Webhook sent multiple times causes duplicate subscription records

**Solution:**
```typescript
import { Stripe } from 'stripe';
import { PrismaClient } from '@prisma/client';

const stripe = new Stripe(process.env.STRIPE_SECRET_KEY!);
const prisma = new PrismaClient();

export async function handleStripeWebhook(req: Request) {
  const sig = req.headers.get('stripe-signature')!;
  const body = await req.text();

  let event: Stripe.Event;

  try {
    // Verify webhook signature
    event = stripe.webhooks.constructEvent(
      body,
      sig,
      process.env.STRIPE_WEBHOOK_SECRET!
    );
  } catch (err) {
    return Response.json({ error: 'Invalid signature' }, { status: 400 });
  }

  // Idempotency: Check if event already processed
  const existingEvent = await prisma.webhookEvent.findUnique({
    where: { stripeEventId: event.id }
  });

  if (existingEvent) {
    console.log(`Event ${event.id} already processed`);
    return Response.json({ received: true });
  }

  // Process event in transaction
  await prisma.$transaction(async (tx) => {
    // 1. Record webhook event
    await tx.webhookEvent.create({
      data: {
        stripeEventId: event.id,
        type: event.type,
        processedAt: new Date()
      }
    });

    // 2. Handle event
    switch (event.type) {
      case 'customer.subscription.created':
        await handleSubscriptionCreated(event.data.object, tx);
        break;
      case 'customer.subscription.updated':
        await handleSubscriptionUpdated(event.data.object, tx);
        break;
      case 'customer.subscription.deleted':
        await handleSubscriptionCancelled(event.data.object, tx);
        break;
      case 'invoice.payment_failed':
        await handlePaymentFailed(event.data.object, tx);
        break;
    }
  });

  return Response.json({ received: true });
}

async function handleSubscriptionCreated(
  subscription: Stripe.Subscription,
  tx: PrismaClient
) {
  const teamId = subscription.metadata.teamId;

  await tx.subscription.create({
    data: {
      teamId,
      stripeCustomerId: subscription.customer as string,
      stripeSubscriptionId: subscription.id,
      status: subscription.status,
      priceId: subscription.items.data[0].price.id,
      currentPeriodEnd: new Date(subscription.current_period_end * 1000)
    }
  });

  // Upgrade team plan
  await tx.team.update({
    where: { id: teamId },
    data: { plan: 'PRO' }
  });
}
```

**When to Apply:**
- ✅ SaaS with Stripe subscriptions
- ✅ Any webhook integration (Shopify, GitHub)

**Confidence:** MEDIUM (works well, need to test edge cases)

---

### Pattern: Feature Gating by Plan

**Context:** Free users get basic features, Pro users get AI features

**Problem:** Need to check user's plan before allowing features

**Solution:**
```typescript
// middleware/feature-gate.ts
import { getTeamPlan } from '@/lib/auth';

export function requirePlan(allowedPlans: Plan[]) {
  return async function (req: Request) {
    const team = await getTeamPlan(req);

    if (!allowedPlans.includes(team.plan)) {
      return Response.json(
        { error: 'Upgrade to Pro to access this feature' },
        { status: 403 }
      );
    }

    return null; // Allow access
  };
}

// Usage in API route
export async function POST(req: Request) {
  // Check if user has Pro or Team plan
  const gateResult = await requirePlan(['PRO', 'TEAM'])(req);
  if (gateResult) return gateResult;

  // Feature logic (AI suggestions)
  const suggestions = await generateAISuggestions(projectId);
  return Response.json(suggestions);
}

// Usage in UI (show upgrade prompt)
function AIFeatureButton() {
  const { team } = useTeam();

  if (team.plan === 'FREE') {
    return (
      <Button onClick={() => router.push('/pricing')}>
        🚀 Upgrade to Pro for AI suggestions
      </Button>
    );
  }

  return (
    <Button onClick={generateSuggestions}>
      ✨ Generate AI suggestions
    </Button>
  );
}
```

**When to Apply:**
- ✅ SaaS with tiered pricing
- ✅ Feature differentiation (Free vs Pro)

**Confidence:** MEDIUM (simple implementation, but need to track usage limits)

---

## 🔬 LOW CONFIDENCE PATTERNS (1 application)

### Pattern: PostHog for Product Analytics

**Context:** Need to track user behavior (feature usage, funnels, retention)

**Problem:** Google Analytics too basic, Mixpanel too expensive

**Solution:**
```typescript
// lib/posthog.ts
import posthog from 'posthog-js';

export function initPostHog() {
  if (typeof window !== 'undefined') {
    posthog.init(process.env.NEXT_PUBLIC_POSTHOG_KEY!, {
      api_host: 'https://app.posthog.com',
      loaded: (posthog) => {
        if (process.env.NODE_ENV === 'development') {
          posthog.opt_out_capturing(); // Don't track in dev
        }
      }
    });
  }
}

// Track events
export function trackEvent(eventName: string, properties?: object) {
  posthog.capture(eventName, properties);
}

// Identify user (for user-specific tracking)
export function identifyUser(userId: string, traits: object) {
  posthog.identify(userId, traits);
}

// Usage
// Track task creation
trackEvent('task_created', {
  project_id: projectId,
  priority: priority,
  has_ai_suggestion: true
});

// Track feature usage
trackEvent('ai_suggestions_used', {
  project_id: projectId,
  suggestions_count: suggestions.length,
  acceptance_rate: acceptedCount / suggestions.length
});
```

**Dashboards to Build:**
1. Feature usage (which features used most)
2. Onboarding funnel (signup → first task → invite team)
3. Retention (weekly active users)
4. AI feature impact (usage → upgrade correlation)

**Confidence:** LOW (just implemented, need to see data over time)

---

## 📚 Patterns to Extract (Next /learn Session)

**Potential Patterns:**
1. **Supabase Auth Integration** - Social login (Google, GitHub), magic links
2. **Vercel Deployment** - Preview deployments, environment variables, edge config
3. **TailwindCSS Component System** - Reusable components, dark mode, responsive
4. **React Query Cache Strategy** - Stale times, invalidation, prefetching
5. **Solo Founder Energy Management** - When to work on what (high vs low energy)

---

## 🔗 Cross-References

**Related Semantic Memory:**
- `memory/semantic/tech/nextjs-app-router.md` - Server Actions, caching
- `memory/semantic/tech/supabase.md` - Realtime, Auth, Storage
- `memory/semantic/tech/gpt-prompt-engineering.md` - GPT-4 prompt patterns

**Related Episodic Memory:**
- `memory/episodic/2025-11-08-gpt-task-suggestions-tuning.md` - 4-hour prompt engineering session
- `memory/episodic/2025-11-09-stripe-subscription-flow.md` - Stripe webhook integration

**Related Projects:**
- `projects/client-a-ecommerce/` - Shares Stripe patterns
- `projects/client-b-analytics/` - Shares FastAPI patterns (different stack but similar concepts)

---

**Pattern Extraction Stats:**
- Total Patterns: 8
- HIGH Confidence: 3 (reusable immediately)
- MEDIUM Confidence: 4 (working well, need more validation)
- LOW Confidence: 1 (just implemented)
- Potential Patterns: 5 (need extraction)

**Next Review:** After beta launch (Dec 15, 2025)
