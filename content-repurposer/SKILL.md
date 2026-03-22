---
name: content-repurposer
description: "Transform content from one format to multiple platform-specific versions. Turns a blog post into Twitter threads, newsletters, Reddit posts, and more. Use when user says: repurpose, 改写, multi-platform, 分发, adapt for, cross-post, 多平台."
---

# Content Repurposer - Multi-Platform Content Adapter

Transform a single piece of content into optimized versions for different platforms, preserving the core message while adapting format, tone, and length.

## Trigger Conditions

Activate this skill when the user:
- Has a finished piece and wants versions for other platforms
- Wants to maximize content reach across platforms
- Says keywords like: "repurpose", "改写", "multi-platform", "分发", "adapt for", "cross-post", "多平台"

## Configuration

Read shared config from `~/.qoder/skills/content-config.json` for:
- `preferred_content_formats` — default target platforms
- `target_audience` — adjusts adaptation style
- `language` — output language

## Input

Accepts:
1. A draft file path (from `~/.qoder/data/content/drafts/`)
2. Pasted content in the conversation
3. A URL to fetch and repurpose (via `WebFetch`)

## Platform Adaptation Rules

### Source → Twitter/X Thread

**Transformation strategy:**
- Extract the single most compelling insight as the hook tweet
- Each tweet = one self-contained idea (280 chars max)
- Target 8-12 tweets for a blog-length source
- Convert data points into punchy, tweetable stats
- Replace nuance with clarity — threads reward bold statements
- End with a CTA tweet

**What to cut:**
- Background context (readers can click through for that)
- Hedging language ("it seems", "perhaps", "arguably")
- Citations (save for the source link)

**What to add:**
- Thread numbering (1/, 2/, ...)
- Line breaks for readability
- A "Source/full article" link in the last tweet

### Source → Newsletter Excerpt

**Transformation strategy:**
- Lead with a personal hook ("I spent 3 hours reading about X so you don't have to")
- Summarize the core argument in 3-4 paragraphs
- Add personal opinion layer — what's YOUR take?
- Include 2-3 curated links (the original + related resources)
- Conversational tone, write like you're emailing a smart friend

**What to cut:**
- Detailed technical sections (link out instead)
- Excessive data tables

**What to add:**
- Personal perspective and editorial voice
- "Why I think this matters" section
- Reader question to prompt replies

### Source → Reddit Post

**Transformation strategy:**
- Rewrite title to be descriptive, not promotional
- Open with the key finding/insight, not background
- Use Reddit markdown formatting
- Include a TL;DR at the top for long posts
- End with a genuine discussion question
- If cross-posting, respect subreddit rules

**What to cut:**
- Any promotional language or CTAs
- Self-congratulatory framing
- "Subscribe to my newsletter" type closings

**What to add:**
- TL;DR section
- Honest framing ("I wrote about X, here's what I found")
- Specific discussion question

### Source → LinkedIn Post

**Transformation strategy:**
- Start with a hook line (separated by line break)
- Short paragraphs (1-2 sentences each)
- Professional but not corporate tone
- Include a personal story or lesson learned
- Limit to 1300 characters for optimal engagement (before "see more")
- End with a question to drive comments

**What to cut:**
- Technical jargon (unless your audience is technical)
- Lengthy analysis sections

**What to add:**
- Professional framing and relevance to work/career
- Line breaks between paragraphs for mobile readability

### Source → Short Summary (for bookmarking/archives)

**Transformation strategy:**
- 3-5 bullet points capturing the key takeaways
- Include metadata: topic, date, source, key data points
- Useful for personal knowledge management

## Process

### Step 1: Analyze Source
Read the source content and identify:
- Core thesis / main argument
- Top 3-5 key insights
- Best data points and quotes
- The emotional hook
- Target platforms for adaptation

### Step 2: Ask User Preferences
If not specified, ask:
- Which platforms to target? (show all options with recommendations)
- Should each version link back to the original?
- Any platform-specific requirements? (subreddit rules, hashtags, etc.)

### Step 3: Generate Adaptations
For each target platform, generate the adapted version following the rules above.

### Step 4: Present All Versions
Show all versions with clear platform headers. For each version, include:
- Platform name and format
- Estimated reading time / tweet count
- Character count (relevant for Twitter, LinkedIn)

## Output Format

```
## 🔄 Repurposed Content: "[Original Title]"

---

### 🐦 Twitter/X Thread ([N] tweets)

1/ [Tweet 1]

2/ [Tweet 2]

...

---

### 📧 Newsletter Version (~[N] words)

Subject: [Subject line]

[Newsletter content]

---

### 🤖 Reddit Post

**Title:** [Reddit-optimized title]

[Reddit content]

---

### 💼 LinkedIn Post ([N] characters)

[LinkedIn content]

---

### 📌 Summary (for archives)

- [Key point 1]
- [Key point 2]
- [Key point 3]
```

## Saving Outputs

Save each version to `~/.qoder/data/content/drafts/` with format suffix:
```
[date]-[slug]-twitter.md
[date]-[slug]-newsletter.md
[date]-[slug]-reddit.md
[date]-[slug]-linkedin.md
```

## Quality Checks

- Each version must stand alone — a reader seeing only that version should get full value
- No version should read like a "shortened copy" — each should feel native to its platform
- Verify character limits (Twitter 280/tweet, LinkedIn 3000 but optimal <1300 for visibility)
- Cross-check that no data points or attributions were lost in the adaptation
