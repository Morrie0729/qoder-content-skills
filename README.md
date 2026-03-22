# qoder-content-skills

Qoder skills for content creators — full pipeline from trend scanning to multi-platform content publishing.

## Skills

| Skill | Description | Trigger Keywords |
|-------|-------------|------------------|
| **trending-hunter** | Crawl and aggregate trending topics from HackerNews, Reddit, Twitter/X, ProductHunt, GitHub Trending. Supports custom sources, history comparison, and domain filtering. | `trending`, `热点`, `what's hot`, `趋势扫描` |
| **topic-advisor** | Analyze trends and generate content topic recommendations with differentiation angles, SEO signal check, and history dedup. | `选题`, `topic ideas`, `what should I write`, `内容方向` |
| **web-researcher** | Deep web research — collects facts, data, expert opinions, academic papers, and references. Auto-saves reports. | `research`, `调研`, `dig into`, `深挖`, `素材收集` |
| **content-competitor** | Analyze competing content on a topic — reviews angles, structure, engagement signals, and identifies content gaps. | `competitor analysis`, `竞品分析`, `content gap` |
| **content-drafter** | Generate content drafts in multiple formats: blog posts, Twitter threads, newsletters, Reddit posts. | `write draft`, `写初稿`, `draft it`, `写文章` |
| **content-repurposer** | Transform one piece of content into optimized versions for multiple platforms. | `repurpose`, `改写`, `multi-platform`, `分发` |
| **content-pipeline** | One-command end-to-end workflow — chains all skills with user approval checkpoints. | `full pipeline`, `一键创作`, `全流程` |

## Full Pipeline Workflow

```
Stage 1        Stage 2         Stage 3         Stage 4         Stage 5
[Scan]    →    [Select]   →   [Research]  →   [Draft]    →   [Repurpose]
trending-      topic-          web-            content-        content-
hunter         advisor         researcher      drafter         repurposer
```

Use `content-pipeline` to run the full workflow, or invoke individual skills as needed.

## Shared Configuration

Edit `content-config.json` to customize:
- Default data sources and custom sources
- Focus domains (AI, DevTools, SaaS, etc.)
- Target audience and preferred content formats
- Data persistence settings

## Data Persistence

```
~/.qoder/data/content/
  ├── trending-history/     # Daily trending snapshots (JSON)
  ├── topic-selections/     # Past topic choices
  ├── research-reports/     # Saved research reports (Markdown)
  └── drafts/               # Content drafts by format
```

## Installation

Copy all skill folders to your Qoder skills directory:

```bash
cp -r trending-hunter topic-advisor web-researcher content-competitor content-drafter content-repurposer content-pipeline ~/.qoder/skills/
cp content-config.json ~/.qoder/skills/
mkdir -p ~/.qoder/data/content/{trending-history,topic-selections,research-reports,drafts}
```

Or clone and symlink:

```bash
git clone https://github.com/Morrie0729/qoder-content-skills.git
cd qoder-content-skills
for skill in trending-hunter topic-advisor web-researcher content-competitor content-drafter content-repurposer content-pipeline; do
  ln -sf "$(pwd)/$skill" ~/.qoder/skills/$skill
done
cp content-config.json ~/.qoder/skills/
```
