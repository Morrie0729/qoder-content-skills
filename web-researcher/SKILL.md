---
name: web-researcher
description: "Deep web research on specific topics — collects facts, data points, expert opinions, timelines, and references from authoritative sources. Use when user says: research, 调研, dig into, 深挖, investigate, find sources, gather material, 素材收集."
---

# Web Researcher - Deep Topic Research Assistant

Conduct thorough web research on a specified topic, collecting structured materials (facts, data, opinions, references) for content creation.

## Trigger Conditions

Activate this skill when the user:
- Wants to research a specific topic in depth
- Needs to gather source material, data points, or expert quotes
- Says keywords like: "research", "调研", "dig into", "深挖", "investigate", "find sources", "gather material", "素材收集"

## Research Process

### Phase 1: Scope Definition

Before starting research, clarify with the user:
1. **Topic:** What exactly are we researching?
2. **Depth:** Quick overview (5 min) or deep dive (thorough)?
3. **Focus areas:** Technical details? Market data? Expert opinions? Historical context?
4. **Output use:** Blog post? Twitter thread? Newsletter? Internal reference?

If the user doesn't specify, default to a **deep dive for blog post use**.

### Phase 2: Multi-Layer Search Strategy

Execute searches in waves, each building on previous findings:

#### Wave 1 — Broad Discovery
Run 3-5 parallel `WebSearch` queries with different angles:
- `"[topic]" overview explanation`
- `"[topic]" latest news 2025 2026`
- `"[topic]" expert opinion analysis`
- `"[topic]" data statistics numbers`
- `"[topic]" controversy debate criticism`

#### Wave 2 — Authoritative Sources
Based on Wave 1 results, fetch the most promising sources with `WebFetch`:
- Official documentation or announcements
- Peer-reviewed papers or technical reports
- Established media coverage (major tech publications)
- Expert blog posts or analyses

For each source, use this prompt template with WebFetch:
> "Extract the key facts, data points, quotes, and arguments from this page about [topic]. Include specific numbers, dates, and attributable quotes. Note the author and publication date if available."

#### Wave 3 — Gap Filling
Review collected material and identify gaps:
- Missing counter-arguments or criticisms?
- Need more recent data?
- Missing key stakeholder perspective?

Run targeted searches to fill gaps.

#### Wave 4 — Verification
For critical claims and data points:
- Cross-reference with a second independent source
- Check if data is from a primary source or secondary reporting
- Verify dates and attribution accuracy

### Phase 3: Source Credibility Assessment

Rate each source on a 3-tier scale:

- **Tier 1 — Primary Source** 🟢
  Official announcements, original research, firsthand accounts, raw data
  
- **Tier 2 — Quality Secondary** 🟡
  Major publication coverage, expert analysis with cited sources, well-researched articles
  
- **Tier 3 — Reference Only** 🔴
  Social media posts, opinion pieces without citations, aggregator articles
  Note: Still useful for gauging sentiment, but shouldn't be cited as facts

### Phase 4: Material Synthesis

Organize collected material into structured categories:

1. **Key Facts** — Verified, citable facts about the topic
2. **Data Points** — Specific numbers, statistics, metrics
3. **Expert Quotes** — Attributable quotes from named experts/organizations
4. **Timeline** — Chronological sequence of key events
5. **Arguments For** — Supporting positions and their evidence
6. **Arguments Against** — Criticisms, risks, counter-arguments
7. **Open Questions** — Unresolved debates, missing data, areas of uncertainty

## Output Format

```
## 🔍 Research Report: [Topic]

**Research Date:** [Date]
**Depth Level:** Quick Overview / Deep Dive
**Sources Consulted:** [N] sources across [N] platforms

---

### Executive Summary
[3-5 sentence overview of the topic, key findings, and why it matters now]

---

### Key Facts
1. **[Fact]** — [Source, Tier 🟢/🟡]
2. **[Fact]** — [Source, Tier 🟢/🟡]
3. ...

### Data & Statistics
| Metric | Value | Source | Tier |
|--------|-------|--------|------|
| ...    | ...   | ...    | 🟢   |

### Expert Perspectives
> "[Quote]"
> — [Name], [Title/Organization] ([Source link])

> "[Quote]"
> — [Name], [Title/Organization] ([Source link])

### Timeline
| Date | Event | Source |
|------|-------|--------|
| ...  | ...   | ...    |

### Arguments & Perspectives

**Supporting Arguments:**
1. [Argument] — [Evidence] — [Source]
2. ...

**Counter-Arguments / Criticisms:**
1. [Argument] — [Evidence] — [Source]
2. ...

### Open Questions & Gaps
- [What's still unknown or debated]
- [Where data is insufficient]

### Full Source Bibliography
| # | Title | URL | Type | Tier | Key Takeaway |
|---|-------|-----|------|------|-------------|
| 1 | ...   | ... | Article | 🟢 | ...    |
| 2 | ...   | ... | Blog | 🟡 | ...       |
```

## User Interaction

1. **During research:** If the user is present, surface interesting findings as they emerge and ask if they want to adjust the research direction
2. **After report:** Ask if they want to:
   - Go deeper on any specific section
   - Research a related subtopic
   - Save the report to a file for later reference
   - Get content angle suggestions based on findings (→ trigger `topic-advisor`)

## Quality Standards

- **Never fabricate** URLs, quotes, or data points. Every piece of information must come from actual WebSearch/WebFetch results
- **Always attribute** — no orphan facts without a source
- **Distinguish** between facts, opinions, and speculation in the report
- **Date-stamp** the research — web information changes rapidly
- **Prefer recency** — when two sources conflict, prefer the more recent one unless the older source is more authoritative

## Performance Notes

- Run parallel WebSearch queries in Wave 1 to save time
- Use WebFetch selectively — only on the most promising URLs, not every search result
- For very broad topics, ask the user to narrow the scope before diving deep
- Target 8-15 quality sources for a deep dive, 3-5 for a quick overview
