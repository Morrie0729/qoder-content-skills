# qoder-content-skills

Qoder skills for content creators — trending topic hunting, topic selection advisory, and deep web research.

## Skills

| Skill | Description | Trigger Keywords |
|-------|-------------|------------------|
| **trending-hunter** | Crawl and aggregate trending topics from HackerNews, Reddit, Twitter/X, ProductHunt, GitHub Trending | `trending`, `热点`, `what's hot`, `趋势扫描` |
| **topic-advisor** | Analyze trends and generate content topic recommendations with differentiation angles | `选题`, `topic ideas`, `what should I write`, `内容方向` |
| **web-researcher** | Deep web research — collects facts, data, expert opinions, and references | `research`, `调研`, `dig into`, `深挖`, `素材收集` |

## Workflow

```
trending-hunter → topic-advisor → web-researcher
   (scan)          (select)        (research)
```

1. **Scan** — Run `trending-hunter` to get a multi-platform trending report
2. **Select** — Feed trending data to `topic-advisor` for angle analysis and topic recommendations
3. **Research** — Use `web-researcher` to deep dive into the chosen topic and gather source material

## Installation

Copy the skill folders to your Qoder skills directory:

```bash
cp -r trending-hunter topic-advisor web-researcher ~/.qoder/skills/
```

Or clone the repo and symlink:

```bash
git clone https://github.com/Morrie0729/qoder-content-skills.git
ln -s $(pwd)/qoder-content-skills/trending-hunter ~/.qoder/skills/trending-hunter
ln -s $(pwd)/qoder-content-skills/topic-advisor ~/.qoder/skills/topic-advisor
ln -s $(pwd)/qoder-content-skills/web-researcher ~/.qoder/skills/web-researcher
```
