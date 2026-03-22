---
name: topic-advisor
description: "Analyze trending topics and generate content topic recommendations with angle analysis, audience targeting, and differentiation strategies. Use when user says: 选题, topic ideas, what should I write, content angles, 内容方向, pick a topic."
---

# Topic Advisor - Content Topic Selection Strategist

Analyze trending data or user-specified themes to generate actionable content topic recommendations with differentiation angles.

## Trigger Conditions

Activate this skill when the user:
- Wants help deciding what to write about
- Has trending data and needs content angles
- Says keywords like: "选题", "topic ideas", "what should I write about", "content angles", "内容方向", "pick a topic"

## Configuration

Read shared config from `~/.qoder/skills/content-config.json` for:
- `target_audience` — skip asking if already configured
- `preferred_content_formats` — default platform preference
- `focus_domains` — narrow candidate topic scope

## Topic Selection History

Before recommending, check past selections:
1. Read files from `~/.qoder/data/content/topic-selections/`
2. If a candidate topic was previously selected, flag it:
   - **Previously selected** — show date, ask if user wants to revisit or skip
   - **Selected but never drafted** — highlight as "unfinished business"
3. After user confirms a topic, save the selection:
   - File: `~/.qoder/data/content/topic-selections/[YYYY-MM-DD]-[slug].json`
   - Format: `{"topic": "...", "angle": "...", "platform": "...", "status": "selected", "date": "..."}`

## Input Sources

This skill works with two types of input:

### A. Trending Data (from trending-hunter)
If the user has already run `trending-hunter`, use that report as the primary input. Focus on cross-platform hot topics first.

### B. User-Specified Theme
If the user provides a specific domain, keyword, or theme, use `WebSearch` to gather current discussions and content around that topic.

## Analysis Framework

For each candidate topic, evaluate across these 5 dimensions:

### 1. Timeliness Score (1-5)
- **5**: Breaking news, happened in last 24h
- **4**: This week's hot topic, still actively discussed
- **3**: Ongoing trend, relevant for 1-2 weeks
- **2**: Evergreen adjacent — can be tied to current events
- **1**: Purely evergreen, no time pressure

### 2. Content Saturation Check
Use `WebSearch` to check existing coverage:
- Search: `"[topic keyword]" article/guide/analysis`
- Count high-quality results on page 1
- **Low saturation** (0-3 strong pieces): Blue ocean, high opportunity
- **Medium saturation** (4-7 strong pieces): Needs a clear differentiator
- **High saturation** (8+): Avoid unless you have a unique angle

### 3. Audience Fit Assessment
Ask the user about their target audience (if not already known), then evaluate:
- Does this topic match the audience's interests?
- What's the knowledge level required? (Beginner / Intermediate / Expert)
- Is there emotional resonance? (curiosity, fear, excitement, controversy)

### 4. Differentiation Angles
For each topic, brainstorm 2-3 unique angles that haven't been well-covered:

Angle types to consider:
- **Contrarian**: Challenge the popular opinion with evidence
- **First-person**: Share unique personal experience or data
- **Deep-dive**: Go deeper than surface-level coverage
- **Comparison**: Compare competing approaches/tools/ideas
- **Tutorial**: Practical how-to that existing coverage lacks
- **Prediction**: Forward-looking analysis of where this trend goes
- **Behind-the-scenes**: Explain the mechanics others skip
- **Curated synthesis**: Aggregate and synthesize scattered information

### 5. Platform Suitability
Evaluate which platforms this topic works best for:
- **Long-form** (blog, Medium, newsletter): Complex analysis, tutorials
- **Short-form** (Twitter/X threads): Hot takes, quick insights
- **Community** (Reddit, HN): Discussion-oriented, technical depth
- **Visual** (YouTube, Instagram): Demos, infographics, walkthroughs

### 6. SEO Signal Check
Use `WebSearch` to assess search viability:
- Search the candidate keyword — count total results (approximate competition level)
- Check if page 1 is dominated by high-authority sites (hard to rank) or mixed (opportunity)
- Look for "People also ask" style related queries — these indicate search demand and content angles
- Rate: **SEO Viable** (searchable, not dominated) / **SEO Challenging** (high competition) / **SEO Irrelevant** (no search demand — social-first topic only)

## Process

### Step 1: Gather Candidate Topics
- If from trending-hunter: Take top 10 cross-platform topics + top 5 per category
- If user-specified: Use WebSearch to find 8-12 related trending subtopics

### Step 1b: History Dedup
Check `~/.qoder/data/content/topic-selections/` for past selections.
- If a candidate was previously selected AND drafted → skip or deprioritize
- If previously selected but NOT drafted → flag as "unfinished — revisit?"

### Step 2: Quick Filter
Eliminate topics that are:
- Already oversaturated with identical angles
- Too niche for the user's audience size goals
- Require expertise the user doesn't have

### Step 3: Deep Analysis (Top 5)
Apply the full 5-dimension framework to the remaining top candidates.

### Step 4: Generate Recommendations
Create 3 ranked recommendations, each with a complete brief.

## Output Format

```
## 📝 Topic Recommendations — [Date]

### 🥇 Top Pick: [Topic Title]

**Why this topic:**
[1-2 sentences on why this is the best pick right now]

**Recommended angle:** [Angle type] — [specific angle description]
**Timeliness:** ⭐⭐⭐⭐⭐ (5/5) — [reason]
**Saturation:** Low/Medium — [what exists, what's missing]
**Target audience:** [who, what level]
**Best platform:** [platform] — [why]
**Suggested title options:**
1. "[Title option 1]"
2. "[Title option 2]"
3. "[Title option 3]"
**Key points to cover:**
- [point 1]
- [point 2]
- [point 3]
**Reference links:**
- [url 1] — [what to reference from it]
- [url 2] — [what to reference from it]

---

### 🥈 Runner Up: [Topic Title]
[Same structure as above]

---

### 🥉 Backup Pick: [Topic Title]
[Same structure as above]

---

### 📊 Decision Matrix

| Topic | Timeliness | Saturation | Audience Fit | Differentiation | Platform Fit | SEO | Overall |
|-------|-----------|------------|-------------|----------------|-------------|-----|---------|
| Pick 1 | 5 | Low | High | Strong | Blog | Viable | ⭐⭐⭐⭐⭐ |
| Pick 2 | 4 | Medium | High | Medium | Twitter | Irrelevant | ⭐⭐⭐⭐ |
| Pick 3 | 3 | Low | Medium | Strong | Blog | Viable | ⭐⭐⭐⭐ |
```

## User Interaction

1. **Before analysis:** Ask about target audience and content goals if not already known
2. **After recommendations:** Ask if they want to:
   - Proceed with deeper research on the chosen topic (→ trigger `web-researcher`)
   - See more angle variations for a specific topic
   - Re-analyze with different criteria or domain focus

## Notes

- Always provide actionable, specific angles — not generic "write about X"
- Title suggestions should be concrete and platform-appropriate
- Reference links must be real URLs discovered via WebSearch/WebFetch, never fabricated
- If the user's niche is specific, tailor the saturation check to that niche rather than general web
