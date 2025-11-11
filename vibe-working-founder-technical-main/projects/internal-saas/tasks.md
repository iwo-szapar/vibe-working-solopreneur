# Internal SaaS - FlowStack - Task Queue

**Last Updated:** 2025-11-10
**Current Sprint:** Week 10 (Nov 10-16)
**Sprint Goal:** AI suggestions + Stripe integration complete

---

## 🔴 URGENT (Launch Blockers)

### 1. Improve GPT-4 Task Suggestion Accuracy
**Priority:** URGENT
**Status:** In Progress
**Assignee:** Me
**Due:** Nov 12, 2025
**Estimate:** 6 hours

**Description:**
Current GPT-4 suggestions only 70% accepted by beta testers. Need to improve prompt engineering to reach 85%+ acceptance.

**Current Issues:**
- Suggestions too generic ("Set up project", "Create documentation")
- Missing context from existing tasks
- No priority assignment logic
- Inconsistent task descriptions

**Improved Prompt Strategy:**
1. Add few-shot examples (3 good task sets)
2. Include more project context (tech stack, timeline, team size)
3. Specify output format strictly (JSON schema validation)
4. Add priority assignment based on dependencies

**Acceptance Criteria:**
- [ ] New prompt tested with 10 projects
- [ ] Acceptance rate > 85%
- [ ] Tasks specific and actionable (no generic suggestions)
- [ ] Priorities assigned correctly

**Related Files:**
- `api/src/ai/task-suggester.ts`
- `api/src/ai/prompts/task-suggestion.ts`

**Research:**
- OpenAI prompt engineering guide: https://platform.openai.com/docs/guides/prompt-engineering

---

### 2. Implement Stripe Subscription Flow
**Priority:** URGENT
**Status:** Not Started
**Assignee:** Me
**Due:** Nov 14, 2025
**Estimate:** 12 hours

**Description:**
Build complete Stripe subscription flow for Pro ($15/user/month) and Team ($25/user/month) plans.

**Features Needed:**
1. Pricing page with plan comparison
2. Stripe Checkout integration
3. Webhook handling (subscription events)
4. Customer portal (manage subscription, update card)
5. Usage-based billing (per user)

**Flow:**
```
User clicks "Upgrade to Pro"
  → Stripe Checkout (collect payment)
    → Redirect to success page
      → Webhook creates subscription in DB
        → User gets Pro features
```

**Acceptance Criteria:**
- [ ] Pricing page live
- [ ] Stripe Checkout working
- [ ] Webhook handling all events (created, updated, cancelled, payment_failed)
- [ ] Customer portal accessible from settings
- [ ] Pro features gated correctly (AI suggestions, analytics)

**Related Files:**
- `ui/app/pricing/page.tsx`
- `api/src/stripe/checkout.ts`
- `api/src/stripe/webhooks.ts`

**Stripe Test Mode:** Use test card 4242 4242 4242 4242

---

### 3. Email Notifications for Task Assignments
**Priority:** URGENT
**Status:** Not Started
**Assignee:** Me
**Due:** Nov 15, 2025
**Estimate:** 4 hours

**Description:**
Send email when:
1. User assigned to task
2. User mentioned in comment (@username)
3. Task marked as blocker

**Email Provider:** SendGrid (free tier: 100 emails/day)

**Email Template:**
```
Subject: You were assigned to "{task_title}" in {project_name}

Hi {user_name},

{assigner_name} assigned you to a task:

Task: {task_title}
Project: {project_name}
Due: {due_date}
Priority: {priority}

{task_description}

View task: {task_url}

---
You can manage notification preferences in Settings.
```

**Acceptance Criteria:**
- [ ] SendGrid integrated
- [ ] Email sent on task assignment
- [ ] Email sent on @mention
- [ ] Unsubscribe link included
- [ ] Email preferences in user settings

**Related Files:**
- `api/src/email/sendgrid.ts`
- `api/src/email/templates/task-assigned.html`

---

## 🟠 HIGH PRIORITY (Pre-Launch)

### 4. Find 10 More Beta Testers
**Priority:** HIGH
**Status:** Not Started
**Assignee:** Me (marketing hat)
**Due:** Nov 16, 2025
**Estimate:** 3 hours

**Description:**
Currently have 3 beta testers (friends). Need 10 more to validate product before Product Hunt launch.

**Outreach Channels:**
1. Reddit: r/SideProject, r/entrepreneur, r/startups
2. Indie Hackers: Post in "Show IH" section
3. Twitter: Tweet demo video, tag relevant accounts
4. LinkedIn: Post in PM groups
5. Product Hunt: "Ship" page (pre-launch testers)

**Offer:**
- Free Pro plan forever (early adopter discount)
- Direct Slack access to founder (me)
- Feature requests prioritized

**Acceptance Criteria:**
- [ ] 10 new signups
- [ ] At least 5 actively using (created 3+ tasks)
- [ ] Feedback collected (survey + interviews)

**Time Breakdown:**
- Write Reddit/IH posts: 1 hour
- Create demo video (Loom): 30 min
- Outreach + replies: 1.5 hours

---

### 5. Complete Blocker Detection Algorithm
**Priority:** HIGH
**Status:** In Progress (60% done)
**Assignee:** Me
**Due:** Nov 18, 2025
**Estimate:** 8 hours

**Description:**
Finish blocker detection feature that alerts team when tasks are stuck.

**Detection Logic:**
1. Task "In Progress" > 2x average completion time for this project
2. Task has dependencies but dependency not started
3. No activity (comments, updates) in 5+ days

**Remedies to Suggest:**
- Reassign task to someone else
- Break down into smaller subtasks
- Mark dependency as urgent
- Ask for help in comment

**Acceptance Criteria:**
- [ ] Blocker detection runs daily (cron job)
- [ ] Alerts sent to task assignee + project owner
- [ ] Suggested remedies included in alert
- [ ] Dashboard shows blocked tasks prominently

**Related Files:**
- `api/src/cron/blocker-detection.ts`
- `api/src/ai/blocker-remedies.ts`

---

### 6. Build Pricing Page
**Priority:** HIGH
**Status:** Not Started
**Assignee:** Me
**Due:** Nov 13, 2025
**Estimate:** 4 hours

**Description:**
Design and build pricing page with plan comparison.

**Plans:**
- **Free:** 1 project, 5 users, basic features
- **Pro:** $15/user/month - Unlimited projects, AI suggestions, advanced analytics
- **Team:** $25/user/month - Everything + SSO, priority support, custom integrations

**Design:**
- 3-column comparison table
- Annual discount (20% off if pay yearly)
- FAQ section
- Testimonials from beta users

**Acceptance Criteria:**
- [ ] Pricing page matches Figma design
- [ ] Annual discount toggle works
- [ ] FAQ section complete (10 questions)
- [ ] CTA buttons go to Stripe Checkout

**Related Files:**
- `ui/app/pricing/page.tsx`
- `ui/components/PricingTable.tsx`

---

## 🟢 NORMAL PRIORITY (Post-Launch)

### 7. Task Templates
**Priority:** NORMAL
**Status:** Not Started
**Due:** Dec 2025
**Estimate:** 10 hours

**Description:**
Pre-built task templates for common project types:
- Web app development
- Marketing campaign
- Content creation
- Bug fix process

**Features:**
- Browse templates library
- One-click add template to project
- Customize after adding

**Deferred Until:** Post-launch (nice-to-have, not blocker)

---

### 8. Time Tracking
**Priority:** NORMAL
**Status:** Not Started
**Due:** Jan 2026
**Estimate:** 15 hours

**Description:**
Track time spent on tasks:
- Start/stop timer
- Manual time entry
- Time reports per project
- Exportable timesheets

**Deferred Until:** Phase 2 (users haven't requested yet)

---

## 🔵 LOW PRIORITY (Future)

### 9. Mobile App (React Native)
**Priority:** LOW
**Status:** Not Started
**Due:** TBD (Q2 2026?)
**Estimate:** 100+ hours

**Description:**
Native iOS/Android app for on-the-go task management.

**Deferred Until:** After achieving $10k MRR (need validation first)

---

### 10. AI Learning System (Embeddings + Pinecone)
**Priority:** LOW
**Status:** Research phase
**Due:** TBD (Feb 2026?)
**Estimate:** 40 hours

**Description:**
Use past completed tasks to improve future suggestions.

**Technical Approach:**
1. Generate embeddings for completed tasks (OpenAI Embeddings API)
2. Store in Pinecone vector database
3. When suggesting tasks, find similar past tasks
4. Use historical data for time estimates, dependencies

**Deferred Until:** Post-launch (need more data first)

---

## ✅ COMPLETED (This Week)

### ✅ Supabase Realtime Optimistic Updates
**Completed:** Nov 8, 2025
**Time Spent:** 5 hours

Implemented optimistic updates for task drag-and-drop:
- Update UI immediately (optimistic)
- Send to Supabase in background
- Rollback on error

**Key Learning:**
- Supabase Realtime broadcasts changes to all clients
- Optimistic updates + Realtime can cause conflicts (need conflict resolution)

---

### ✅ File Upload to Supabase Storage
**Completed:** Nov 9, 2025
**Time Spent:** 3 hours

Users can attach files to tasks:
- Drag-and-drop or click to upload
- Stored in Supabase Storage
- Generate signed URLs for download

**Key Learning:**
- Supabase Storage has 1GB free tier
- Need to implement file size limits (10MB per file)

---

## 📊 Sprint Summary

**Sprint Duration:** 1 week (Nov 10-16)
**Total Tasks:** 10
**Urgent:** 3 🔴
**High:** 3 🟠
**Normal:** 2 🟢
**Low:** 2 🔵

**Estimated Hours This Sprint:** 41 hours
**Available Hours:** 20 hours (nights + weekends)
**Overflow:** 21 hours

**Capacity Issue:** MASSIVELY overcommitted (2x capacity)

**Adjustment Plan:**
1. Defer Task #5 (Blocker detection) to next sprint (60% done, can wait)
2. Defer Task #6 (Pricing page) to next sprint (not critical until Stripe working)
3. Reduce Task #2 (Stripe) scope: Skip customer portal for now (add post-launch)
4. **Adjusted hours:** 41 - 8 - 4 - 4 = 25 hours (still 5 hours over)
5. Ask for help: Outsource demo video creation for Task #4 (save 30 min)
6. **Final adjusted:** 24.5 hours (within capacity with buffer)

**Realistic This Week:**
- ✅ Task #1: Improve AI suggestions (6 hrs)
- ✅ Task #2: Stripe integration (8 hrs instead of 12, skip portal)
- ✅ Task #3: Email notifications (4 hrs)
- ✅ Task #4: Find beta testers (2.5 hrs instead of 3, outsource video)
- ⏸️ Task #5: Blocker detection (defer to next sprint)
- ⏸️ Task #6: Pricing page (defer to next sprint)

---

**Critical Path:**
- Task #2 (Stripe) blocks revenue (most important for launch)
- Task #1 (AI suggestions) is differentiator (can't launch without good AI)
- Task #3 (Email notifications) improves engagement (important but not blocker)
- Task #4 (Beta testers) validates product (critical feedback loop)

**Risk:**
- Trying to do too much solo (burnout risk)
- Context switching between this + 2 client projects (exhausting)
- Mitigation: Cut scope aggressively, focus on launch blockers only

---

**Notes:**
- Need to be ruthless about scope (launch with MVP, iterate post-launch)
- Beta testers will tell me what's truly important
- Pricing page can be simple (no fancy design needed)
- Remember: "Done is better than perfect"
