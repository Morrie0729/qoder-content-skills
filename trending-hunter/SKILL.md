---
name: trending-hunter
description: "Crawl and aggregate trending topics from English platforms (HackerNews, Reddit, Twitter/X, ProductHunt, GitHub Trending). Use when user says: trending, 热点, what's hot, 趋势扫描, scan trends, hot topics, buzz."
---

# Trending Hunter - Multi-Platform Trend Aggregator

Crawl, aggregate, and analyze trending topics across major English-language platforms to surface content opportunities.

## Trigger Conditions

Activate this skill when the user:
- Asks about current trending topics or hot news
- Wants to scan what's popular across platforms
- Says keywords like: "trending", "热点", "what's hot", "趋势扫描", "scan trends", "热榜"

## Data Sources & Crawling Strategy

Use the following sources in order of reliability. For each source, use `WebFetch` or `WebSearch` as specified.

### Tier 1 — High Signal Sources

1. **HackerNews**
   - Use `WebFetch` on `https://news.ycombinator.com/` with prompt: "Extract the top 15 stories with titles, points, comment counts, and URLs. Format as a numbered list."
   - Also check `https://news.ycombinator.com/newest` for rising stories

2. **Reddit Popular**
   - Use `WebSearch` with query: `site:reddit.com popular today` or specific subreddits relevant to the user's domain
   - Use `WebFetch` on `https://old.reddit.com/r/popular/` with prompt: "Extract the top 15 posts with titles, subreddit names, upvote counts, and comment counts."

3. **GitHub Trending**
   - Use `WebFetch` on `https://github.com/trending` with prompt: "Extract the trending repositories with names, descriptions, stars today, language, and total stars."
   - Also check `https://github.com/trending?since=weekly` for weekly trends

### Tier 2 — Discovery Sources

4. **ProductHunt**
   - Use `WebFetch` on `https://www.producthunt.com/` with prompt: "Extract today's top 10 launched products with names, taglines, upvote counts, and categories."

5. **Lobsters** (tech community)
   - Use `WebFetch` on `https://lobste.rs/` with prompt: "Extract the top 15 stories with titles, points, comment counts, and tags."

### Tier 3 — Broad Trend Signals

6. **Google Trends**
   - Use `WebSearch` with query: `trending topics today` or `trending [user's domain] this week`

7. **Twitter/X Trends**
   - Use `WebSearch` with query: `twitter trending topics today` or `site:twitter.com trending`
   - Note: Direct crawling of X may be limited; rely on WebSearch aggregation

## Processing Pipeline

After crawling, process the raw data through these steps:

### Step 1: Categorize
Classify each topic into one or more categories:
- **Tech/Engineering** — programming, tools, infrastructure
- **AI/ML** — models, research, products, ethics
- **Business/Startup** — funding, launches, strategy
- **Science** — research breakthroughs, papers
- **Culture/Society** — policy, social trends, media
- **Other** — anything that doesn't fit above

### Step 2: Cross-Platform Detection
Identify topics that appear on 2+ platforms — these are **cross-platform hot topics** and should be flagged with higher priority. Cross-platform signals indicate broader interest and higher content potential.

### Step 3: Trend Direction
For each topic, estimate trend direction:
- 🔺 **Rising** — new in last 24h, gaining traction fast
- ➡️ **Stable** — consistently popular over multiple days
- 🔻 **Cooling** — was hot, engagement declining

### Step 4: Keyword Extraction
Extract 3-5 keywords/tags per topic for later use in topic selection and SEO.

## Output Format

Present results as a structured report. Use the following format:

```
## 🔥 Trending Report — [Date]

### Cross-Platform Hot Topics (appeared on 2+ platforms)
| # | Topic | Platforms | Category | Trend | Keywords |
|---|-------|-----------|----------|-------|----------|
| 1 | ...   | HN, Reddit | AI/ML  | 🔺    | ...      |

### Platform Breakdown

#### HackerNews Top Stories
| # | Title | Points | Comments | Link |
|---|-------|--------|----------|------|

#### Reddit Popular
| # | Title | Subreddit | Upvotes | Comments |
|---|-------|-----------|---------|----------|

#### GitHub Trending
| # | Repo | Description | ⭐ Today | Language |
|---|------|-------------|----------|----------|

#### ProductHunt Today
| # | Product | Tagline | Upvotes | Category |
|---|---------|---------|---------|----------|

### Quick Stats
- Total topics scanned: X
- Cross-platform topics: X
- Top category: [category]
- Dominant trend: [Rising/Stable/Cooling]
```

## User Interaction

1. **Before crawling:** Ask the user if they want to focus on specific domains or scan broadly
2. **After report:** Ask if they want to:
   - Deep dive into any specific topic (→ trigger `web-researcher`)
   - Get topic selection advice (→ trigger `topic-advisor`)
   - Save the report to a file

## Error Handling

- If a source fails to load, note it in the report and continue with remaining sources
- If WebFetch returns truncated data, supplement with WebSearch for that platform
- Always indicate the crawl timestamp so users know data freshness

## Performance Notes

- Launch independent WebFetch/WebSearch calls in parallel where possible to minimize crawl time
- Cache results during a session to avoid re-crawling if user asks follow-up questions
