# Fibery MCP Server: Capabilities & Integration Guide

**Last Updated:** 2025-11-11
**Status:** Production-ready
**MCP Setup Skill:** See `.claude/skills/mcp-setup/SKILL.md` for general MCP installation instructions

---

## Executive Summary

The Fibery MCP Server is an official bridge that connects AI assistants (Claude, ChatGPT, Cursor, etc.) directly to your Fibery workspace using the Model Context Protocol (MCP). It enables conversational data interaction, allowing AI to query databases, create tasks, update documents, and search your workspace using natural language—without writing complex API calls.

**Key Value Proposition:** Fibery's flexible, interconnected workspace structure makes its MCP server uniquely powerful compared to rigid alternatives (Atlassian, GitHub, Asana). It can become the "central nervous system" for company-wide AI initiatives.

**For general MCP setup instructions:** See `.claude/skills/mcp-setup/SKILL.md`

---

## Core Capabilities

### 1. Conversational Data Interaction

Instead of writing REST API calls, you simply ask:

**Traditional API Approach:**
```bash
# Multiple steps required
GET /api/databases/tasks
POST /api/entities with JSON payload
Parse response, handle errors
```

**MCP Approach:**
```
"Create a new bug report for login failure on iOS"
"What tasks are currently in progress for Q4 launch?"
"Give me 5 positive customer quotes about our licensing model"
```

### 2. Dynamic Schema Discovery

AI can learn your workspace structure on-the-fly:

- `schema` - High-level workspace structure (Spaces and Databases)
- `schema_detailed` - Detailed fields, relations for specific databases

**Why this matters:** Your Fibery workspace is unique. The AI adapts to *your* data model without hard-coding field names. This enables truly autonomous agents.

### 3. Entity & Document Management

**Structured Data (Entities):**
- Create single entities: `create_entity`
- Batch create: `create_entities` (major time-saver)
- Update existing: `update_entity`
- Advanced queries: `query` (with filtering, sorting, pagination)

**Unstructured Data (Documents):**
- Read documents: `get_documents_content` (Markdown format)
- Update/append: `update_document_content`

**Example workflow:** AI drafts a spec document, gets feedback, updates the document directly in Fibery. Seamless loop.

---

## Complete Tool Reference

| Tool Name | Purpose | Key Inputs | Output |
|-----------|---------|------------|--------|
| `schema` | Get workspace structure | None | JSON (Spaces, Databases) |
| `schema_detailed` | Get detailed database schema | `database_names` | JSON (fields, relations) |
| `query` | Advanced data queries | `query_dsl` | Array of entities |
| `create_entity` | Create single entity | `db_name`, `fields` | Created entity |
| `create_entities` | Batch create entities | `db_name`, `entities` | Array of entities |
| `update_entity` | Update existing entity | `db_name`, `entity_id`, `fields` | Updated entity |
| `get_documents_content` | Read Fibery documents | `document_ids` | Markdown content |
| `update_document_content` | Update/append to document | `document_id`, `content`, `mode` | Success message |
| `search_guide` | Search Fibery User Guide | `query` | Search results |

### Query DSL Example

Find all high-priority bugs from last 7 days:

```json
{
  "query_dsl": {
    "from": "Bugs",
    "select": ["name", "priority", "creation_date"],
    "where": {
      "and": [
        {"field": "priority", "op": "=", "value": "High"},
        {"field": "creation_date", "op": ">=", "value": "NOW-7D"}
      ]
    },
    "limit": 10
  }
}
```

### Natural Language → Tool Mapping

| What You Say | Tool Used | Action |
|--------------|-----------|--------|
| "Show me all databases in the workspace" | `schema` | Retrieve workspace structure |
| "What fields does the Tasks database have?" | `schema_detailed` | Get detailed schema |
| "Create a bug report for login issue" | `create_entity` | Create new entity |
| "Update task #123 to completed status" | `update_entity` | Modify entity field |
| "Show content of the Q4 strategy document" | `get_documents_content` | Read document |
| "Add summary to meeting notes doc" | `update_document_content` | Append to document |
| "Find all high-priority tasks due this week" | `query` | Advanced filtered query |

---

## Decision Tree: Choosing the Right Tool

```
Need workspace info?
├─ Yes: Use `schema` or `schema_detailed`
└─ No: Working with data?
    ├─ Create new?
    │   ├─ Single item: `create_entity`
    │   └─ Multiple items: `create_entities`
    ├─ Update existing?
    │   ├─ Structured data: `update_entity`
    │   └─ Document: `update_document_content`
    └─ Read/query?
        ├─ Complex filters: `query`
        ├─ Document content: `get_documents_content`
        └─ Help docs: `search_guide`
```

---

## Real-World Use Cases

### Use Case 1: Automated Sales Insights

**Scenario:** Sales call transcripts stored in Fibery.

**Traditional approach:** Manually read hours of transcripts.

**MCP approach:**
```
"What are the latest trends customers mention for solution X?"
"Give me 5 positive quotes about our new licensing model"
```

**Result:** AI reads documents, synthesizes trends, provides actionable intelligence in seconds.

### Use Case 2: Ad-Hoc CRM Reporting

**Scenario:** Head of sales needs monthly activity report.

**Traditional approach:** Request from analyst, wait 2-3 days.

**MCP approach:**
```
"Create activity report for Jane Doe: top activities last month
and major new opportunities created"
```

**Result:** Instant report generation. Non-technical leaders self-serve.

### Use Case 3: Team Onboarding

**Scenario:** New hire needs to learn company processes.

**Traditional approach:** Interrupt colleagues constantly.

**MCP approach:**
```
"Where's the Q4 marketing strategy documentation?"
"Who led the Phoenix project?"
"What's our code review process?"
```

**Result:** AI acts as interactive knowledge base. Faster onboarding, less disruption.

---

## Competitive Landscape

### Fibery MCP vs. Alternatives

| Criteria | Fibery | Atlassian | GitHub | Asana |
|----------|--------|-----------|--------|-------|
| **Focus** | Holistic workspace (tasks, docs, strategy, CRM) | Issue tracking (Jira, Confluence) | Code & repos | Project management |
| **Data Structure** | Highly flexible & customizable | Structured but rigid | Structured but rigid | Structured but rigid |
| **Key Differentiator** | Interconnected data model; company "nervous system" | Developer lifecycle integration | Code manipulation | Team project execution |
| **Flexibility** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐ |

**Fibery's Unique Value:** Because Fibery itself is a "build-your-own-tool" platform, its MCP server inherits that flexibility. It's not limited to one department—it can connect sales, product, and engineering data in ways specialized servers cannot.

---

## Fibery-Specific Installation

**Prerequisites:**
- Fibery account with admin access
- For general MCP setup steps, see `.claude/skills/mcp-setup/SKILL.md`

### Method 1: Remote MCP (Recommended)

1. **Navigate to Claude Settings**
   - Desktop: `Settings → Connectors → Add custom connector`

2. **Fibery-Specific Configuration:**
   - **Name:** `Fibery` (⚠️ **MUST** be exactly "Fibery" - case-sensitive!)
   - **URL:** Choose based on region:
     - EU: `https://mcp-eu-svc.fibery.io/mcp`
     - US: `https://mcp-us-svc.fibery.io/mcp`

3. **Authenticate via OAuth**
   - Follow OAuth flow to your Fibery account
   - Grant permissions
   - Done!

4. **Verify:** Ask Claude "What databases exist in my Fibery workspace?"

### Method 2: Local MCP (Development)

**Installation:**
```bash
uv tool install fibery-mcp-server
```

**Get Fibery API Token:**
1. Login: `your-domain.fibery.io`
2. Navigate: `Settings → API Keys`
3. Generate and copy token

**Claude Configuration:**
```json
{
  "mcpServers": {
    "fibery-mcp-server": {
      "command": "uv",
      "args": [
        "tool",
        "run",
        "fibery-mcp-server",
        "--fibery-host",
        "your-domain.fibery.io",
        "--fibery-api-token",
        "your-token-here"
      ]
    }
  }
}
```

Replace:
- `your-domain.fibery.io` → Your Fibery domain
- `your-token-here` → API token from above

---

## Fibery-Specific Troubleshooting

**For general MCP troubleshooting:** See `.claude/skills/mcp-setup/SKILL.md`

### Quick Fibery Diagnosis

### Fibery-Specific Connection Issues

**Required Arguments:**
```bash
--fibery-host your-domain.fibery.io
--fibery-api-token your-token
```

**Test Manual Connection:**
```bash
# Verify server can start
uv tool run fibery-mcp-server \
  --fibery-host your-domain.fibery.io \
  --fibery-api-token your-token

# Expected: Server starts without errors
```

### Fibery OAuth Issues (Remote MCP)

**Critical Requirements:**
- **Connector name:** MUST be exactly `Fibery` (case-sensitive)
- **Server URL:** Region-specific:
  - EU (most common): `https://mcp-eu-svc.fibery.io/mcp`
  - US: `https://mcp-us-svc.fibery.io/mcp`

**Common Errors:**
```bash
# ❌ Wrong - Missing /mcp endpoint
https://mcp-eu-svc.fibery.io

# ❌ Wrong - Using workspace URL
https://your-domain.fibery.io

# ✅ Correct
https://mcp-eu-svc.fibery.io/mcp
```

### Fibery API Token Issues (Local MCP)

**Token Format:**
- Must start with: `fib_`
- Example: `fib_abc123def456ghi789`
- No extra spaces when copying

**Verify Token:**
```bash
# Test token manually
curl -X POST https://your-domain.fibery.io/api/commands \
  -H "Authorization: Token fib_your_token" \
  -H "Content-Type: application/json" \
  -d '{"command":"fibery.schema/query"}'

# Expected: JSON workspace schema
# 401: Invalid/expired token
# 403: Insufficient permissions
```

**Token Permissions:**
1. Login: `your-domain.fibery.io`
2. Settings → API Keys
3. Required: Full workspace access OR specific Space access
4. If insufficient: Regenerate with correct permissions

### Fibery-Specific Debug Logging

If you need detailed logs for Fibery MCP troubleshooting:

```json
{
  "mcpServers": {
    "fibery-debug": {
      "command": "uv",
      "args": [
        "tool",
        "run",
        "fibery-mcp-server",
        "--fibery-host", "your-domain.fibery.io",
        "--fibery-api-token", "fib_...",
        "--log-level", "debug"
      ],
      "env": {
        "MCP_DEBUG": "1",
        "MCP_LOG_LEVEL": "debug"
      }
    }
  }
}
```

**Test Fibery API Connectivity:**
```bash
# Verify token and workspace access
curl -X POST https://your-domain.fibery.io/api/commands \
  -H "Authorization: Token fib_your_token" \
  -H "Content-Type: application/json" \
  -d '{"command":"fibery.schema/query"}'

# Expected: JSON response with schema data
# If error: Check token format (must start with fib_), workspace URL, network access
```

**Common Fibery-Specific Errors:**

| Error Message | Fibery-Specific Cause | Fix |
|---------------|----------------------|-----|
| `Invalid API token format` | Token doesn't start with `fib_` | Regenerate token in Fibery Settings → API |
| `Connection timeout` | Wrong workspace URL format | Must be `workspace.fibery.io` (no https://) |
| `OAuth flow failed` (Remote MCP) | Connector name not exactly "Fibery" | Change connector name to `Fibery` (case-sensitive) |
| `401 Unauthorized` | Invalid/expired Fibery token | Regenerate API token in Fibery settings |
| `Process exiting early` | Missing `--fibery-host` or `--fibery-api-token` | Add all required Fibery arguments |

**For platform issues, configuration validation, and advanced debugging:** See `.claude/skills/mcp-setup/SKILL.md`

### Contact Fibery Support

If troubleshooting doesn't resolve your issue:

**Fibery Community:** https://community.fibery.io/
**GitHub Issues:** https://github.com/Fibery-inc/fibery-mcp-server/issues

**Include in your report:**
- Fibery workspace URL (domain.fibery.io)
- Installation method (Remote vs Local MCP)
- Token format verification (starts with `fib_`)
- Full error message from logs
- Configuration file (with token redacted)

---

## Security & Permissions

### Authentication Methods

**Remote MCP:** OAuth 2.0 (industry standard, no token sharing)
**Local MCP:** API token (stored locally in config file)

### Access Control

**Current state:** Access level determined by:
- API key permissions (local setup)
- Authenticated user account (remote setup)

**Roadmap (Q4 2025+):** Granular permissions:
- Limit AI to specific databases
- Read-only access modes
- Space-level restrictions
- Admin data hiding

### Supported MCP Clients (Allowlist)

As of 2025, Fibery permits connections from:
- Claude (Desktop & Web)
- ChatGPT
- Cursor IDE
- VS Code (GitHub Copilot)

**Other clients are blocked** for security reasons. Request new clients in Fibery community forum.

---

## Advanced Features & Future Roadmap

### Community-Requested Enhancements

1. **Persistent AI Context**
   - Create "AI Contexts" database
   - Pre-load workspace info at session start
   - Eliminate re-discovery overhead

2. **Granular API Permissions**
   - Move beyond all-or-nothing access
   - Database-specific permissions
   - Read-only modes for sensitive data

### Official Roadmap (Q4 2025)

- Enhanced permission controls
- First-class integration in AI client marketplaces (one-click setup)
- Improved security for admin data
- Better MCP server performance & reliability

---

## Best Practices

### For AI Engineers

1. **Start with schema discovery:** Always begin by asking AI to explore workspace structure
2. **Batch operations:** Use `create_entities` instead of multiple `create_entity` calls
3. **Test queries incrementally:** Start with simple filters, add complexity gradually
4. **Monitor API usage:** Fibery has rate limits; design workflows accordingly

### For Product/Team Leads

1. **Structure data well:** More structured data = more powerful AI
2. **Document database schemas:** Clear field names and descriptions help AI understand context
3. **Create interconnections:** Link related databases (Tasks → Projects → Clients) for AI to traverse relationships
4. **Establish workflows:** Define standard processes AI can automate

### Data Quality Flywheel

```
Structured Data in Fibery
    ↓
More Powerful AI Insights
    ↓
Better Decisions
    ↓
More Valuable Data Created
    ↓
(cycle repeats)
```

---

## Quick Reference Commands

### Initial Setup Verification

```bash
# Ask Claude after setup:
"What databases exist in my Fibery workspace?"
"Show me the schema for the Tasks database"
"List all spaces in the workspace"
```

### Common Operations

```bash
# Create
"Create a new bug report titled 'Login fails on iOS 18' with high priority"

# Read
"Show me all tasks assigned to John that are due this week"

# Update
"Mark task #456 as completed and add completion notes"

# Query
"Find all customer feedback from the last 30 days mentioning pricing"

# Document
"Read the Q4 strategy document and summarize key initiatives"
```

---

## When to Use Fibery MCP

### ✅ Ideal Scenarios

- Managing interconnected company data (CRM + projects + strategy)
- Ad-hoc reporting without analyst dependency
- Team onboarding and knowledge discovery
- Automating repetitive data entry workflows
- Synthesizing insights from documents and structured data

### ❌ Not Ideal For

- Simple, single-purpose tools (use specialized MCP servers like GitHub)
- Highly sensitive data requiring air-gapped systems
- Real-time collaborative editing (better done directly in Fibery UI)

---

## Support & Resources

**Official Documentation:**
- Guide: https://the.fibery.io/@public/User_Guide/Guide/Fibery-MCP-Server-401
- GitHub: https://github.com/Fibery-inc/fibery-mcp-server

**Community:**
- Forum: https://community.fibery.io/
- Use cases thread: https://community.fibery.io/t/share-your-fibery-mcp-use-cases/9624

**Technical Specs:**
- MCP Protocol: https://modelcontextprotocol.io/

---

## Summary

The Fibery MCP Server transforms your flexible Fibery workspace into an AI-queryable knowledge base. By enabling natural language interaction with structured and unstructured data, it empowers teams to:

1. **Automate workflows** without writing code
2. **Generate insights** from sales calls, customer feedback, project data
3. **Self-serve reporting** for non-technical users
4. **Accelerate onboarding** with AI-powered knowledge discovery

**Getting started is simple:** Use Remote MCP for quick setup (OAuth flow) or Local MCP for development environments (requires `uv` and API token).

**The result:** Your company's collective knowledge becomes an active, intelligent asset that compounds over time.

---

**Next Steps:**

1. Install Fibery MCP Server (Remote or Local)
2. Verify connection with schema queries
3. Start with simple entity creation/queries
4. Gradually explore advanced use cases
5. Share learnings with team and Fibery community
