---
title: Vault Organizing Guide
created: 2026-04-13
updated: 2026-04-13
tags: [guide, vault]
---

# Vault Organizing Guide

Comprehensive instructions for organizing the SecondBrain vault. This guide supersedes VAULT_GUIDELINES.md and the research organization guide — everything about vault structure lives here.

---

## Table of Contents

1. [Folder Structure](#1-folder-structure)
2. [File Naming](#2-file-naming)
3. [Frontmatter](#3-frontmatter)
4. [Wikilinks and References](#4-wikilinks-and-references)
5. [Research Documents](#5-research-documents)
6. [Journal](#6-journal)
7. [Meetings](#7-meetings)
8. [Inbox](#8-inbox)
9. [Projects](#9-projects)
10. [Memory Files](#10-memory-files)
11. [When to Split Files](#11-when-to-split-files)
12. [When to Create Subfolders](#12-when-to-create-subfolders)
13. [Git Workflow](#13-git-workflow)
14. [Renamed Files Log](#14-renamed-files-log)
15. [Archiving](#15-archiving)

---

## 1. Folder Structure

### Top-Level Folders

| Folder | Purpose |
|--------|---------|
| `journal/` | Daily journal entries, one file per day |
| `memory/` | Vault-level persistent context (user profile, priorities, system notes) |
| `inbox/` | Unprocessed capture — review and move items regularly |
| `interests/` | Research, topics, areas of curiosity |
| `meetings/` | Meeting notes, sorted by date |
| `issues/` | Technical issues, bugs, problem tracking |
| `people/` | People (contacts, team, collaborators) |
| `projects/` | Active and future projects |

### Rules

- **2–3 levels max** — `projects/RVR/marketing/` is fine. `projects/RVR/marketing/social/reddit/campaigns/week1/` is too deep.
- **Folder names: lowercase, hyphenated** — `tech-research/`, not `TechResearch/` or `tech_research/`
- **No empty folders** — if you create a folder, put something in it or delete it

---

## 2. File Naming

### Dated Files

For events, meetings, captures, issues — anything timestamped:

```
YYYY-MM-DD-topic.md
```

Examples:
- `2026-04-13-rvr-planning.md`
- `2026-04-12-weekly-review.md`
- `2026-04-03-n8n-crash-loop.md`
- `2026-04-11-meeting-emerson-marketing.md`

### General Files

For ongoing notes — projects, people, topics with no fixed date:

```
CamelCase.md
```

Examples:
- `RedVelvetRenders.md`
- `UserRowen.md`
- `CurrentPriorities.md`
- `CommunityAndFandomPlan.md`

### Rules

- **Dated files: lowercase hyphenated** — no spaces, no special characters
- **General files: CamelCase** — no spaces, capital on each word
- **No special characters** — no `#`, `:`, `(`, `)`, `!`, `@` in filenames
- **Be descriptive but brief** — `RvrMarketingPlan.md` not `Marketing.md`
- **No dates in general notes** — `CurrentPriorities.md` not `2026-04-10-CurrentPriorities.md`
- **No dates in project files** — the `created:` frontmatter field tracks creation date

### Quick Reference

| Type | Format | Example |
|------|--------|---------|
| Meeting | `YYYY-MM-DD-topic.md` | `2026-04-13-rvr-planning.md` |
| Issue | `YYYY-MM-DD-topic.md` | `2026-04-03-n8n-crash-loop.md` |
| Daily capture | `YYYY-MM-DD-topic.md` | `2026-04-13-voice-ai-idea.md` |
| Project | `CamelCase.md` | `RedVelvetRenders.md` |
| Person | `CamelCase.md` | `UserRowen.md` |
| Plan | `CamelCase.md` | `CommunityAndFandomPlan.md` |

---

## 3. Frontmatter

Every note gets this minimum frontmatter:

```yaml
---
title: note-name
created: YYYY-MM-DD
tags: [tag1, tag2]
---
```

- **`title:`** — display name (use title case, spaces allowed)
- **`created:`** — date the note was first created
- **`tags:`** — comma-separated, no `#` prefix, no spaces inside brackets

### Optional Frontmatter Fields

- **`updated:`** — date the note was last modified (use when the note tracks evolving state)
- **`tags:`** — use sparingly. Effective tags: `[project]`, `[status]`, `[guide]`, `[vault]`, `[active]`

---

## 4. Wikilinks and References

- Use `[[note name]]` for all internal references
- Use `[[note name|display text]]` for aliased links
- Use `![[note]]` to embed another note's content inline
- Use `%% inline comment %%` for notes that don't appear in preview

### When to Link vs. Embed

- **Link (`[[]]`)** — when you want to reference a note without duplicating content
- **Embed (`![[]]`)** — when you want to pull in the full content inline (use rarely — creates coupling)

---

## 5. Research Documents

Research is a **process**: initial questions → inputs → outputs → synthesis → action. The folder structure reflects this lifecycle.

### Folder Structure

For any project or interest area with active research:

```
area/
  research/
    queries/        # Research briefs, questions, prompts sent to AI tools
    reports/        # Raw research outputs from AI tools or manual research
    synthesis/      # (optional) Your own distillation and key takeaways
```

### Folder Names

- Always `research/` — lowercase, no hyphen, singular
- Subfolders: `queries/`, `reports/`, `synthesis/` — lowercase, plural
- Location: inside the relevant `projects/` or `interests/` folder

### Document Types

#### Queries (`queries/`)

The **inputs** to a research process.

Examples:
- A Gemini Deep Research brief sent to an AI
- A list of questions for web research
- A hypothesis to investigate
- A competitor analysis framework

**Naming:** `YYYY-MM-DD-topic-research-brief.md` or `YYYY-MM-DD-topic-questions.md`

#### Reports (`reports/`)

The **outputs** from a research process.

Examples:
- Gemini Deep Research output
- Web search compilation
- Competitive landscape analysis
- Market research findings

**Naming:** `YYYY-MM-DD-topic-report.md` or `YYYY-MM-DD-topic-analysis.md`

#### Synthesis (`synthesis/` — optional)

Your own **processed output** — distilled insights, actionable next steps, or a brief for stakeholders.

Use when:
- Multiple reports exist and you need a consolidated view
- The research directly informs a project decision

**Naming:** `YYYY-MM-DD-topic-synthesis.md`

### When to Create a Research Folder

Create `research/` when:
1. A project has **multi-stage or ongoing research** (e.g., RVR's advertising strategy)
2. Research files are **5+ and growing**
3. Inputs and outputs need to be tracked **separately**

Do **not** create a research folder for:
- A single research file that won't grow (put it directly in the project folder)
- One-off questions (use `inbox/` instead)

### AI-Assisted Research Workflow (Gemini Deep Research)

1. **Create query** → `queries/YYYY-MM-DD-topic-research-brief.md`
   - Write a detailed brief with context, constraints, and specific research questions
   - Include product/project background so the AI has full context

2. **Run research** → Copy the brief into Gemini Deep Research

3. **Save report** → Save the AI output to `reports/YYYY-MM-DD-topic-report.md`
   - If the output is very long (>500 lines), consider splitting into multiple files

4. **Synthesize** → Create `synthesis/YYYY-MM-DD-topic-synthesis.md` (if needed)
   - Extract key findings relevant to the project
   - Note actionable next steps

### Examples in This Vault

**Project-specific research:**
```
projects/RVR/research/
  queries/
    2026-04-13-india-advertising-research-brief.md
  reports/
    2026-04-09-indic-ai-roleplay-market.md
    2026-04-13-india-instagram-advertising-strategy.md
```

**Interest-area research:**
```
interests/AI/research-summaries/
  2026-04-09-frontier-ai-landscape.md
  2026-04-09-indic-ai-roleplay-market.md
```

---

## 6. Journal

Daily journal entries at `journal/YYYY-MM-DD.md`.

### Start of Session Checklist

Every session, read before working:
1. `journal/daily-journal` — today's entry and format guide
2. `memory/CurrentPriorities` — what's active and what needs to happen next
3. `projects/ActiveTasks` — task lists for active projects
4. `inbox/` — unprocessed items, loose threads, capture from the day

### Journal Entry Format

```markdown
# YYYY-MM-DD

## What I'm Working On

## Decisions Made

## Next Steps

## Notes / Loose Threads
```

---

## 7. Meetings

Meeting notes at `meetings/YYYY-MM-DD-topic.md`.

### Meeting Note Format

```markdown
---
title: meeting-topic
created: YYYY-MM-DD
attendees: [name1, name2]
tags: [meeting]
---

# Meeting: Topic
**Date:** YYYY-MM-DD
**Attendees:** name1, name2

## Agenda

## Discussion

## Decisions

## Action Items
- [ ] Task 1 — @name
- [ ] Task 2 — @name
```

---

## 8. Inbox

`inbox/` is the capture basket — anything unprocessed goes here.

### Rules

- Review inbox **at the start of every session**
- Either process it (move to the right place, delete, or act on it) or leave it
- If it stays for more than a week, either process it or delete it
- Never let inbox become a graveyard — things in there should be actionable

---

## 9. Projects

Project notes live in `projects/`. Each project gets a folder if it has multiple areas of work.

### Project Folder Structure

```
projects/ProjectName/
  ProjectName.md          # Overview, goals, current status
  SubAreaPlan.md          # Plans for specific areas
  SubAreaNotes.md         # Ongoing notes for that area
```

### Project File Template

```markdown
---
title: project-name
created: YYYY-MM-DD
tags: [project]
---

# Project Name

## Overview

## Goals

## Current Status

## Key Decisions

## Next Steps

## Related
```

---

## 10. Memory Files

`memory/` holds vault-level context that persists across sessions.

### Core Memory Files

| File | Purpose |
|------|---------|
| `user.md` or `UserName.md` | User profile — role, preferences, communication style |
| `current-priorities.md` | What's active right now across all projects |
| `projects-overview.md` | Summary of all active projects |

### Rules

- Memory files are **summaries, not duplicate records** — link to source files rather than copying content
- Update memory files **immediately** when new information arrives
- After every session: push to git so memory survives across devices

---

## 11. When to Split Files

### Size Threshold

- **~500+ lines** → consider splitting
- **~1000+ lines** → strongly consider splitting regardless

### Topic Splitting

If one file covers **multiple distinct topics** → split by topic
If a project file has **independent sections** (plan, notes, tasks, retro) → split or use headers

### Example

```markdown
# Before: one giant project file
rvr.md (800 lines — planning, tasks, notes, retro all mixed)

# After: split by concern
rvr-project.md (200 lines — overview, goals, key decisions)
rvr-marketing-plan.md (300 lines — current campaign)
rvr-tasks.md (300 lines — active and pending)
```

---

## 12. When to Create Subfolders

- A folder with **20+ files** → consider sub-categorizing
- A dated file older than **2 months** that no longer needs active editing → archive or move to an `archive/` subfolder
- Files that belong to a **specific project or topic** → move into that project's folder
- When a topic area is **ongoing and growing** (e.g., `interests/AI/research-summaries/`)

---

## 13. Git Workflow

### Sync After Every Change

The vault must be synced to GitHub after **every single change** — no exceptions.

```bash
git add <file>
git commit -m "Descriptive message"
git push
```

### Before Starting a Session

Always pull latest before reading or changing anything:

```bash
git pull --rebase
```

### Commit Messages

- Write for your future self: "why" not "what"
- Be specific: "Add voice AI tier research to RVR" not "Update research"
- Reference issue/ticket if applicable

---

## 14. Renamed Files Log

This log tracks files that were renamed to follow the guidelines. Update this log when renaming files.

### Renamed Files (2026-04-10)

| Old Name | New Name |
|---|---|
| `interests/AI/ResearchSummaries/2026.04.09 - Indic AI Roleplay Market Analysis.md` | `interests/AI/research-summaries/2026-04-09-indic-ai-roleplay-market.md` |
| `interests/AI/ResearchSummaries/2026.04.09 - The Frontier AI Landscape.md` | `interests/AI/research-summaries/2026-04-09-frontier-ai-landscape.md` |
| `issues/openclaw/2026-04-01_pairing-required-error.md` | `issues/openclaw/2026-04-01-pairing-required-error.md` |
| `issues/vps/redvelvetrenders/2026-04-03_n8n-crash-loop-cpu-spike.md` | `issues/vps/redvelvetrenders/2026-04-03-n8n-crash-loop.md` |
| `projects/RVR/SCENARIO_CARD_SYSTEM_PROMPT.md` | `projects/RVR/character-card-system-prompt.md` |
| `projects/current_priorities.md` | `projects/active-tasks.md` |
| `memory/user_rowen.md` | `memory/user-rowen.md` |
| `memory/projects_overview.md` | `memory/projects-overview.md` |
| `memory/current_priorities.md` | `memory/current-priorities.md` |
| `memory/system_architecture.md` | `memory/system-architecture.md` |
| `projects/AgroTechCompany.md` | `projects/agrotech-company.md` |
| `projects/RedVelvetRenders.md` | `projects/redvelvet-renders.md` |
| `interests/AI/Evo2.md` | `interests/AI/evo2.md` |
| `interests/Tools/TECH_TOOLS.md` | `interests/Tools/tech-tools.md` |
| `issues/personal/linux/fedora/2026-04-03_vlc-codec-missing-fedora42.md` | `issues/personal/linux/fedora/2026-04-03-vlc-codec-missing-fedora42.md` |

### Renamed Folders (2026-04-13)

| Old Name | New Name |
|---|---|
| `projects/RVR/research-summaries/` | `projects/RVR/research/` (with `queries/` and `reports/` subfolders) |

---

## 15. Archiving

When work is complete, active files transition to archive status rather than accumulating in active folders.

### When to Archive

- A project, phase, or workstream is marked complete
- A file is superseded by a final version or implementation
- Working files (prompts, plans, briefs) that are no longer referenced in active workflows

### When NOT to Archive

- **Active references** — if other files still `[[wikilink]]` to a file, keep it or update the link first
- **Debugging needs** — files that may need to be referenced for debugging or reversal
- **Meeting notes, journal entries, inbox contents** — these are never archived

### Archive Folder Locations

| File Type | Archive Location |
|---|---|
| Project files | `projects/ProjectName/archive/` |
| Vault-level files | `memory/archive/` |
| Root-level files (rare) | `archive/` at vault root |

### Archive File Naming

- **Dated items:** `YYYY-MM-DD-topic.md` — same as active files (date is when the work was done, not when archived)
- **Merged archives:** `YYYY-MM-DD-topic-archive.md` for combined documents from the same project/phase

### Archiving Checklist

1. Confirm the work is complete and no active references remain
2. Merge if multiple files cover the same topic
3. Add `archived:` date and `status: archived` tag to frontmatter
4. Move to the appropriate `archive/` subfolder
5. Remove from any active task lists or index files
6. Git commit with message referencing what was archived

### Archive Frontmatter Template

```yaml
---
title: Title of Work
created: YYYY-MM-DD
archived: YYYY-MM-DD
tags: [archived, project-name]
status: archived
---
```

### Finding Archived Files

Archived files are still searchable and accessible — they just live in `archive/` subfolders. Use `[[archive/path/to/file]]` to link to them if needed for reference.

---

## Quick Reference

### File Name Formula

```markdown
# Dated (events, capture, issues, meetings)
YYYY-MM-DD-topic.md

# Ongoing (projects, people, interests)
CamelCase.md
```

### Folder Name Formula

```
lowercase-hyphenated/
```

### Research Folder Formula

```
research/
  queries/      # inputs (briefs, questions)
  reports/      # outputs (AI results, analysis)
  synthesis/    # (optional) distilled insights
```

### Frontmatter Formula

```yaml
---
title: Title Case
created: YYYY-MM-DD
tags: [tag1, tag2]
---
```
