---
name: content-drafter
description: "Generate content drafts based on research materials and topic briefs. Supports multiple formats: blog posts, Twitter threads, newsletters, Reddit posts. Use when user says: write draft, 写初稿, draft it, generate content, 写文章, start writing."
---

# Content Drafter - Multi-Format Content Generator

Generate polished first drafts from research materials, topic briefs, or raw ideas. Adapts structure, tone, and length to the target platform.

## Trigger Conditions

Activate this skill when the user:
- Has chosen a topic and wants to start writing
- Has research materials and needs a structured draft
- Says keywords like: "write draft", "写初稿", "draft it", "generate content", "写文章", "start writing"

## Configuration

Read shared config from `~/.qoder/skills/content-config.json` for:
- `target_audience` — adjusts vocabulary and depth
- `preferred_content_formats` — default format if user doesn't specify
- `language` — output language

## Input Requirements

Before drafting, gather these inputs (from previous skills or user):

1. **Topic & angle** — from `topic-advisor` output or user specification
2. **Research materials** — from `web-researcher` report, or check `~/.qoder/data/content/research-reports/` for saved reports
3. **Target format** — ask if not specified
4. **Tone** — ask if not specified (options below)

## Supported Formats & Templates

### 1. Blog Post (1500-3000 words)

Structure:
```
# [Title — hook + keyword]

[Opening hook — 2-3 sentences that create curiosity or state a surprising fact]

[Context paragraph — why this matters now, who should care]

## [Section 1 — Setup / Background]
[Establish the foundation. Use data from research.]

## [Section 2 — Core Argument / Analysis]
[Main content. This is the meat — insights, analysis, examples.]

## [Section 3 — Implications / How-to / Comparison]
[Practical takeaway or deeper exploration]

## [Section 4 — What's Next / Prediction] (optional)
[Forward-looking perspective]

## Key Takeaways
- [Takeaway 1]
- [Takeaway 2]
- [Takeaway 3]

---
*[CTA — subscribe, follow, comment prompt]*
```

### 2. Twitter/X Thread (8-15 tweets)

Structure:
```
🧵 1/ [Hook tweet — the most compelling insight or contrarian take. Must stand alone.]

2/ [Context — why this matters right now]

3-N/ [Body tweets — one idea per tweet, each self-contained but building on previous]
    - Use specific numbers and data
    - Include examples
    - Break complex ideas into simple analogies

N-1/ [Summary / key lesson]

N/ [CTA — retweet prompt, follow prompt, or question to drive replies]

Formatting rules:
- Max 280 chars per tweet
- Use line breaks within tweets for readability
- Emojis sparingly — at thread start and key transitions
- No hashtags in the middle of tweets, only in the last tweet
```

### 3. Newsletter (800-1500 words)

Structure:
```
Subject: [Compelling subject line — curiosity gap or specific benefit]
Preview text: [First 90 chars shown in inbox]

Hey [reader],

[Personal opening — 1-2 sentences connecting to the topic casually]

## The Big Idea
[Core insight in 2-3 paragraphs]

## Why It Matters
[Practical implications, who's affected]

## What I Think
[Personal perspective — this is what makes newsletters valuable]

## Links & Resources
- [Curated link 1] — [one-line annotation]
- [Curated link 2] — [one-line annotation]

Until next time,
[Sign-off]
```

### 4. Reddit Post

Structure:
```
Title: [Descriptive, not clickbaity — Reddit hates clickbait]

[Opening — state your point or question clearly in the first paragraph]

[Body — detailed explanation, personal experience if applicable]
- Use markdown formatting
- Include relevant data/sources
- Be honest about what you don't know

[Discussion prompt — genuine question to invite comments]

Tone: Conversational, authentic, slightly self-deprecating. Never promotional.
```

## Tone Profiles

| Tone | Description | Best For |
|------|-------------|----------|
| **Analytical** | Data-driven, objective, structured | Technical blogs, deep dives |
| **Conversational** | Casual, first-person, relatable | Newsletters, Reddit |
| **Authoritative** | Expert voice, confident, concise | Industry analysis, LinkedIn |
| **Provocative** | Contrarian, challenges assumptions | Twitter threads, opinion pieces |
| **Tutorial** | Step-by-step, patient, clear | How-to guides, documentation |

## Drafting Process

### Step 1: Outline
Before writing, present an outline to the user:
- 4-6 main sections with 1-sentence descriptions
- Proposed title options (3)
- Estimated length
- Ask for approval before proceeding

### Step 2: Draft
Write the full draft incorporating:
- Research data and quotes (properly attributed)
- Platform-specific formatting
- The chosen tone throughout
- Strong opening hook
- Clear transitions between sections
- Actionable or thought-provoking conclusion

### Step 3: Self-Review Checklist
Before presenting the draft, verify:
- [ ] Hook in the first 2 sentences
- [ ] No unattributed claims — all data has sources
- [ ] Consistent tone throughout
- [ ] Each section adds new value (no filler)
- [ ] CTA or discussion prompt at the end
- [ ] Within target word count for the format
- [ ] No fabricated URLs or quotes

### Step 4: Present & Iterate
Show the draft and ask:
- "What sections need expansion or trimming?"
- "Is the tone right?"
- "Want me to adjust the angle?"

## Saving Drafts

Save drafts to `~/.qoder/data/content/drafts/[date]-[slug].md` with frontmatter:
```yaml
---
title: "Draft Title"
format: blog/twitter/newsletter/reddit
topic: "Topic keyword"
status: draft
created: YYYY-MM-DD
---
```

## Quality Standards

- Never pad content with filler phrases ("In today's fast-paced world...")
- Every paragraph must earn its place — if it doesn't add insight, cut it
- Use specific examples over vague generalizations
- Data and quotes from research materials are the backbone — opinions alone are insufficient
- Match the real voice patterns of the target platform (Twitter is punchy, newsletters are personal, Reddit is authentic)
