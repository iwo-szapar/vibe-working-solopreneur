# Content Chain Workflow

**Purpose:** Transform one idea into a complete content chain (blog → LinkedIn → video → social) in your voice

**Who it's for:** Content creators, founders building audience, thought leaders

**Time savings:** 3-4 hours per content chain (40-50% time reduction)

---

## What This Use Case Provides

### 1. Multi-Format Content Generation
- **Input:** Single topic or content idea
- **Output:** Complete content package:
  1. Blog post (1,200-1,800 words, SEO-optimized)
  2. LinkedIn post (150-300 words, engagement-optimized)
  3. Video script (60-90 seconds, visual storytelling)
  4. Social snippets (3-5 posts for Twitter/Threads)

### 2. Voice & Pattern Consistency
- Uses your learned content patterns (what works)
- Maintains your voice (technical but accessible)
- Applies successful formulas automatically
- References real examples from your work

### 3. Performance Tracking
- Tracks what content performs best
- Extracts patterns from successful content
- Compounds content knowledge over time

---

## Installed Components

### Commands
- `/content-chain [topic] [format]` - Generate complete content package

**Example:** `/content-chain "Building with Claude Code" blog`

### Agents
- **content-strategist** - Creates content chains
  - Reads content patterns (what's worked before)
  - Generates content in your voice
  - Optimizes for each platform (blog/LinkedIn/video)
  - Tracks performance and extracts learnings

### Memory Structure
- **Patterns:** `memory/semantic/patterns/content-patterns.md`
  - What topics resonate
  - What hooks drive clicks
  - What formats perform best
  - Successful formulas

- **Experiences:** `memory/episodic/content/YYYY-MM-DD-title.md`
  - What was created
  - How it performed (views, engagement, conversions)
  - What was learned

- **Ideas:** `memory/semantic/ideas/content-ideas.md`
  - Content ideas captured via `/idea`
  - Prioritized and categorized

---

## How to Use

### Basic Usage

```bash
/content-chain "How I Debug OAuth in Production" blog

# Output:
## Content Chain Created: "How I Debug OAuth in Production"

### 📝 Blog Post (1,650 words)
**Title:** How I Fixed a Production OAuth Bug in 2 Hours (And the Framework I Use Now)
**File:** content-drafts/2025-01-11-oauth-debugging-blog.md
**Status:** Draft ready for review

**Structure:**
- Hook: "2 AM. Production down. 10,000 users locked out."
- Story: Step-by-step debugging journey
- Framework: 5-step process for auth debugging
- Results: Fixed in 2 hours, created prevention pattern

---

### 💼 LinkedIn Post (285 words)
**Hook:** "Production down at 2 AM. 10,000 users locked out. Here's what I learned."
**File:** content-drafts/2025-01-11-oauth-debugging-linkedin.md
**Status:** Ready to publish

---

### 🎥 Video Script (75 seconds)
**Format:** Screen recording + talking head
**File:** content-drafts/2025-01-11-oauth-debugging-video.md
**Status:** Script ready, needs recording

---

### 📱 Social Snippets (4 posts)
**File:** content-drafts/2025-01-11-oauth-debugging-snippets.md
**Status:** Ready to schedule

1. "2 AM. Production down. Here's how I fixed it in 2 hours."
2. "My 5-step OAuth debugging framework..."
3. "JWT tokens expiring too fast? Check mobile settings..."
4. "Every bug becomes a pattern. Never debug the same issue twice."

---

## Next Steps:
1. Review blog draft (15 min)
2. Publish blog
3. Record video (30 min)
4. Schedule LinkedIn + social posts

**Est. time:** 2 hours total (vs 6-8 hours creating from scratch)
```

---

## Content Chain Components

### Component 1: Blog Post

**Structure:**
```markdown
# [Compelling Title with Keyword]

**Meta description:** [155 chars, SEO]
**Reading time:** 7-9 minutes

## Introduction (150 words)
- Hook (dramatic moment, question)
- Context (why this matters)
- Promise (what reader learns)

## The Problem (200 words)
- What went wrong
- Real example
- Cost of not solving

## The Solution (300 words)
- Your approach
- Key principles
- Why it works

## Implementation (500 words)
- Step-by-step guide
- Code examples
- Common pitfalls

## Advanced Tips (200 words)
- Power user techniques
- Customizations

## Real Example (300 words)
- Detailed walkthrough
- Before/after comparison
- Results (numbers!)

## Conclusion (150 words)
- Recap
- Next steps
- CTA
```

### Component 2: LinkedIn Post

**Structure:**
```markdown
[HOOK - First 2 lines]
Make it impossible not to click "see more"

[CONTEXT - 1-2 paragraphs]
Set up problem/opportunity

[VALUE - 3-5 bullets]
Main takeaways

[PROOF - 1 paragraph]
Real example, results

[CTA - Final line]
Question or action

#Hashtag1 #Hashtag2 #Hashtag3
```

### Component 3: Video Script

**Structure:**
```markdown
## Opening (10 sec)
[Hook that stops scroll]

## Problem (15 sec)
[What's broken, why it matters]

## Solution (30 sec)
[How to fix it, key steps]

## Results (20 sec)
[What happened, numbers]

## CTA (15 sec)
[Link to blog, next action]

**Video notes:**
- Thumbnail ideas
- B-roll suggestions
- Caption-friendly (short sentences)
```

### Component 4: Social Snippets

**5 formats:**
1. Problem statement + link
2. Solution teaser + link
3. Specific tip/example + link
4. Data point + link
5. CTA + link

---

## Content Patterns Applied

### Pattern: Story-Driven Technical Content

**What:** Frame technical content as real debugging story

**Why it works:**
- People remember stories > tutorials
- Shows vulnerability (relatable)
- Demonstrates expertise through problem-solving

**Applied in:**
- Blog hook: "2 AM. Production down."
- LinkedIn hook: Same dramatic opening
- Video opening: Start with tension

### Pattern: "Hours Saved" Metrics

**What:** Quantify time/value in specific numbers

**Why it works:**
- Time is universal currency
- Specific > vague ("12 hours" vs "save time")
- Creates "wow" moment

**Applied in:**
- Blog: "Fixed in 2 hours" (not "quickly")
- Social snippet: "Save 10+ hours with this framework"

### Pattern: Before/After Comparisons

**What:** Show concrete results with metrics

**Why it works:**
- Numbers are objective (builds trust)
- Readers can compare to their situation
- Demonstrates real-world impact

**Applied in:**
- Blog implementation section (code examples)
- Video results section (performance data)

---

## Performance Tracking

### Track After Publishing

**File:** `memory/episodic/content/YYYY-MM-DD-title.md`

```markdown
## Content: "How I Debug OAuth in Production"

**Published:** 2025-01-11
**Channels:** Blog + LinkedIn + Video + Social

### Week 1 Performance

**Blog:**
- Views: 350
- Avg time: 6 min
- Bounce rate: 12%

**LinkedIn:**
- Impressions: 4,200
- Likes: 180
- Comments: 32
- Shares: 15

**Video:**
- Views: 1,800
- Watch time: 65%
- Likes: 45

**Social snippets:**
- Combined impressions: 800

### Conversions
- Email signups: 23
- Template downloads: 67
- Consultation bookings: 2
- Revenue attributed: $3,000

### What Worked
- "2 AM production down" hook performed exceptionally
- "12 hours saved" metric was most shared
- Video thumbnail had 8.2% CTR

### What Didn't Work
- Blog intro too long (7% bounced early)
- Video could be 15 sec shorter
- Some hashtags didn't drive engagement

### Patterns to Extract
- Story-driven hooks get 2x engagement (HIGH confidence now)
- Hour savings metrics drive shares (MEDIUM → HIGH confidence)
- Video optimal length: 60-75 sec (new pattern)

**Run:** /learn to update content-patterns.md
```

---

## Distribution Strategy

### Week 1: Initial Push

**Monday 9 AM:**
- Publish blog post
- Share on LinkedIn (link in first comment)
- Pin blog post on website

**Tuesday 3 PM:**
- Upload video to YouTube
- Share video on LinkedIn
- Link to blog in video description

**Wed-Fri:**
- Social snippets (1 per day)
- Each snippet links to blog
- Engage with comments on LinkedIn

### Week 2: Engagement

- Respond to blog comments
- Engage on LinkedIn (reply to comments)
- Re-share high-performing snippets
- Email newsletter (summary + link)

### Ongoing:
- Repurpose as needed (carousels, infographics)
- Update blog with new learnings
- Cross-link in future content

---

## Success Metrics

**Goals per content chain:**

**Traffic:**
- Blog: 200+ views
- LinkedIn: 2,000+ impressions
- Video: 500+ views

**Engagement:**
- LinkedIn: 50+ likes, 10+ comments, 5+ shares
- Video: 50+ likes
- Blog: 5+ email signups

**Conversions:**
- Email signups: 20+
- Consultation bookings: 1-2
- Revenue: $1,500+

**ROI:**
- Time invested: 2-3 hours (with content strategist)
- Expected return: $1,500-$3,000
- ROI: 5-15x

---

## Common Workflows

### Workflow 1: From Client Success to Content Chain

```bash
# Just completed successful client project
# Client A: Improved API performance 10x

# Capture idea
/idea "Blog post: How I made Client A's API 10x faster"

# Later, when ready to create content:
/content-chain "Making APIs 10x Faster" blog

# Content strategist:
# - Reads Client A project notes
# - References performance patterns
# - Creates technical case study
# - Outputs complete content chain

# Review, customize, publish
```

### Workflow 2: Weekly Content Cadence

**Goal:** 1 content chain every 2 weeks

**Monday:** Choose topic from `content-ideas.md`
**Tuesday:** Run `/content-chain`, review drafts
**Wednesday:** Customize drafts, add personal touches
**Thursday:** Record video
**Friday:** Publish blog + LinkedIn
**Next week:** Social snippets + engagement

---

## Next Steps

### Week 1: First Content Chain
- [ ] Pick topic from content-ideas.md
- [ ] Run `/content-chain [topic] blog`
- [ ] Review all 4 components (blog, LinkedIn, video, social)
- [ ] Customize with personal voice
- [ ] Publish blog + LinkedIn

### Week 2: Complete Distribution
- [ ] Record and publish video
- [ ] Schedule social snippets
- [ ] Track performance (views, engagement)

### Week 3: Extract Learnings
- [ ] Review Week 1 performance
- [ ] Run `/learn` to extract content patterns
- [ ] Update content-patterns.md

### Week 4: Optimize
- [ ] Create 2nd content chain
- [ ] Apply learned patterns
- [ ] Compare performance (Week 1 vs Week 4)
- [ ] Refine content strategist prompts

---

**Related use cases:**
- Idea Capture Workflow (capture content ideas)
- Client PM Workflow (client success → case studies)

**Related files:**
- `.claude/agents/content-strategist.md` - Content creation agent
- `memory/semantic/patterns/content-patterns.md` - What content works
- `memory/episodic/content/` - Published content performance
- `memory/semantic/ideas/content-ideas.md` - Content backlog
