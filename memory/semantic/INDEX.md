# Semantic Memory - What You Know

This folder stores **permanent knowledge** - facts, patterns, principles that you've learned and apply repeatedly.

## Structure

### tech/
Technical knowledge, architecture decisions, dev patterns
- `architecture-decisions.md` - Tech stack, architectural choices
- `python-patterns.md` - Python-specific patterns
- `javascript-patterns.md` - JS/React patterns  
- `dev-patterns.md` - Cross-language development patterns

### clients/
Client-specific contexts and preferences
- `client-template.md` - Template for new clients
- `[client-name].md` - One file per client

### products/
Product visions, features, roadmaps
- `product-vision.md` - Your product strategy
- Feature lists, roadmaps, etc.

### patterns/
Cross-cutting patterns that apply to multiple domains
- `dev-patterns.md` - Development patterns
- `content-patterns.md` - Content creation patterns
- `pm-patterns.md` - Project management patterns

### ideas/
Captured ideas categorized by type
- `product-ideas.md` - Product features, improvements
- `content-ideas.md` - Blog posts, videos, social content
- `business-ideas.md` - Business strategies, pivots
- `tech-ideas.md` - Technical explorations, refactorings

## How This Grows

**Automatic:**
- `/learn` command extracts patterns from experiences
- Patterns gain confidence over time (low → medium → high)
- Cross-references to episodic experiences

**Manual:**
- Add tech documentation as you learn
- Update client contexts as you work with them
- Capture ideas with `/idea` command

## Pattern Confidence Levels

**HIGH (5+ examples):**
- Apply automatically without thinking
- Save significant time (30-45 min per application)
- Examples: "JWT tokens for mobile", "PostgreSQL indexing"

**MEDIUM (2-4 examples):**
- Apply with slight consideration
- Emerging pattern, needs more validation
- Examples: "API rate limiting", "CSV export patterns"

**LOW (1 example):**
- Mention as suggestion
- May or may not be a real pattern
- Need more data points to confirm

## Cross-References

Semantic memory ← feeds from → Episodic memory
- Experiences generate patterns
- Patterns guide future work
- Continuous learning loop
