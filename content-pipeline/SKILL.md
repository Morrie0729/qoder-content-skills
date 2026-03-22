---
name: content-pipeline
description: "One-command content creation pipeline — chains trending-hunter, topic-advisor, web-researcher, content-drafter, and content-repurposer into an end-to-end workflow with user checkpoints. Use when user says: full pipeline, 一键创作, end to end, 全流程, content workflow, 帮我从头到尾."
---

# Content Pipeline - End-to-End Content Creation Workflow

Orchestrate the full content creation pipeline from trend scanning to multi-platform draft output, with user approval checkpoints at each stage.

## Trigger Conditions

Activate this skill when the user:
- Wants to go from "I want to write something" to a finished draft in one session
- Says keywords like: "full pipeline", "一键创作", "end to end", "全流程", "content workflow", "帮我从头到尾"

## Pipeline Stages

```
Stage 1        Stage 2         Stage 3         Stage 4         Stage 5
[Scan]    →    [Select]   →   [Research]  →   [Draft]    →   [Repurpose]
trending-      topic-          web-            content-        content-
hunter         advisor         researcher      drafter         repurposer
    ↓              ↓               ↓               ↓               ↓
 Checkpoint    Checkpoint     Checkpoint     Checkpoint     Final Output
 "Proceed?"    "Pick topic"   "Enough?"      "Revise?"      "Publish?"
```

Each stage triggers the corresponding skill. Between stages, there is a **user checkpoint** where the user reviews output and decides to proceed, adjust, or stop.

## Execution Flow

### Pre-Flight
1. Read `~/.qoder/skills/content-config.json` for defaults
2. Ask the user one opening question:
   > "What would you like to create today? Options:
   > A) Scan trends and let me suggest what to write (full pipeline)
   > B) I already have a topic: [user specifies] (skip to Stage 2 or 3)
   > C) I have a draft ready, help me repurpose it (skip to Stage 5)"

### Stage 1: Trend Scanning
- Invoke `trending-hunter` skill behavior
- Present the trending report
- **Checkpoint:** "Which topics interest you? Pick 1-3 for deeper analysis, or tell me a direction."

### Stage 2: Topic Selection
- Invoke `topic-advisor` skill behavior with the selected topics
- Present 3 ranked recommendations
- **Checkpoint:** "Which topic do you want to go with? Or want me to analyze different ones?"
- Save the chosen topic to `~/.qoder/data/content/topic-selections/`

### Stage 3: Deep Research
- Invoke `web-researcher` skill behavior on the chosen topic
- Present the research report
- **Checkpoint:** "Is this enough material, or should I dig deeper into specific areas?"
- Auto-save report to `~/.qoder/data/content/research-reports/`

### Stage 4: Drafting
- Invoke `content-drafter` skill behavior
- First present an outline for approval
- Then generate the full draft
- **Checkpoint:** "How's the draft? Want me to revise any sections, adjust the tone, or proceed to repurposing?"
- Save draft to `~/.qoder/data/content/drafts/`

### Stage 5: Multi-Platform Repurposing
- Invoke `content-repurposer` skill behavior
- Generate versions for all configured platforms
- **Final output:** Present all versions
- Save all versions to `~/.qoder/data/content/drafts/`

## Checkpoint Rules

At each checkpoint:
1. **Always wait for user input** — never auto-proceed to the next stage
2. **Allow skipping** — user can say "skip ahead to drafting" at any point
3. **Allow backing up** — user can say "go back and pick a different topic"
4. **Allow stopping** — user can say "save progress, I'll continue later"
5. **Show progress** — indicate which stage they're at (e.g., "Stage 2/5: Topic Selection")

## Quick Modes

Support shortcuts for experienced users:

| Command | Behavior |
|---------|----------|
| "pipeline" | Full 5-stage flow |
| "pipeline [topic]" | Skip Stage 1, start at topic analysis |
| "pipeline draft [topic]" | Skip to Stage 3-4, research + draft |
| "pipeline repurpose" | Skip to Stage 5 with existing draft |

## Progress Tracking

Use the TodoWrite tool to display pipeline progress:
```
1. [completed] Scan trending topics
2. [in_progress] Select content topic
3. [pending] Deep research
4. [pending] Write draft
5. [pending] Multi-platform repurpose
```

## Error Recovery

- If any stage produces poor results, offer to retry with adjusted parameters
- If the user abandons mid-pipeline, save all progress so far to the data directory
- Next time the user starts a pipeline, check for unfinished sessions and offer to resume

## Session Summary

At the end of a completed pipeline, present a summary:
```
## Pipeline Complete!

**Topic:** [chosen topic]
**Angle:** [chosen angle]
**Sources used:** [N]
**Outputs created:**
- Blog post draft: ~/.qoder/data/content/drafts/[file]
- Twitter thread: ~/.qoder/data/content/drafts/[file]
- Newsletter version: ~/.qoder/data/content/drafts/[file]

**Next steps:**
- Review and polish the drafts
- Schedule publication
- Run `trending-hunter` again tomorrow for fresh topics
```
