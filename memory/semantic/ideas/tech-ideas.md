# Technical Ideas - Luca Tomba

**Captured via `/idea` command or voice notes**

**Priority levels:**
- 🔴 **High** - Solves current bottleneck, directly saves time, high ROI
- 🟡 **Medium** - Nice improvement, moderate ROI, not urgent
- 🟢 **Low** - Exploratory, future-looking, low immediate ROI

**Luca's Technical Context:**
- 41-year-old technical founder, 2-person IT services company
- Tech stack: Python/FastAPI, React/TypeScript, PostgreSQL, AWS
- Current pain points: Fibery tracking overhead (6.5hr/week), context switching (8hr/week), idea capture workflow
- Current AI productivity: 8hr/week saved (target: 25hr/week)
- Integration ecosystem: Fibery (project tracking), GitLab (code hosting), Claude Code (second brain)

---

## High Priority Ideas 🔴

### 1. Fibery MCP Server with Bi-Directional Sync

**Priority:** 🔴 High
**Captured:** 2025-11-11
**Source:** Manual Fibery tracking wastes 6.5 hours/week
**Estimated effort:** 3-4 weeks (MCP server + Fibery API integration)
**Impact:** Eliminate 6.5hr/week manual tracking = 338hr/year saved
**ROI:** 338 hours * $100/hour = $33,800 annually
**Next step:** Research MCP server architecture and Fibery GraphQL API

**Problem:**
Luca manually copies tasks between his second brain (Claude Code) and Fibery:
- Creates task in Claude Code → manually creates in Fibery (3-5 min each)
- Updates task status in Fibery → manually updates in Claude Code (2-3 min each)
- Context switching between tools disrupts flow
- Risk of forgetting to sync (leads to out-of-date project state)
- Currently doing this 20+ times per week = 6.5 hours wasted

**Solution:**
Build custom MCP (Model Context Protocol) server that enables Claude Code to directly read/write Fibery data:
- Real-time bi-directional sync (Claude Code ↔ Fibery)
- Automatic task creation: `/add-task` in Claude Code → creates in Fibery automatically
- Status updates: Mark task complete in either system → syncs to other
- Project context: `/switch` loads Fibery data (tasks, deadlines, notes)
- Offline-first: Works without Fibery connection, syncs when online

**Technical Architecture:**

```
Claude Code → MCP Protocol → Custom Fibery MCP Server → Fibery GraphQL API
                                     ↓
                              Local SQLite cache
                              (for offline mode)
```

**Implementation Phases:**

**Phase 1: Read-Only MCP Server (Week 1-2)**
- Set up Node.js/TypeScript MCP server
- Implement Fibery GraphQL API client (authentication, rate limiting)
- Expose read operations: list tasks, get project details, query by status
- Test with `/recall` command (search Fibery from Claude Code)

**Phase 2: Write Operations (Week 3)**
- Implement task creation (map Claude Code task format → Fibery fields)
- Implement status updates (completed, in-progress, blocked)
- Add conflict resolution (last-write-wins with timestamp)
- Validation layer (ensure required Fibery fields present)

**Phase 3: Bi-Directional Sync (Week 4)**
- Webhook listener for Fibery changes (detects updates in Fibery)
- Update local cache when Fibery changes
- Sync on `/switch` command (pull latest Fibery data for project)
- Background sync every 5 minutes (keep data fresh)

**Phase 4: Offline Mode (Week 5 - optional)**
- SQLite local cache for tasks/projects
- Queue write operations when offline
- Sync when connection restored
- Conflict resolution if both systems changed same task

**Technical Details:**

**MCP Server Configuration (`mcp.json`):**
```json
{
  "mcpServers": {
    "fibery": {
      "command": "node",
      "args": ["/path/to/fibery-mcp-server/dist/index.js"],
      "env": {
        "FIBERY_WORKSPACE": "luca-company.fibery.io",
        "FIBERY_API_TOKEN": "fibery_api_token_here"
      }
    }
  }
}
```

**Fibery MCP Server Structure:**
```
fibery-mcp-server/
├── src/
│   ├── index.ts              # MCP server entry point
│   ├── fibery/
│   │   ├── client.ts         # Fibery GraphQL API client
│   │   ├── queries.ts        # GraphQL queries (tasks, projects)
│   │   ├── mutations.ts      # GraphQL mutations (create, update)
│   │   └── webhooks.ts       # Listen for Fibery changes
│   ├── sync/
│   │   ├── mapper.ts         # Map Claude Code ↔ Fibery formats
│   │   ├── conflict.ts       # Conflict resolution (last-write-wins)
│   │   └── queue.ts          # Offline queue (pending writes)
│   ├── cache/
│   │   ├── sqlite.ts         # Local SQLite cache
│   │   └── queries.ts        # Cache queries
│   └── tools/
│       ├── list-tasks.ts     # MCP tool: list tasks
│       ├── create-task.ts    # MCP tool: create task
│       ├── update-task.ts    # MCP tool: update task status
│       └── get-project.ts    # MCP tool: get project context
├── tests/
│   ├── integration/          # Test with real Fibery workspace
│   └── unit/                 # Mock Fibery API responses
└── package.json
```

**Fibery GraphQL API Examples:**

**Query: List tasks for project**
```graphql
query GetProjectTasks($projectId: ID!) {
  fibery {
    findTasks(filter: {project: {id: {is: [$projectId]}}}) {
      id
      name
      state {
        name
      }
      priority {
        name
      }
      dueDate
      assignee {
        name
      }
    }
  }
}
```

**Mutation: Create task**
```graphql
mutation CreateTask($projectId: ID!, $name: String!, $priority: String!) {
  fibery {
    createTask(
      project: {id: $projectId}
      name: $name
      priority: {name: $priority}
    ) {
      id
      name
    }
  }
}
```

**Mutation: Update task status**
```graphql
mutation UpdateTaskStatus($taskId: ID!, $status: String!) {
  fibery {
    updateTask(
      id: $taskId
      state: {name: $status}
    ) {
      id
      state {
        name
      }
    }
  }
}
```

**Configuration File (`config.json`):**
```json
{
  "fibery": {
    "workspace": "luca-company.fibery.io",
    "apiToken": "fibery_api_token_here",
    "rateLimitPerMinute": 100
  },
  "mapping": {
    "projects": {
      "secondBrainPath": "projects/*/context.md",
      "fiberyDatabase": "Projects",
      "fields": {
        "name": "Name",
        "status": "Status",
        "client": "Client"
      }
    },
    "tasks": {
      "secondBrainPath": "projects/*/tasks.md",
      "fiberyDatabase": "Tasks",
      "fields": {
        "title": "Name",
        "priority": "Priority",
        "status": "State",
        "dueDate": "Due Date"
      }
    }
  },
  "sync": {
    "interval": 60,
    "conflictResolution": "last-write-wins",
    "offlineMode": true
  }
}
```

**Usage Examples:**

```bash
# In Claude Code, after MCP server is configured

# Create task in Claude Code → auto-creates in Fibery
/add-task "Fix authentication bug in client-a project" priority:HIGH

# Switch project → loads Fibery data
/switch client-a-ecommerce
# Claude Code fetches from Fibery:
# - Open tasks
# - Recent activity
# - Project notes
# - Deadlines

# Mark task complete → syncs to Fibery
Edit projects/client-a-ecommerce/tasks.md
# Change: - [ ] Fix auth bug
# To:     - [x] Fix auth bug
# MCP server detects change, updates Fibery automatically

# Query Fibery from Claude Code
/recall "tasks due this week"
# Searches both local second brain AND Fibery
```

**Benefits:**
- **Time saved:** 6.5hr/week = 338hr/year
- **Reduced context switching:** Stay in Claude Code, no Fibery tab switching
- **Single source of truth:** Both systems always in sync
- **Offline capability:** Work without internet, sync later
- **Client visibility:** Clients see updated Fibery without Luca manually updating

**Challenges:**
- Fibery GraphQL API learning curve (1 week to master)
- MCP protocol still evolving (may need updates)
- Rate limiting (100 requests/min on Fibery API)
- Conflict resolution (if both systems change same task simultaneously)
- Authentication security (store Fibery API token safely)

**Testing Strategy:**
1. Unit tests with mocked Fibery API
2. Integration tests with test Fibery workspace
3. Manual testing: Create task in Claude Code → verify in Fibery
4. Manual testing: Update task in Fibery → verify in Claude Code
5. Offline testing: Disconnect internet → create tasks → reconnect → verify sync

**Deployment:**
- Package as npm module for easy installation
- Store Fibery credentials in environment variables (not in code)
- Document setup process in README
- Publish to npm (if making open-source) or keep private

**Status:**
- [ ] Research MCP server architecture (read Anthropic MCP docs)
- [ ] Research Fibery GraphQL API (read Fibery API docs)
- [ ] Set up development Fibery workspace for testing
- [ ] Implement read-only MCP server (Phase 1)
- [ ] Implement write operations (Phase 2)
- [ ] Implement bi-directional sync (Phase 3)
- [ ] Test with real projects for 1 week
- [ ] Deploy to production

**Revenue Potential:**
- If productized: $29/month SaaS for Fibery users
- Market: 10,000+ Fibery users worldwide
- Conservative: 50 customers = $1,450/month = $17,400/year recurring

---

### 2. GitLab Webhook Integration for Auto-Learning

**Priority:** 🔴 High
**Captured:** 2025-11-11
**Source:** Forgetting to run `/learn` after completing work (pattern extraction missed)
**Estimated effort:** 2-3 weeks (webhook server + Claude API integration)
**Impact:** Automatic pattern extraction on every git commit = 100% capture rate
**ROI:** Currently missing 50%+ of learnings (forget to `/learn`), this captures all
**Next step:** Research GitLab webhook payloads and Claude API pattern extraction

**Problem:**
Luca completes work but forgets to run `/learn`:
- Finishes debugging → commits code → forgets `/learn` → pattern lost
- Manual `/learn` is friction (requires remembering, takes 2-3 min)
- Patterns extracted inconsistently (maybe 50% of the time)
- Lost opportunities to compound knowledge

**Solution:**
GitLab webhook automatically triggers `/learn` after every commit:
- Git push → GitLab webhook fires → Server receives commit data
- Server analyzes commit (message, diff, files changed)
- Claude API extracts patterns from changes
- Automatically updates `memory/semantic/patterns/dev-patterns.md`
- Optionally: Creates episodic memory record in `memory/episodic/completed-work/`

**Technical Architecture:**

```
GitLab Repo → Webhook (on push) → Node.js/Express Server → Claude API
                                          ↓
                                    Pattern Extraction
                                          ↓
                                    Update dev-patterns.md
                                          ↓
                                    Git commit & push (automated)
```

**Implementation Phases:**

**Phase 1: Webhook Server (Week 1)**
- Set up Node.js/Express server (DigitalOcean droplet or AWS Lambda)
- Configure GitLab webhook (trigger on push events)
- Validate webhook signature (HMAC-SHA256 for security)
- Parse webhook payload (commit message, diff, files changed)
- Log received webhooks for debugging

**Phase 2: Commit Analysis (Week 1-2)**
- Parse commit message using Conventional Commits format
- Analyze code diff (lines added/removed, files modified)
- Calculate "complexity score" (filter out trivial commits like typo fixes)
- Extract relevant context (which files, what changed, why)

**Phase 3: Pattern Extraction (Week 2)**
- Send commit data to Claude API
- Prompt: "Extract development patterns from this commit"
- Parse Claude's response (pattern name, confidence, why it works)
- Format for `dev-patterns.md` (match existing structure)

**Phase 4: Memory Updates (Week 3)**
- Read existing `dev-patterns.md`
- Check for duplicate patterns (don't create if already exists)
- If new pattern: Append to dev-patterns.md with LOW confidence
- If existing pattern: Increment example count, update "last reinforced" date
- Commit changes to second brain repo (automated git commit)

**Phase 5: Notifications (Week 3 - optional)**
- Send notification to Luca: "New pattern extracted: [name]"
- Options: Slack, email, or mobile push notification
- Include link to GitHub commit in second brain repo

**Technical Details:**

**GitLab Webhook Configuration:**
```yaml
# In GitLab project settings → Webhooks
URL: https://luca-learning-webhook.example.com/webhook/gitlab
Secret Token: [random secure token]
Trigger: Push events
SSL Verification: Enabled
```

**Webhook Server Structure:**
```
gitlab-learning-webhook/
├── src/
│   ├── server.ts              # Express webhook server
│   ├── webhooks/
│   │   ├── handler.ts         # GitLab webhook handler
│   │   └── validator.ts       # HMAC signature validation
│   ├── analysis/
│   │   ├── commit-parser.ts   # Parse commit messages
│   │   ├── diff-analyzer.ts   # Analyze code changes
│   │   └── complexity.ts      # Calculate commit complexity
│   ├── learning/
│   │   ├── queue.ts           # Async processing queue
│   │   ├── extractor.ts       # Claude API pattern extraction
│   │   └── writer.ts          # Update dev-patterns.md
│   └── utils/
│       ├── similarity.ts      # Check for duplicate patterns
│       └── confidence.ts      # Calculate pattern confidence
├── config/
│   └── config.json            # Configuration
├── tests/
│   ├── fixtures/              # Sample webhook payloads
│   └── integration/           # Test with real GitLab webhooks
└── package.json
```

**Webhook Payload Example:**
```json
{
  "object_kind": "push",
  "user_name": "Luca Tomba",
  "commits": [
    {
      "id": "a1b2c3d4",
      "message": "fix(auth): Prevent JWT token expiration during active use\n\nImplemented proactive token refresh logic that renews tokens 5-10 minutes before expiration.",
      "timestamp": "2025-11-11T10:30:00Z",
      "url": "https://gitlab.com/luca/project/commit/a1b2c3d4",
      "author": {
        "name": "Luca Tomba",
        "email": "luca@example.com"
      },
      "added": ["src/auth/token-refresh.ts"],
      "modified": ["src/auth/auth-service.ts"],
      "removed": []
    }
  ]
}
```

**Commit Complexity Scoring:**
```typescript
// complexity.ts
interface ComplexityFactors {
  linesChanged: number;      // Total lines added + removed
  filesModified: number;     // Number of files touched
  languages: string[];       // Programming languages involved
  commitType: string;        // fix, feat, refactor, docs, etc.
}

function calculateComplexity(commit: Commit): number {
  let score = 0;

  // Lines changed (0-10 scale)
  const linesChanged = commit.added.length + commit.removed.length;
  if (linesChanged < 50) score += 3;
  else if (linesChanged < 200) score += 5;
  else score += 8;

  // Files modified (0-10 scale)
  if (commit.modified.length === 1) score += 2;
  else if (commit.modified.length <= 5) score += 5;
  else score += 8;

  // Languages (0-10 scale)
  const langs = detectLanguages(commit.modified);
  if (langs.length === 1) score += 2;
  else if (langs.length === 2) score += 5;
  else score += 8;

  // Commit type (0-10 scale)
  if (commit.message.startsWith('fix:')) score += 7;      // Bug fixes often have patterns
  else if (commit.message.startsWith('feat:')) score += 5; // New features sometimes
  else if (commit.message.startsWith('refactor:')) score += 6;
  else if (commit.message.startsWith('docs:')) score += 2; // Docs rarely have patterns

  return score / 4; // Average of all factors (0-10 scale)
}

// Only process commits with score >= 5.0
if (calculateComplexity(commit) >= 5.0) {
  processCommit(commit);
}
```

**Claude API Pattern Extraction:**
```typescript
// extractor.ts
async function extractPattern(commit: Commit): Promise<Pattern | null> {
  const prompt = `
You are analyzing a git commit to extract reusable development patterns.

Commit message: ${commit.message}
Files changed: ${commit.modified.join(', ')}
Lines added: ${commit.added.length}
Lines removed: ${commit.removed.length}

Code diff:
${commit.diff}

Based on this commit, identify if there's a reusable development pattern.

If yes, respond with JSON:
{
  "hasPattern": true,
  "pattern": {
    "name": "Pattern Name",
    "confidence": "LOW",
    "context": "When does this pattern apply?",
    "approach": "What's the reusable approach?",
    "why": "Why does this work?",
    "codeExample": "Code snippet showing the pattern"
  }
}

If no clear pattern, respond:
{
  "hasPattern": false,
  "reason": "Why no pattern (too simple, too specific, etc.)"
}
`;

  const response = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: {
      'anthropic-version': '2023-06-01',
      'content-type': 'application/json',
      'x-api-key': process.env.CLAUDE_API_KEY
    },
    body: JSON.stringify({
      model: 'claude-3-5-sonnet-20250219',
      max_tokens: 2048,
      messages: [{
        role: 'user',
        content: prompt
      }]
    })
  });

  const data = await response.json();
  const result = JSON.parse(data.content[0].text);

  return result.hasPattern ? result.pattern : null;
}
```

**Pattern Writer:**
```typescript
// writer.ts
async function updateDevPatterns(pattern: Pattern, commit: Commit) {
  // Read existing dev-patterns.md
  const content = await fs.readFile('memory/semantic/patterns/dev-patterns.md', 'utf-8');

  // Check for duplicate pattern (fuzzy string matching)
  const isDuplicate = checkDuplicate(pattern.name, content);

  if (isDuplicate) {
    // Update existing pattern: increment example count, update "last reinforced" date
    const updated = incrementPatternExamples(content, pattern.name, commit);
    await fs.writeFile('memory/semantic/patterns/dev-patterns.md', updated);
    console.log(`Updated existing pattern: ${pattern.name}`);
  } else {
    // Add new pattern with LOW confidence
    const newPattern = formatPattern(pattern, commit);
    const updated = appendPattern(content, newPattern);
    await fs.writeFile('memory/semantic/patterns/dev-patterns.md', updated);
    console.log(`Added new pattern: ${pattern.name}`);
  }

  // Git commit the changes
  await exec('git add memory/semantic/patterns/dev-patterns.md');
  await exec(`git commit -m "learn: Auto-extracted pattern from ${commit.id.substring(0, 7)}"`);
  await exec('git push origin main');
}
```

**Configuration (`config.json`):**
```json
{
  "gitlab": {
    "webhookSecret": "your-secret-token-here",
    "signatureHeader": "X-Gitlab-Token"
  },
  "claude": {
    "apiKey": "sk-ant-...",
    "model": "claude-3-5-sonnet-20250219",
    "maxTokens": 2048
  },
  "secondBrain": {
    "repoPath": "/path/to/second-brain-repo",
    "patternsFile": "memory/semantic/patterns/dev-patterns.md",
    "episodicPath": "memory/episodic/completed-work/"
  },
  "processing": {
    "minComplexityScore": 5.0,
    "queueEnabled": true,
    "retryAttempts": 3
  },
  "notifications": {
    "enabled": true,
    "slack": {
      "webhookUrl": "https://hooks.slack.com/services/..."
    }
  }
}
```

**Benefits:**
- **100% pattern capture:** Never forget to `/learn` again
- **Zero manual effort:** Happens automatically on git push
- **Consistent extraction:** Same prompt every time (quality)
- **Real-time updates:** Patterns available within minutes of commit
- **Episodic memory:** Auto-creates work log in `memory/episodic/`

**Challenges:**
- False positives (trivial commits triggering pattern extraction)
- Claude API costs ($0.03 per commit × 50 commits/week = $1.50/week = $78/year)
- Rate limiting (Claude API: 50 requests/min, need queue for burst pushes)
- Security (webhook secret must be secure, validate all requests)
- Git conflicts (if manual `/learn` runs simultaneously with auto-learning)

**Testing Strategy:**
1. Test with sample webhook payloads (mock GitLab responses)
2. Test complexity scoring (ensure trivial commits filtered out)
3. Test Claude API extraction (verify patterns make sense)
4. Test pattern deduplication (don't create duplicates)
5. Test git automation (ensure commits don't conflict)

**Deployment:**
- Deploy to AWS Lambda (serverless, cheap: ~$5/month)
- Or DigitalOcean droplet ($6/month, more control)
- Set up monitoring (CloudWatch or custom logging)
- Configure GitLab webhook to point to deployed URL

**Status:**
- [ ] Research GitLab webhook format
- [ ] Set up Node.js/Express webhook server
- [ ] Implement HMAC signature validation
- [ ] Implement commit complexity scoring
- [ ] Integrate Claude API for pattern extraction
- [ ] Test with real commits for 1 week
- [ ] Deploy to production

**Revenue Potential:**
- If productized: "Auto-Learn" add-on for second brain systems
- Market: Developers using Claude Code + GitLab/GitHub
- Pricing: $19/month per user
- Conservative: 100 customers = $1,900/month = $22,800/year recurring

---

### 3. Voice-to-Text Pipeline with Google Drive Auto-Sync

**Priority:** 🔴 High
**Captured:** 2025-11-11
**Source:** Ideas come while walking/driving, but hard to capture (currently 1-2 ideas/week, should be 5-10)
**Estimated effort:** 2 weeks (Google Drive API + Whisper API + Claude categorization)
**Impact:** 5-10 ideas captured per week (vs current 1-2) = 3-4x more ideas
**ROI:** More ideas → more opportunities → potential new products/clients
**Next step:** Research Google Drive API for file polling

**Problem:**
Luca has ideas while away from computer:
- Walking, driving, showering → idea strikes → forgets by the time at computer
- Currently: Stops, types into phone notes app → manually transcribes later (tedious)
- Many ideas lost because friction too high to capture

**Solution:**
Voice memo → auto-transcription → auto-categorization → lands in second brain:
- Record voice memo on phone (30-60 seconds)
- Auto-upload to Google Drive (iOS/Android native feature)
- Server watches Drive folder → detects new audio file
- Whisper API transcribes audio → text
- Claude API categorizes idea (product, business, or technical)
- Automatically appends to appropriate memory file
- Notification: "Idea captured: [title]"

**Technical Architecture:**

```
Voice Memo (Phone) → Auto-upload to Google Drive → Drive Watcher (poll every 60s)
                                                           ↓
                                                    Download audio file
                                                           ↓
                                                    Whisper API ($0.006/min)
                                                           ↓
                                                    Transcription text
                                                           ↓
                                                    Claude API (categorize)
                                                           ↓
                                            Append to product-ideas.md / business-ideas.md / tech-ideas.md
                                                           ↓
                                                    Send notification (Slack/email)
```

**Implementation Phases:**

**Phase 1: Google Drive Watcher (Week 1)**
- Set up Google Drive API OAuth 2.0 authentication
- Create Node.js script that polls Drive folder every 60 seconds
- Detect new files (filter by .m4a, .mp3, .wav extensions)
- Download audio file to local temp directory
- Mark file as "processed" (move to archive folder or add metadata)

**Phase 2: Whisper Transcription (Week 1)**
- Integrate OpenAI Whisper API (or self-hosted Whisper if privacy concern)
- Send audio file to Whisper API
- Parse transcription response (text + confidence score)
- Cost: $0.006 per minute of audio (typical idea: 30-60 sec = $0.003-$0.006 per idea)

**Phase 3: Claude Categorization (Week 2)**
- Send transcription to Claude API
- Prompt: "Categorize this idea as product, business, or technical"
- Extract: Category, title (1-sentence summary), priority (HIGH/MEDIUM/LOW)
- Format for memory files (match existing structure)

**Phase 4: Memory File Updates (Week 2)**
- Append to appropriate file: `product-ideas.md`, `business-ideas.md`, or `tech-ideas.md`
- Include: Priority, captured date, source (voice memo), estimated effort, impact
- Git commit the changes (automated)
- Send notification to Luca (Slack or email)

**Technical Details:**

**Google Drive API Setup:**
```typescript
// drive-watcher.ts
import { google } from 'googleapis';

async function watchDriveFolder(folderId: string) {
  const auth = await google.auth.getClient({
    keyFile: 'config/google-credentials.json',
    scopes: ['https://www.googleapis.com/auth/drive.readonly']
  });

  const drive = google.drive({ version: 'v3', auth });

  // Poll every 60 seconds
  setInterval(async () => {
    const response = await drive.files.list({
      q: `'${folderId}' in parents and trashed=false and (mimeType='audio/m4a' or mimeType='audio/mpeg' or mimeType='audio/wav')`,
      fields: 'files(id, name, createdTime)',
      orderBy: 'createdTime desc'
    });

    for (const file of response.data.files) {
      if (!isProcessed(file.id)) {
        await processAudioFile(file);
      }
    }
  }, 60000); // 60 seconds
}
```

**Whisper API Transcription:**
```typescript
// whisper.ts
async function transcribeAudio(audioPath: string): Promise<string> {
  const formData = new FormData();
  formData.append('file', fs.createReadStream(audioPath));
  formData.append('model', 'whisper-1');
  formData.append('language', 'en');

  const response = await fetch('https://api.openai.com/v1/audio/transcriptions', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${process.env.OPENAI_API_KEY}`
    },
    body: formData
  });

  const data = await response.json();
  return data.text;
}
```

**Claude API Categorization:**
```typescript
// categorizer.ts
async function categorizeIdea(transcription: string): Promise<IdeaMetadata> {
  const prompt = `
Categorize this voice memo idea:

"${transcription}"

Respond with JSON:
{
  "category": "product" | "business" | "technical",
  "title": "One-sentence summary (max 80 chars)",
  "priority": "HIGH" | "MEDIUM" | "LOW",
  "estimatedEffort": "Timeframe (e.g., '2-3 weeks', '1 day', '3-6 months')",
  "impact": "Describe potential impact",
  "nextStep": "First concrete action to take"
}
`;

  const response = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: {
      'anthropic-version': '2023-06-01',
      'content-type': 'application/json',
      'x-api-key': process.env.CLAUDE_API_KEY
    },
    body: JSON.stringify({
      model: 'claude-3-5-sonnet-20250219',
      max_tokens: 512,
      messages: [{
        role: 'user',
        content: prompt
      }]
    })
  });

  const data = await response.json();
  return JSON.parse(data.content[0].text);
}
```

**Memory File Appender:**
```typescript
// memory-writer.ts
async function appendIdea(idea: IdeaMetadata, transcription: string) {
  const filename = idea.category === 'product' ? 'product-ideas.md' :
                   idea.category === 'business' ? 'business-ideas.md' :
                   'tech-ideas.md';

  const filepath = `memory/semantic/ideas/${filename}`;

  const prioritySection = idea.priority === 'HIGH' ? '## High Priority Ideas 🔴' :
                          idea.priority === 'MEDIUM' ? '## Medium Priority Ideas 🟡' :
                          '## Low Priority Ideas 🟢';

  const newIdea = `
### ${idea.title}

**Priority:** ${idea.priority === 'HIGH' ? '🔴' : idea.priority === 'MEDIUM' ? '🟡' : '🟢'} ${idea.priority}
**Captured:** ${new Date().toISOString().split('T')[0]} (voice memo)
**Source:** Voice memo while ${guessContext(new Date().getHours())}
**Estimated effort:** ${idea.estimatedEffort}
**Impact:** ${idea.impact}
**Next step:** ${idea.nextStep}

**Original transcript:**
"${transcription}"

**Status:**
- [ ] ${idea.nextStep}

---
`;

  // Read file, find correct priority section, append idea
  const content = await fs.readFile(filepath, 'utf-8');
  const updated = insertAfterSection(content, prioritySection, newIdea);
  await fs.writeFile(filepath, updated);

  // Git commit
  await exec(`git add ${filepath}`);
  await exec(`git commit -m "idea: Captured via voice memo - ${idea.title}"`);
  await exec('git push origin main');
}

function guessContext(hour: number): string {
  if (hour >= 6 && hour < 9) return 'morning walk';
  if (hour >= 9 && hour < 12) return 'work session';
  if (hour >= 12 && hour < 14) return 'lunch break';
  if (hour >= 14 && hour < 18) return 'afternoon work';
  if (hour >= 18 && hour < 22) return 'evening';
  return 'late night';
}
```

**Configuration (`config.json`):**
```json
{
  "googleDrive": {
    "credentialsPath": "config/google-credentials.json",
    "folderId": "1abc123def456",
    "pollIntervalSeconds": 60,
    "supportedFormats": ["m4a", "mp3", "wav"],
    "archiveFolderId": "1xyz789ghi012"
  },
  "whisper": {
    "apiKey": "sk-...",
    "model": "whisper-1",
    "language": "en",
    "costPerMinute": 0.006
  },
  "claude": {
    "apiKey": "sk-ant-...",
    "model": "claude-3-5-sonnet-20250219",
    "categorizationPrompt": "prompts/categorize-idea.txt"
  },
  "memory": {
    "productIdeasPath": "memory/semantic/ideas/product-ideas.md",
    "businessIdeasPath": "memory/semantic/ideas/business-ideas.md",
    "techIdeasPath": "memory/semantic/ideas/tech-ideas.md",
    "episodicPath": "memory/episodic/ideas/"
  },
  "notifications": {
    "slack": {
      "webhookUrl": "https://hooks.slack.com/services/..."
    }
  }
}
```

**Workflow Example:**

1. **Luca's morning walk (7:30 AM):**
   - Idea strikes: "What if we built a Fibery MCP server for bi-directional sync?"
   - Opens Voice Memos app on iPhone
   - Records 45-second memo explaining the idea
   - Voice Memos auto-uploads to Google Drive (iOS feature)

2. **Server detects new file (7:31 AM):**
   - Drive watcher polls every 60 seconds
   - Detects `Voice Memo 2025-11-11 07:30.m4a`
   - Downloads file to temp directory

3. **Transcription (7:31 AM):**
   - Sends to Whisper API
   - Receives transcription: "What if we built a Fibery MCP server for bi-directional sync? This would eliminate the 6.5 hours per week I spend manually copying tasks between Claude Code and Fibery..."
   - Cost: $0.006 per minute × 0.75 min = $0.0045

4. **Categorization (7:32 AM):**
   - Sends transcription to Claude API
   - Claude responds:
     ```json
     {
       "category": "technical",
       "title": "Fibery MCP Server with Bi-Directional Sync",
       "priority": "HIGH",
       "estimatedEffort": "3-4 weeks",
       "impact": "Eliminate 6.5hr/week manual tracking = 338hr/year saved",
       "nextStep": "Research MCP server architecture and Fibery GraphQL API"
     }
     ```

5. **Memory update (7:32 AM):**
   - Appends to `tech-ideas.md` under "## High Priority Ideas 🔴"
   - Git commits: `idea: Captured via voice memo - Fibery MCP Server with Bi-Directional Sync`
   - Pushes to GitHub

6. **Notification (7:33 AM):**
   - Slack message: "💡 New HIGH priority idea captured: Fibery MCP Server with Bi-Directional Sync"
   - Luca sees notification, feels satisfied idea is saved

**Benefits:**
- **Zero friction:** Record voice memo → done (no manual typing)
- **Never lose ideas:** Capture while walking, driving, showering
- **Automatic categorization:** Claude decides product/business/technical
- **3-4x more ideas:** Currently 1-2/week, potential 5-10/week
- **Searchable:** All ideas in second brain, searchable via `/recall`

**Challenges:**
- Audio quality (background noise, mumbling)
- Transcription errors (Whisper not perfect, especially with jargon)
- Costs ($0.003-$0.006 per idea × 10 ideas/week = $0.03-$0.06/week = $1.56-$3.12/year - negligible)
- Privacy (audio uploaded to Google Drive and OpenAI Whisper - GDPR concern if sensitive info)
- Phone setup (must configure auto-upload to specific Google Drive folder)

**Testing Strategy:**
1. Test with 10 sample voice memos (various lengths, accents, background noise)
2. Validate transcription accuracy (>95% word accuracy)
3. Validate Claude categorization (100% correct category assignment)
4. Test notification delivery (Slack/email)
5. Test git automation (no conflicts)

**Deployment:**
- Deploy to AWS Lambda (scheduled job every 60 seconds)
- Or Raspberry Pi at home (always-on, free hosting)
- Monitor with CloudWatch or custom logging

**Status:**
- [ ] Research Google Drive API for file polling
- [ ] Set up Google Drive OAuth 2.0 credentials
- [ ] Implement Drive watcher (poll every 60 seconds)
- [ ] Integrate Whisper API
- [ ] Integrate Claude API for categorization
- [ ] Test with 10 voice memos
- [ ] Deploy to production

**Revenue Potential:**
- If productized: "Voice Idea Capture" for second brain users
- Market: Knowledge workers using Claude Code or similar systems
- Pricing: $9/month per user
- Conservative: 200 customers = $1,800/month = $21,600/year recurring

---

## Medium Priority Ideas 🟡

### 4. Real-Time WebSocket Dashboard for Client Status

**Priority:** 🟡 Medium
**Captured:** 2025-11-11
**Source:** Clients asking "What's the status?" multiple times per day (interrupts flow)
**Estimated effort:** 2-3 weeks (WebSocket infrastructure + dashboard UI)
**Impact:** Reduce client interruptions by 50% = 2-3 hours/week saved
**ROI:** 2.5hr/week × 52 weeks = 130hr/year saved
**Next step:** Research WebSocket libraries (Socket.io vs native WebSockets)

**Problem:**
Clients interrupt Luca with status questions:
- Client: "What's the status of the authentication bug?"
- Client: "Is the deployment ready?"
- Each interruption: 5-10 minutes (context switch + response)
- Happens 3-5 times per day = 15-50 minutes wasted
- Total: 2-3 hours/week

**Solution:**
Real-time dashboard showing project status:
- Public URL: `https://status.luca-company.com/client-a-ecommerce`
- Auto-updates when Luca marks tasks complete (no manual status updates)
- Shows: Open tasks, blockers, next milestone, estimated completion date
- Clients can check anytime (self-service, no interruption)

**Technical Architecture:**

```
Luca marks task complete → Git commit → Webhook fires → WebSocket Server
                                                              ↓
                                                    Broadcast update
                                                              ↓
                                                    Client Dashboard (browser)
                                                    Auto-refreshes with new status
```

**Benefits:**
- **Reduce interruptions:** Clients self-serve status (no Slack/email questions)
- **Transparency:** Clients see real-time progress (builds trust)
- **Automatic updates:** Luca never manually updates status (zero effort)

**Status:**
- [ ] Research WebSocket libraries
- [ ] Design dashboard UI (mockup)
- [ ] Implement WebSocket server (Node.js + Socket.io)
- [ ] Connect to second brain git repo (detect changes)
- [ ] Deploy to production

---

### 5. Automated Test Generation from Git Commits

**Priority:** 🟡 Medium
**Captured:** 2025-11-11
**Source:** Test coverage low (~40%), writing tests manually is tedious
**Estimated effort:** 3-4 weeks (Claude API + test generation logic)
**Impact:** Increase test coverage from 40% to 70-80%
**ROI:** Prevent production bugs (hard to quantify, but high value)
**Next step:** Research test generation prompts for Claude API

**Problem:**
Luca writes code but skips tests (time pressure):
- Manual test writing is tedious (30-60 min per module)
- Test coverage currently ~40% (risky for production)
- Bugs slip through to production (client complaints)

**Solution:**
Auto-generate tests after every commit:
- Git push → Webhook fires → Claude API generates tests
- Test format: Jest (TypeScript), pytest (Python)
- Covers: Happy path, edge cases, error handling
- Creates PR with generated tests → Luca reviews → Merge

**Benefits:**
- **Higher test coverage:** 40% → 70-80%
- **Less manual effort:** Claude writes tests, Luca reviews (10 min vs 60 min)
- **Fewer production bugs:** Catch issues before deployment

**Status:**
- [ ] Research test generation prompts
- [ ] Prototype with Claude API (generate tests for sample code)
- [ ] Integrate with GitLab webhook
- [ ] Test with real commits
- [ ] Deploy to production

---

### 6. Infrastructure-as-Code Template Generator

**Priority:** 🟡 Medium
**Captured:** 2025-11-11
**Source:** Setting up new client projects takes 4-6 hours (AWS, DB, CI/CD)
**Estimated effort:** 2 weeks (Terraform/Pulumi template library)
**Impact:** Reduce project setup from 4-6 hours to 30 minutes
**ROI:** 4hr/project × 10 projects/year = 40hr/year saved
**Next step:** Research Terraform vs Pulumi for template generation

**Problem:**
Every new client project requires manual infrastructure setup:
- AWS (EC2, RDS, S3, CloudFront, Route53)
- Database (PostgreSQL, Redis)
- CI/CD (GitLab pipelines, deployment scripts)
- Monitoring (CloudWatch, Sentry)
- Currently: 4-6 hours of manual clicking in AWS Console

**Solution:**
Template generator for common infrastructure patterns:
- CLI tool: `infra-gen --project client-a --type ecommerce`
- Generates: Terraform/Pulumi files, GitLab CI/CD config, Docker Compose
- Luca customizes (DB size, region, etc.)
- Run `terraform apply` → infrastructure provisioned in 10 minutes

**Benefits:**
- **Time saved:** 4-6 hours → 30 minutes per project
- **Consistency:** All projects use same infrastructure patterns
- **Documentation:** Infrastructure-as-code = self-documenting

**Status:**
- [ ] Research Terraform vs Pulumi
- [ ] Create template library (3-5 common patterns)
- [ ] Build CLI tool
- [ ] Test with new client project
- [ ] Deploy to production

---

### 7. Database Migration Safety Framework

**Priority:** 🟡 Medium
**Captured:** 2025-11-11
**Source:** Nearly broke production with unsafe migration (forgot to test rollback)
**Estimated effort:** 1 week (migration validator + rollback scripts)
**Impact:** Prevent production database disasters
**ROI:** Hard to quantify, but one production incident costs 10-20 hours
**Next step:** Research Django migration best practices

**Problem:**
Database migrations are risky:
- Forgot to test rollback → can't undo migration if bug found
- Schema changes can lock tables (downtime)
- Data migrations can corrupt data if logic is buggy

**Solution:**
Migration safety framework:
- Pre-deployment validation: Check for unsafe operations (dropping columns, locking tables)
- Automatic rollback scripts: Generate `down` migration for every `up` migration
- Staging tests: Run migration on production data dump in staging
- Zero-downtime migrations: Blue-green deployment strategy

**Benefits:**
- **Prevent disasters:** Catch unsafe migrations before production
- **Easy rollback:** Always have `down` migration ready
- **Zero downtime:** Deploy migrations without taking site offline

**Status:**
- [ ] Research Django migration best practices
- [ ] Build migration validator (check for unsafe operations)
- [ ] Generate automatic rollback scripts
- [ ] Test with production data dump
- [ ] Deploy to production

---

### 8. Local Development Environment Generator

**Priority:** 🟡 Medium
**Captured:** 2025-11-11
**Source:** Onboarding new contractor took 3 hours (setting up local environment)
**Estimated effort:** 1 week (Docker Compose templates)
**Impact:** Reduce onboarding from 3 hours to 10 minutes
**ROI:** 3hr/contractor × 5 contractors/year = 15hr/year saved
**Next step:** Create Docker Compose template for standard tech stack

**Problem:**
New contractors waste time setting up local environment:
- Install Python, Node.js, PostgreSQL, Redis
- Configure environment variables
- Seed database with test data
- Run migrations
- Currently: 2-3 hours (often with errors)

**Solution:**
Docker Compose template:
- `docker-compose up` → entire environment running in 2 minutes
- Includes: Database (PostgreSQL + Redis), backend (FastAPI), frontend (React), seeded test data
- Contractor runs one command → ready to code

**Benefits:**
- **Fast onboarding:** 3 hours → 10 minutes
- **Consistency:** Everyone has identical environment (no "works on my machine")
- **Easy reset:** `docker-compose down` → fresh environment

**Status:**
- [ ] Create Docker Compose template
- [ ] Add database seeding scripts
- [ ] Document setup process
- [ ] Test with new contractor
- [ ] Publish to GitHub

---

### 9. Build Command Testing Framework (from user request)

**Priority:** 🟡 Medium
**Captured:** 2025-11-10
**Source:** Testing /idea command functionality
**Estimated effort:** 1-2 days (framework setup, writing validators)
**Impact:** Automated validation of all 11 slash commands, prevent regressions, ensure reliability
**Next step:** Research test frameworks and define specification format

**Problem:**
Currently testing commands manually (time-consuming, error-prone):
- No automated way to verify commands work as documented
- Risk of breaking commands when updating system
- Difficult to validate all 11 commands after changes

**Solution:**
Automated testing framework:
- Validates each command against its specification
- Runs tests on every change to catch regressions
- Generates test reports showing pass/fail status

**Implementation plan:**
1. Research test frameworks (Jest for Node, pytest for Python, or custom shell-based)
2. Define test specification format (YAML/JSON schema mapping command specs to expected behavior)
3. Implement validator that:
   - Reads command specification from .claude/commands/*.md
   - Executes command with test inputs
   - Compares actual output to expected behavior
   - Reports pass/fail with detailed diff
4. Create test suite for all 11 commands
5. Integrate into CI/CD pipeline

**Status:**
- [ ] Research test framework options
- [ ] Define test specification format
- [ ] Implement command validator
- [ ] Write tests for all 11 commands
- [ ] Set up automated test execution

---

## Low Priority Ideas 🟢

### 10. AI Code Review Bot with Security Scanning

**Priority:** 🟢 Low
**Captured:** 2025-11-11
**Source:** Code reviews take 30-60 minutes, often skip due to time pressure
**Estimated effort:** 3-4 weeks (Claude API + security scanner integration)
**Impact:** Faster code reviews (30 min → 10 min), catch security issues
**ROI:** 20 min/review × 20 reviews/month = 6.7hr/month = 80hr/year saved
**Next step:** Research Claude API for code review prompts

**Problem:**
Code reviews are time-consuming:
- Manual review: Check style, logic, security, tests
- Takes 30-60 minutes per PR
- Often skipped due to time pressure (risky)

**Solution:**
AI code review bot:
- PR opened → Bot runs Claude API + security scanners
- Claude reviews: Code quality, logic bugs, test coverage
- Security scanners: Check for SQL injection, XSS, secrets in code
- Bot comments on PR: "⚠️ Potential SQL injection on line 45"
- Luca reviews bot's findings (10 minutes vs 30 minutes)

**Benefits:**
- **Faster reviews:** 30 min → 10 min
- **Catch security issues:** Automated security scanning
- **Consistent quality:** Same review standards every time

**Status:**
- [ ] Research Claude API for code review prompts
- [ ] Integrate security scanners (Bandit for Python, ESLint for JavaScript)
- [ ] Build GitLab MR webhook handler
- [ ] Test with sample PRs
- [ ] Deploy to production

---

### 11. Multi-Cloud Cost Optimizer

**Priority:** 🟢 Low
**Captured:** 2025-11-11
**Source:** AWS costs creeping up (~$500/month), not sure why
**Estimated effort:** 2-3 weeks (AWS Cost Explorer API + recommendations engine)
**Impact:** Reduce AWS costs by 20-30% (~$100-$150/month saved)
**ROI:** $1,200-$1,800/year saved
**Next step:** Research AWS Cost Explorer API

**Problem:**
AWS costs increasing but not sure why:
- Currently: $500/month (~$6,000/year)
- Likely waste: Unused EC2 instances, oversized RDS, inefficient storage
- No visibility into cost breakdown by project/client

**Solution:**
Cost optimizer dashboard:
- Daily cost breakdown (by project, service, region)
- Recommendations: "💡 Resize RDS from db.t3.large to db.t3.medium → save $50/month"
- Alerts: "⚠️ Costs up 30% this week, investigate"
- Auto-shutdown: Turn off dev/staging environments at night (save 50% on compute)

**Benefits:**
- **Cost visibility:** Know exactly where money is spent
- **Savings recommendations:** Automated cost optimization suggestions
- **Prevent surprises:** Alerts if costs spike unexpectedly

**Status:**
- [ ] Research AWS Cost Explorer API
- [ ] Build cost dashboard
- [ ] Implement recommendations engine
- [ ] Test with real AWS account
- [ ] Deploy to production

---

### 12. Blockchain-Based Time Tracking (Extreme Transparency for Clients)

**Priority:** 🟢 Low
**Captured:** 2025-11-11
**Source:** Client questioned hours billed (requested detailed breakdown)
**Estimated effort:** 4-6 weeks (blockchain integration + time tracking UI)
**Impact:** Extreme transparency → client trust → potential premium pricing
**ROI:** Hard to quantify, but could enable 10-20% price increase
**Next step:** Research public blockchains for immutable time logs

**Problem:**
Clients sometimes question hours billed:
- "Did you really work 40 hours this week?"
- Luca manually tracks time (could be fabricated)
- No proof of actual work done at claimed times

**Solution:**
Blockchain-based time tracking:
- Start task → Record timestamp on public blockchain (Ethereum/Polygon)
- Stop task → Record end timestamp on blockchain
- Blockchain = immutable proof (can't fake timestamps)
- Client can verify: "Show me the blockchain transaction ID"
- Ultimate transparency → client trust

**Benefits:**
- **Extreme transparency:** Clients can independently verify hours
- **Competitive differentiation:** "We're the only dev shop with blockchain-verified time tracking"
- **Premium pricing:** Transparency justifies higher rates

**Challenges:**
- Blockchain transaction costs (~$0.01-$0.10 per transaction on Polygon)
- Overkill for most clients (complexity vs value)
- Privacy concerns (timestamps are public)

**Status:**
- [ ] Backlog (interesting concept, but low priority - manual time tracking works fine)

---

### 13. Federated Learning for Client-Specific AI Models

**Priority:** 🟢 Low
**Captured:** 2025-11-11
**Source:** Each client has unique domain knowledge (finance, healthcare, e-commerce)
**Estimated effort:** 8-12 weeks (research + implementation)
**Impact:** AI assistant learns client-specific patterns (better code suggestions)
**ROI:** Hard to quantify, experimental
**Next step:** Research federated learning frameworks

**Problem:**
Claude Code is generic (doesn't know client-specific domain knowledge):
- Client A (healthcare): Needs HIPAA compliance patterns
- Client B (finance): Needs PCI-DSS compliance patterns
- Client C (e-commerce): Needs Stripe integration patterns
- Generic Claude doesn't have this domain knowledge

**Solution:**
Train client-specific AI models using federated learning:
- Local model per client (trained on client's codebase)
- Federated learning: Model learns locally, shares only weights (privacy-preserving)
- Client-specific suggestions: "Use HIPAA-compliant logging pattern (client A best practice)"

**Benefits:**
- **Better suggestions:** AI knows client-specific patterns
- **Privacy-preserving:** Client code never leaves their infrastructure
- **Competitive moat:** Unique client-specific AI models

**Challenges:**
- Extremely complex (cutting-edge research)
- Unclear ROI (experimental)
- May not be worth effort vs generic Claude Code

**Status:**
- [ ] Backlog (exploratory, not pursuing now)

---

## Technical Debt Tracking

### High-Priority Debt (Fix Soon)

1. **Missing database indexes on foreign keys (PostgreSQL)**
   - Impact: Queries will slow down as data grows (already at 10K+ rows)
   - Effort: 2-3 hours (add indexes, test query performance)
   - Priority: HIGH
   - Source: dev-patterns.md (PostgreSQL Index on Foreign Keys pattern)

2. **No test coverage on authentication module**
   - Impact: Risk of breaking login/signup (critical user flow)
   - Effort: 1 day (write tests for JWT token refresh, OAuth)
   - Priority: HIGH
   - Source: Recent JWT bug (forgot to test token refresh logic)

3. **Hardcoded Fibery API token in codebase (old commits)**
   - Impact: Security risk if repo leaked
   - Effort: 1 hour (move to environment variable, rotate token)
   - Priority: HIGH
   - Source: Security audit found token in git history

### Medium-Priority Debt

1. **API error responses inconsistent across endpoints**
   - Impact: Frontend error handling is messy (different error formats)
   - Effort: 4-6 hours (standardize error response structure)
   - Priority: MEDIUM
   - Source: dev-patterns.md (API Error Response Structure pattern)

2. **No rate limiting on public API endpoints**
   - Impact: Potential abuse (DDoS, scraping)
   - Effort: 2-3 hours (add rate limiting middleware)
   - Priority: MEDIUM
   - Source: Security best practices

### Low-Priority Debt

1. **Old commented-out code cluttering files**
   - Impact: Code readability (minor)
   - Effort: 1-2 hours (cleanup pass)
   - Priority: LOW
   - Source: Code review feedback

---

## Technology Evaluation

### Currently Using (Luca's Tech Stack)

**Backend:**
- Python 3.11
- FastAPI (REST APIs, async support, automatic OpenAPI docs)
- PostgreSQL 15 (primary database)
- Redis (caching, session storage)

**Frontend:**
- React 18 (TypeScript)
- Tailwind CSS (utility-first styling)
- Vite (fast dev server, optimized builds)

**Infrastructure:**
- AWS (ECS for containers, RDS for PostgreSQL, S3 for storage)
- GitLab CI/CD (automated deployments)
- Sentry (error tracking)
- CloudWatch (logs, metrics)

**Tools:**
- Claude Code (second brain, development assistant)
- Fibery (project management, client tracking)
- Slack (client communication)
- Google Drive (file storage, voice memos)

### Considering

**Monitoring & Observability:**
- Current: CloudWatch (basic, AWS-native)
- Considering: Datadog (comprehensive, $$$) vs Grafana + Prometheus (self-hosted, free)
- Decision: Start with Grafana, upgrade if needed

**Caching:**
- Current: Redis (basic key-value caching)
- Considering: Redis + cache-aside pattern for API responses
- Decision: Implement cache-aside for top 10 slow queries

**Feature Flags:**
- Current: None (risky deployments)
- Considering: LaunchDarkly (SaaS, $$$) vs django-flags (open-source, free)
- Decision: Start with django-flags, upgrade if needed

---

## Learning Goals (Technical Skills)

**This Quarter (Q1 2025):**
- [ ] Deep dive on PostgreSQL query optimization (EXPLAIN ANALYZE, indexes, query planner)
- [ ] Model Context Protocol (MCP) for Claude Code integrations
- [ ] WebSocket real-time infrastructure (Socket.io, scaling considerations)
- [ ] Whisper API for voice-to-text transcription

**Next Quarter (Q2 2025):**
- [ ] Infrastructure-as-Code with Terraform (AWS resource provisioning)
- [ ] Advanced React patterns (concurrent rendering, Suspense, Server Components)
- [ ] Claude API best practices (prompt engineering, cost optimization)
- [ ] GitLab webhook integration (event-driven automation)

**Later (Q3-Q4 2025):**
- [ ] Federated learning (if pursuing client-specific AI models)
- [ ] Rust for performance-critical microservices (if Python becomes bottleneck)
- [ ] Blockchain/smart contracts (if pursuing blockchain time tracking)

---

**Last Updated:** 2025-11-11
**Total Ideas:** 13
**Priority Breakdown:**
- 🔴 HIGH: 3 ideas (23%) - Fibery MCP, GitLab webhook, Voice-to-text
- 🟡 MEDIUM: 6 ideas (46%) - WebSocket dashboard, Test generation, IaC generator, Migration safety, Dev environment, Command testing
- 🟢 LOW: 4 ideas (31%) - Code review bot, Cost optimizer, Blockchain time tracking, Federated learning

**Next Steps:**
1. Start with Fibery MCP Server (highest ROI: 338hr/year saved)
2. Implement GitLab Webhook Integration (100% pattern capture)
3. Build Voice-to-Text Pipeline (3-4x more ideas captured)

**Revenue Potential (if productized):**
- Fibery MCP Server: $17,400/year
- GitLab Auto-Learning: $22,800/year
- Voice Idea Capture: $21,600/year
- Total potential: $61,800/year recurring (if all 3 productized)

**Related Files:**
- memory/semantic/tech/architecture-decisions.md (tech stack choices)
- memory/semantic/patterns/dev-patterns.md (development patterns)
- memory/episodic/learning/ (learning experiences with new technologies)
- projects/INDEX.md (active projects using these technologies)
