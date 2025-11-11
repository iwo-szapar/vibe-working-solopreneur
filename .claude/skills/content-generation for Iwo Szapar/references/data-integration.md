# Data Integration

How to incorporate data sources into content for credibility and specificity.

## Data Source Hierarchy

**Priority order** (use most specific available):

1. **Your own project data** (highest credibility)
   - AI Maturity Index assessments (if available)
   - Platform analytics (Google Analytics)
   - Customer/user data
   - Internal metrics

2. **Published research** (cite sources)
   - Academic studies (peer-reviewed)
   - Industry reports (Gartner, McKinsey, etc.)
   - Government data (census, labor stats)

3. **Platform data** (publicly available)
   - LinkedIn research reports
   - Twitter/X trending data
   - YouTube insights

4. **Industry observations** (your expertise)
   - Patterns from client work
   - Conference insights
   - Conversations with peers

## AI Maturity Index Data (When Available)

### Database Access

**Tool**: `mcp__postgresql__query`

**Key tables**:
- `AssessmentSessionChatAimiReport` (aimi_report) - AI maturity scores
- `AssessmentSessionChat` (session_chat) - User conversations
- `GrowthGuide` (growth_guide) - Personalized recommendations
- `user` (user_profile) - Demographics

**Critical**: Always use `aimi_report.country` (NOT `user_profile.country`)

### Common Queries

**Geographic distribution**:
```sql
SELECT
  aimi_report.country,
  COUNT(*) as assessment_count
FROM "AssessmentSessionChatAimiReport" aimi_report
WHERE aimi_report.country IS NOT NULL
GROUP BY aimi_report.country
ORDER BY assessment_count DESC
LIMIT 10;
```

**Maturity scores by dimension**:
```sql
SELECT
  AVG(score_productivity)::NUMERIC(10,2) as avg_productivity,
  AVG(score_collaboration)::NUMERIC(10,2) as avg_collaboration,
  AVG(score_innovation)::NUMERIC(10,2) as avg_innovation
FROM "AssessmentSessionChatAimiReport" aimi_report
WHERE score_productivity IS NOT NULL;
```

**Industry benchmarking**:
```sql
SELECT
  user_profile.industry,
  AVG(aimi_report.overall_ai_maturity)::NUMERIC(10,2) as avg_maturity,
  COUNT(*) as sample_size
FROM "AssessmentSessionChatAimiReport" aimi_report
JOIN "AssessmentSessionChat" session_chat ON aimi_report.chat_id = session_chat.id
JOIN "user" user_profile ON session_chat.user_id = user_profile.id
WHERE user_profile.industry IS NOT NULL
GROUP BY user_profile.industry
HAVING COUNT(*) > 100
ORDER BY avg_maturity DESC;
```

### Data Presentation

**Always include**:
- **Sample size**: "250K assessments" or "Analysis of 4,287 users"
- **Geographic scope**: "across 75 countries" or "in North America"
- **Time period**: "over past 12 months" or "between Jan-June 2024"

**Examples**:
- ✅ "Our data from 250K assessments across 75 countries shows..."
- ✅ "Analysis of 4,287 enterprise users (2023-2024) revealed..."
- ❌ "Our data shows..." (too vague)
- ❌ "Most users..." (no numbers)

### Data Specificity Levels

**Level 1: Exact numbers** (best)
- "42% of executives overestimate their AI maturity by 2+ levels"
- "Teams using AI daily save 3.2 hours per week on average"

**Level 2: Ranges** (good)
- "Between 40-45% of users report..."
- "Productivity gains range from 2.5-4 hours weekly"

**Level 3: Directional** (acceptable if no precise data)
- "Majority of executives overestimate their maturity"
- "Significant productivity gains observed"

**Level 4: Anecdotal** (use sparingly)
- "I've noticed executives tend to..."
- "In my experience..."

**Rule**: Use Level 1 or 2 whenever possible. Level 3-4 only when data unavailable.

## Google Analytics Data (When Available)

### Tool Access

**Tool**: `mcp__google-analytics__run_report`

**Metrics to track**:
- Newsletter open rates
- Click-through rates
- AIMI assessment starts from content
- Geographic performance
- Content performance by topic

### Common Reports

**Newsletter performance**:
- Open rate trends
- Click-to-assessment rate
- Geographic distribution of opens

**Content attribution**:
- Which LinkedIn posts drive most traffic
- Which topics convert best
- Time of day performance

### Data Usage in Content

**Performance claims**:
- "This approach drove 40% increase in opens (measured over 12 weeks)"
- "Content about X converts 2.3× better than Y (data from 150K sessions)"

**Geographic insights**:
- "Our European audience engages 35% more with strategic content"
- "Asia-Pacific readers prefer tactical how-to guides"

## External Research Integration

### Finding Credible Sources

**Academic research**:
- Google Scholar
- ResearchGate
- University publications

**Industry reports**:
- McKinsey, BCG, Bain reports
- Gartner, Forrester research
- LinkedIn reports

**Government data**:
- Bureau of Labor Statistics
- Census data
- International organizations (OECD, World Bank)

### Citation Standards

**Always include**:
- Source name
- Publication year
- Study size (if applicable)
- Link (in comments or description)

**Examples**:
- "Stanford study (2024, n=5,000) found that..."
- "According to Gartner (2024), 60% of enterprises..."
- "McKinsey research across 1,200 companies shows..."

**Format in content**:
```
Remote teams adopt AI 40% faster than co-located teams.

This finding comes from Stanford's 2024 study of 5,000 knowledge workers across 12 industries. [Link in comments]
```

### Combining Data Sources

**Triangulation** (strongest approach):
- Your data + external research + industry observations

**Example**:
```
Our 250K assessments show remote teams scoring 40% higher in AI adoption.

This aligns with Stanford's 2024 study (n=5,000) showing similar patterns.

And it matches what I've observed across 15 countries: Geographic distribution correlates with remote work adoption.
```

## Personal Experience Data

### What Counts as Data

**Acceptable**:
- Number of countries lived in: "15 countries"
- Years in industry: "10+ years in AI consulting"
- Projects completed: "Assessed 50+ companies"
- Specific roles: "As Saudi AI Advisor..."

**Not acceptable** (fabrication):
- Family situations
- Private conversations
- Unnamed "clients" or "friends"
- Invented statistics

### Framing Personal Experience

**With authority**:
- "Living in 15 countries taught me..."
- "After assessing 50+ companies, I've noticed..."
- "In my role as Saudi AI Advisor, I've observed..."

**With humility**:
- "This is my experience, but data might differ"
- "I've seen this pattern, though sample size is limited"
- "Could be confirmation bias, but..."

### Combining Personal + Data

**Strongest structure**:
1. Start with personal observation
2. Support with data
3. Generalize to broader insight

**Example**:
```
Living in 15 countries, I noticed something odd about AI adoption.

Remote work cultures (Scandinavia, parts of Asia) embraced AI faster.

Our data confirms it: 250K assessments show remote teams score 40% higher in AI maturity.

The pattern? Geographic distance breeds digital-first mindset.
```

## Data Visualization

### When to Include Visuals

**Use charts/graphs for**:
- Comparison data (bar charts)
- Trends over time (line graphs)
- Distribution (pie charts sparingly)
- Complex data that's hard to summarize

**Skip visuals for**:
- Simple statistics (just state the number)
- Single data points
- When text is clearer

### Visual Guidelines

**Chart types**:
- Bar chart: Comparing categories
- Line graph: Trends over time
- Scatter plot: Correlations
- Heat map: Geographic or matrix data

**Design rules**:
- Clear labels
- Readable font size
- Contrasting colors
- Source citation on image

### Screenshot Best Practices

**For AIMI dashboards**:
- Crop to relevant data
- Highlight key insight (circle/arrow)
- Remove sensitive info (names, emails)
- High resolution (at least 1080px wide)

**Formatting**:
- PNG format (not JPEG for text clarity)
- White or neutral background
- Consistent styling across screenshots

## Data Credibility Signals

### What Builds Trust

**Strong signals**:
- Specific numbers: "42%" not "many"
- Large sample sizes: "250K assessments" not "lots of data"
- Geographic diversity: "75 countries" not "global"
- Time frames: "past 12 months" not "recently"
- Source citation: "Stanford (2024)" not "research shows"

**Weak signals**:
- Vague quantities: "most", "many", "often"
- Unnamed sources: "studies show", "experts say"
- Round numbers without context: "50%" (is this precise or estimated?)
- No sample size: "Users report..."

### Avoiding Data Manipulation

**Don't do**:
- Cherry-pick data that supports narrative (show full picture)
- Misrepresent sample size (be accurate)
- Extrapolate beyond data's scope
- Confuse correlation with causation
- Use outdated data without noting

**Do instead**:
- Show full context (even if it complicates narrative)
- Cite limitations: "Sample limited to tech industry"
- Note correlation vs causation explicitly
- Update data regularly
- Admit when data is unclear

## Data Storytelling

### Structure

**1. Start with surprising data**:
"42% of executives overestimate their AI maturity by 2+ levels."

**2. Add context**:
"We analyzed 250K assessments across 75 countries over 18 months."

**3. Explain what it means**:
"This means nearly half of leaders think they're further along than they are."

**4. Show implications**:
"It's why AI pilots fail. Leaders allocate resources based on inflated self-assessment."

**5. Make it actionable**:
"Before your next AI initiative, get an external assessment."

### Making Data Memorable

**Use comparisons**:
- "That's like thinking you're fluent in Spanish after Duolingo's first lesson"
- "Equivalent to a marathon runner thinking they're ready for ultramarathons"

**Use concrete examples**:
- "A VP rated themselves 8/10. External assessment: 4/10."
- "Company spent $500K on tools matched to level 8. Actual level: 4."

**Use visualization language**:
- "Picture a bell curve. Most executives think they're in the top 20%. They're not."

## Quick Reference

### Data Checklist

Before using any data point:
- [ ] Is it specific? (numbers, not "many")
- [ ] Is sample size stated? ("250K assessments")
- [ ] Is geographic scope clear? ("75 countries")
- [ ] Is time period noted? ("past 12 months")
- [ ] Is source cited? (internal data, Stanford 2024, etc.)
- [ ] Is it recent? (<2 years old)
- [ ] Is context provided? (what was measured, how)

### Common Data Mistakes

**Mistake**: "Most people..."
**Fix**: "62% of users in our sample of 250K..."

**Mistake**: "Research shows..."
**Fix**: "Stanford study (2024, n=5,000) found..."

**Mistake**: "AI increases productivity"
**Fix**: "Users save avg 3.2 hours weekly (n=15,487, 2024 data)"

**Mistake**: Using data from 2019
**Fix**: Find recent data or note: "2019 data (latest available)"

### Data Sources Quick List

**When you need**:
- **AI adoption stats** → AIMI database, Gartner, McKinsey
- **Productivity data** → AIMI database, BLS, academic studies
- **Geographic trends** → AIMI country data, World Bank
- **Industry benchmarks** → AIMI industry segmentation, Forrester
- **Tool usage** → AIMI assessments, LinkedIn reports
- **Remote work** → Buffer reports, Gallup, BLS

**Always prioritize**: Your own project data > Published research > Industry observations
