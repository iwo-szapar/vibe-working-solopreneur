# Idea Capture Workflow

**Purpose:** Never lose an idea again. Capture ideas instantly (text or voice) with automatic categorization and prioritization.

**Who it's for:** Product managers, founders, creators, anyone with lots of ideas

**Time savings:** 2 hours/week (when voice integration added)

---

## What This Use Case Provides

### 1. Instant Text Capture
- **Command:** `/idea "your idea here"`
- **Time:** 10 seconds to capture
- **What it does:**
  - Categorizes idea (product/content/business/tech/process)
  - Assigns priority (high/medium/low)
  - Saves to appropriate file
  - Links to related patterns

### 2. Voice Capture (Week 3 Setup)
- **How:** Record voice note on phone → Google Drive
- **What happens:**
  - Auto-transcribed (Whisper API or Google Speech)
  - Auto-categorized (AI determines category)
  - Auto-saved to memory
  - Notification sent with summary

###3. Idea Management
- **Files:** `memory/semantic/ideas/[category]-ideas.md`
- **Categories:**
  - Product ideas (features, improvements)
  - Content ideas (blog posts, videos)
  - Business ideas (revenue, partnerships)
  - Tech ideas (architecture, refactoring)
  - Process ideas (workflow improvements)

---

## Installed Components

### Commands
- `/idea [text]` - Quick idea capture

### Agents
- **idea-categorizer** - Categorizes and prioritizes ideas
  - Determines category (product/content/business/tech/process)
  - Assigns priority based on impact, effort, urgency
  - Detects idea clusters (3+ similar ideas → suggests pattern)

### Memory Structure
- **Ideas:** `memory/semantic/ideas/`
  - `product-ideas.md` - Product/feature ideas
  - `content-ideas.md` - Content creation ideas
  - `business-ideas.md` - Business/revenue ideas
  - `tech-ideas.md` - Technical/architecture ideas
  - `process-ideas.md` - Workflow/process improvements

---

## How to Use

### Text Capture (Immediate)

**Scenario:** You're coding and have an idea

```bash
/idea "Add dark mode to dashboard"

# Output:
✅ Idea captured!

**Category:** Product
**Priority:** Medium
**Saved to:** memory/semantic/ideas/product-ideas.md

**Next step:** Add to backlog (current sprint is full)
**Estimated effort:** 2-3 days

Would you like to add this to a project? (/add-task [project])
```

**Time:** 10 seconds vs 5 minutes (opening Notion, finding page, formatting, categorizing)

### Voice Capture (Week 3+)

**Scenario:** Driving home, have idea, can't type

```
1. Pull out phone
2. Open voice recorder
3. Record: "I just realized our dashboard could have real-time collaboration like Google Docs"
4. Save to Google Drive folder (auto-upload)
5. Forget about it

--- 2 minutes later ---

6. Notification: "💡 New idea captured: Real-time collaboration"
7. Open phone, see summary:
   - Category: Product
   - Priority: High
   - Effort: 3-4 weeks
   - Why high: Competitive feature, client-requested
8. Tap "Add to backlog" → done
```

**Time:** 30 seconds vs never captured (lost idea!)

---

## Voice Integration Setup (Week 3)

### Step 1: Google Drive Folder Watcher

**Create folder:**
```
Google Drive/
└── Ideas/
    └── Voice Notes/
```

**Install watcher:**
```bash
cd use-cases/idea-capture-workflow/integrations/google-drive/
npm install

# Configure
cp .env.example .env
# Edit .env:
GOOGLE_DRIVE_FOLDER_ID=your-folder-id
OPENAI_API_KEY=your-key  # For Whisper transcription

# Test
node watch.js --test

# Run continuously
node watch.js
```

### Step 2: Mobile Setup

**iOS:**
1. Install Google Drive app
2. Create Siri Shortcut:
   - "Hey Siri, capture idea"
   - Action: Record voice → Save to "Ideas/Voice Notes" in Google Drive

**Android:**
1. Install Google Drive app
2. Create Google Assistant routine:
   - "Capture idea"
   - Action: Record voice → Save to "Ideas/Voice Notes"

### Step 3: Transcription & Categorization

**Watcher script:**
```javascript
// watch.js (simplified)
const watcher = chokidar.watch(driveFolder);

watcher.on('add', async (filePath) => {
  // 1. Download voice note
  const audioFile = await downloadFile(filePath);

  // 2. Transcribe using Whisper
  const transcription = await openai.audio.transcriptions.create({
    file: audioFile,
    model: "whisper-1"
  });

  // 3. Categorize using Claude
  const result = await claude.run('/idea "' + transcription.text + '"');

  // 4. Send notification
  await sendNotification({
    title: "💡 Idea captured",
    body: result.category + ": " + result.title
  });

  // 5. Clean up voice note (optional)
  await deleteFile(filePath);
});
```

---

## Idea Categorization Logic

### How Categorizer Works

**Input:** `/idea "Add real-time collaboration to dashboard"`

**Categorizer analyzes:**
- **Keywords:** "real-time", "collaboration", "dashboard" → Product
- **Impact:** Competitive feature, client-requested → High priority
- **Effort:** Complex (WebSockets, conflict resolution) → 3-4 weeks
- **Dependencies:** Needs infrastructure work first

**Output:**
```markdown
## Real-Time Collaboration in Dashboard

**Priority:** 🔴 High
**Captured:** 2025-01-11
**Source:** Voice note
**Estimated effort:** 3-4 weeks
**Potential impact:** $10K/year (competitive feature)
**Dependencies:** WebSocket infrastructure
**Next step:** Create technical spec

### Description
Add Google Docs-style real-time collaboration to dashboard...

### Why this matters
- Requested by 3 clients
- Competitors have this
- Unlocks "teams" pricing tier

### Status
- [x] Idea captured
- [ ] Add to roadmap (Q2 2025)
```

---

## Idea Clustering

**Problem:** Capture 50+ ideas, hard to see themes

**Solution:** Categorizer detects clusters

**Example:**
```
💡 Related ideas found!

You've captured 4 ideas about "real-time features":
1. Real-time collaboration (cursor tracking)
2. Live presence indicators
3. Real-time notifications
4. WebSocket infrastructure

**Suggestion:** Create "Real-Time Features" project

**Pattern detected:** Real-time functionality is emerging theme

Run /learn to extract "real-time product development" pattern?
```

---

## Integration with Other Workflows

### Idea → Task

```bash
# Capture idea
/idea "Migrate to PostgreSQL from SQLite"

# Output shows it's high priority
# Convert to task immediately:
/add-task internal-product "Migrate to PostgreSQL" priority=high

# Idea stays in tech-ideas.md (reference)
# Task added to project (actionable)
```

### Idea → Content

```bash
# Capture content idea
/idea "Blog post: How I manage 5 client projects simultaneously"

# Later, when creating content:
/content-chain "Managing 5 Client Projects" blog

# Reads from content-ideas.md
# Creates: blog + LinkedIn + video + snippets
```

### Idea → Pattern

```bash
# Capture multiple similar ideas
/idea "Automate client weekly updates"
/idea "Automate invoicing"
/idea "Automate deployment checklist"

# Categorizer detects cluster (3+ "automate" ideas)
# Suggests pattern: "Workflow automation opportunities"
# Run /learn to extract
```

---

## Success Metrics

**Track in `brain-health/metrics.md`:**

### Capture Rate
- **Ideas captured:** 15/week (before: 3/week lost)
- **Source breakdown:**
  - Voice: 10 (67%) ← most ideas while driving/walking
  - Text: 5 (33%) ← while working

### Conversion Rate
- **Ideas → Implemented:** 18% (before: unmeasured)
- **Avg time idea → implementation:** 12 days

### Categories
- Product: 45 (35%)
- Content: 32 (25%)
- Tech: 22 (17%)
- Business: 18 (14%)
- Process: 10 (8%)

### Time Saved
- **Before:** 5 min per idea × 3 ideas/week = 15 min/week
- **After (text):** 10 sec per idea × 15 ideas/week = 2.5 min/week
- **After (voice):** 30 sec per idea × 15 ideas/week = 7.5 min/week
- **Ideas not lost:** 12/week × value per idea ≈ 2 hours/week saved from recapturing/recreating

---

## Common Workflows

### Workflow 1: Morning Commute Idea Dump

1. Driving to work, 3 ideas strike
2. "Hey Siri, capture idea" × 3
3. Record each idea (30 sec each)
4. Ideas auto-upload to Google Drive
5. Arrive at work, check notifications
6. Review categorized ideas (2 min)
7. Add urgent ones to tasks immediately

**Time:** 3 min vs 15 min (typing up from memory later)

### Workflow 2: Idea Review Session (Weekly)

1. Friday afternoon (30 min)
2. Review all captured ideas from week
3. Group by priority:
   - High → Add to next sprint
   - Medium → Backlog
   - Low → Keep in ideas file
4. Spot clusters (similar ideas)
5. Run `/learn` if pattern detected

### Workflow 3: Idea → Project

1. Idea has matured (captured weeks ago, validated)
2. Ready to execute
3. Convert idea to project:
   - `/new-project "Second Brain SaaS Product"`
   - Copy idea details to project `context.md`
   - Break down into tasks
   - Start execution

---

## Next Steps

### Week 1: Text Capture
- [ ] Try `/idea` 5+ times (build habit)
- [ ] Review idea categorization (correct?)
- [ ] Convert 1-2 high-priority ideas to tasks

### Week 2: Optimize Categorization
- [ ] Review all captured ideas
- [ ] Check categorization accuracy (90%+?)
- [ ] Adjust categorizer if needed

### Week 3: Voice Integration
- [ ] Set up Google Drive folder
- [ ] Configure watcher script
- [ ] Set up mobile shortcut (Siri/Assistant)
- [ ] Test end-to-end (voice → transcription → categorization)

### Week 4: Refine
- [ ] Track capture rate (ideas/week)
- [ ] Measure time saved
- [ ] Identify idea clusters
- [ ] Extract patterns

---

**Related use cases:**
- Content Chain Workflow (ideas → content)
- Dev Workflow (tech ideas → patterns)
- Client PM Workflow (product ideas → tasks)

**Related files:**
- `.claude/agents/idea-categorizer.md` - Categorization logic
- `memory/semantic/ideas/` - All idea files
- `brain-health/metrics.md` - Track idea capture stats
