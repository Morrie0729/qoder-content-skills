---
name: content-competitor
description: "Analyze competing content on a given topic — reviews angles, structure, engagement, and gaps. Identifies differentiation opportunities. Use when user says: competitor analysis, 竞品分析, what else is out there, who wrote about this, 竞争分析, content gap."
---

# Content Competitor - Competitive Content Analyzer

Analyze existing content on a topic to find gaps, overdone angles, and differentiation opportunities before you write.

## Trigger Conditions

Activate this skill when the user:
- Wants to know what's already been written about a topic
- Needs to find a unique angle before writing
- Says keywords like: "competitor analysis", "竞品分析", "what else is out there", "who wrote about this", "content gap"

## Configuration

Read shared config from `~/.qoder/skills/content-config.json` for:
- `focus_domains` — helps narrow search scope
- `target_audience` — evaluates if competitor content serves the same audience

## Analysis Process

### Phase 1: Content Discovery

Run parallel `WebSearch` queries to find existing content:

```
"[topic]" blog post guide
"[topic]" analysis deep dive
"[topic]" tutorial how to
"[topic]" opinion take
"[topic]" site:medium.com OR site:dev.to OR site:substack.com
```

Also search for video/visual content if relevant:
```
"[topic]" site:youtube.com
```

Collect the top 8-12 most relevant results.

### Phase 2: Content Audit

For each of the top 5-8 results, use `WebFetch` to analyze:

Prompt template:
> "Analyze this article about [topic]. Extract: 1) The main angle/thesis, 2) Article structure (sections/headings), 3) Approximate word count, 4) Key data points or examples used, 5) What's unique about this piece, 6) What's missing or could be better. Also note the author, publication, and date."

### Phase 3: Pattern Analysis

Across all analyzed content, identify:

#### A. Common Angles (Overdone)
What perspectives do 3+ articles share? These are saturated — avoid unless you have genuinely better data.

#### B. Structural Patterns
- What sections appear in most articles?
- What's the typical depth level? (Surface / Medium / Deep)
- What format dominates? (Listicle / Narrative / Tutorial / Opinion)

#### C. Data & Source Overlap
- Which stats get cited repeatedly? (These are "expected" — you need fresh data to stand out)
- Who are the commonly quoted experts?

#### D. Engagement Signals
When visible, note:
- Comment counts and quality of discussion
- Social shares (if detectable)
- Whether the piece ranks on page 1 of Google for the topic

#### E. Content Gaps
Most important output — what's NOT being covered:
- Perspectives missing (technical vs. business vs. user?)
- Depth gaps (everyone is surface-level? or everyone is too technical?)
- Recency gaps (most content is 6+ months old?)
- Format gaps (no one has done a comparison? a tutorial? a contrarian take?)

## Output Format

```
## 🔍 Competitive Content Analysis: [Topic]

**Date:** [Date]
**Articles Analyzed:** [N]
**Search Queries Used:** [N]

---

### Top Competing Content

| # | Title | Author/Pub | Date | Format | Depth | Unique Aspect |
|---|-------|-----------|------|--------|-------|---------------|
| 1 | ...   | ...       | ...  | Blog   | Deep  | ...           |
| 2 | ...   | ...       | ...  | Thread | Surface | ...        |

### Saturation Map

| Angle | # of Articles | Quality | Status |
|-------|--------------|---------|--------|
| "How X works" overview | 5 | High | 🔴 Saturated |
| Comparison of X vs Y | 2 | Medium | 🟡 Moderate |
| Technical deep-dive into Z | 0 | — | 🟢 Open |
| Contrarian "X isn't that great" | 1 | Low | 🟢 Open |

### Common Elements (Expected by Readers)
Readers will expect you to cover these (but they alone won't differentiate):
1. [Common element 1]
2. [Common element 2]
3. [Common element 3]

### Content Gaps (Your Opportunities)

#### Gap 1: [Description]
**Why it's open:** [reason]
**Difficulty to fill:** Easy / Medium / Hard
**Potential impact:** High / Medium

#### Gap 2: [Description]
**Why it's open:** [reason]
**Difficulty to fill:** Easy / Medium / Hard
**Potential impact:** High / Medium

#### Gap 3: [Description]
...

### Recommended Differentiation Strategy

Based on this analysis, here's how to stand out:

**Primary angle:** [specific angle that fills the biggest gap]
**Supporting differentiators:**
1. [What to include that others don't]
2. [Unique data or perspective to add]
3. [Format or structural innovation]

**What to explicitly avoid:**
- [Overdone angle 1]
- [Overdone angle 2]
```

## User Interaction

1. **Before analysis:** Confirm the topic and ask if there's a specific format they plan to write in
2. **After analysis:** Ask if they want to:
   - Proceed to drafting with the recommended angle (-> trigger `content-drafter`)
   - Analyze a specific competitor article more deeply
   - Broaden or narrow the search scope

## Integration Notes

- Results from this skill should inform `topic-advisor`'s saturation assessment
- When used standalone, it replaces the saturation check in `topic-advisor`
- Can be triggered mid-flow: user picks a topic with `topic-advisor`, then runs this to validate the angle before committing
