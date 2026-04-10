# Vault Organization Guidelines

**For all vault members. Applies to every new file created from this point forward.**

---

## 1. File Naming

### Format

**Dated files** (meetings, issues, events, capture):
```
YYYY-MM-DD-topic.md
```

**General files** (ongoing notes, projects, people):
```
CamelCase.md
```

### Rules

- **Dated files: lowercase hyphenated** — `2026-04-10-rvr-planning.md`
- **General files: CamelCase** — `RedVelvetRenders.md`, `UserRowen.md`
- **No special characters** — no `#`, `:`, `(`, `)`, `!`, `@` in filenames
- **Be descriptive but brief** — `RvrMarketingPlan.md` not `Marketing.md`
- **No dates in general notes** — `CurrentPriorities.md` not `2026-04-10-CurrentPriorities.md`
- **Dated items get the date prefix** — `2026-04-10-RvrCommunityPlan.md`

### Examples

| Type | Good | Bad |
|------|------|-----|
| Meeting | `2026-04-10-rvr-planning.md` | `Meeting April 10 notes.docx`, `rvr-meeting-4-10.md` |
| Issue | `2026-04-03-n8n-crash-loop.md` | `2026-04-03-n8n-crash-loop-cpu-spike.md`, `Bugfix.md` |
| Capture | `2026-04-10-idea-voice-ai.md` | `note from convo.md`, `Idea.md` |
| Project | `RedVelvetRenders.md` | `rvr.md`, `redvelvet-renders.md` |
| Person | `UserRowen.md` | `user-rowen.md`, `DrRowen.md` |

---

## 2. Folder Structure

### Standard folders

- `inbox/` — unprocessed capture. Review and move items regularly.
- `memory/` — vault-level persistent context (user profile, priorities, system notes)
- `projects/` — active and future projects
- `interests/` — research, topics, areas of curiosity
- `people/` — people (contacts, team, collaborators)
- `meetings/` — meeting notes, sorted by date
- `issues/` — technical issues, bugs, problem tracking

### Rules

- **2-3 levels max** — `projects/RVR/marketing/` is fine. `projects/RVR/marketing/social/reddit/campaigns/week1/` is too deep.
- **Folder names: lowercase, hyphenated** — `tech-research/`, not `TechResearch/` or `tech_research/`
- **No empty folders** — if you create a folder, put something in it or delete it

### When to create a new subfolder

- When a project/interest has **5+ files** and distinct areas (e.g., `RVR/marketing/`, `RVR/mobile/`)
- When a topic area is **ongoing and growing** (e.g., `interests/AI/research-summaries/`)

---

## 3. When to Split Files

### Size threshold
- **~500+ lines** in a single note → consider splitting
- **~1000+ lines** → strongly consider splitting regardless

### Topic splitting
- If one file covers **multiple distinct topics** → split by topic
- If a project file has **independent sections** (plan, notes, tasks, retro) → split or use headers

### Example
```
// Before: one giant project file
rvr.md (800 lines — planning, tasks, notes, retro all mixed)

// After: split by concern
rvr-project.md (200 lines — overview, goals, key decisions)
rvr-marketing-plan.md (300 lines — current campaign)
rvr-tasks.md (300 lines — active and pending)
```

---

## 4. When to Move Files to a Subfolder

- A folder with **20+ files** → consider sub-categorizing
- A dated file older than **2 months** that no longer needs active editing → archive or move to a `archive/` subfolder
- Files that belong to a **specific project or topic** → move into that project's folder

---

## 5. Frontmatter

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
- **`tags:`** — comma-separated, no # prefix, no spaces inside brackets

---

## 6. Renamed Files (2026-04-10)

These were renamed to follow the guidelines:

| Old Name | New Name |
|---|---|
| `interests/AI/ResearchSummaries/2026.04.09 - Indic AI Roleplay Market Analysis.md` | `interests/AI/research-summaries/2026-04-09-indic-ai-roleplay-market.md` ✓ |
| `interests/AI/ResearchSummaries/2026.04.09 - The Frontier AI Landscape.md` | `interests/AI/research-summaries/2026-04-09-frontier-ai-landscape.md` ✓ |
| `issues/openclaw/2026-04-01_pairing-required-error.md` | `issues/openclaw/2026-04-01-pairing-required-error.md` ✓ |
| `issues/vps/redvelvetrenders/2026-04-03_n8n-crash-loop-cpu-spike.md` | `issues/vps/redvelvetrenders/2026-04-03-n8n-crash-loop.md` ✓ |
| `projects/RVR/SCENARIO_CARD_SYSTEM_PROMPT.md` | `projects/RVR/character-card-system-prompt.md` ✓ |
| `projects/current_priorities.md` | `projects/active-tasks.md` ✓ |
| `memory/user_rowen.md` | `memory/user-rowen.md` ✓ |
| `memory/projects_overview.md` | `memory/projects-overview.md` ✓ |
| `memory/current_priorities.md` | `memory/current-priorities.md` ✓ |
| `memory/system_architecture.md` | `memory/system-architecture.md` ✓ |
| `projects/AgroTechCompany.md` | `projects/agrotech-company.md` ✓ |
| `projects/RedVelvetRenders.md` | `projects/redvelvet-renders.md` ✓ |
| `interests/AI/Evo2.md` | `interests/AI/evo2.md` ✓ |
| `interests/Tools/TECH_TOOLS.md` | `interests/Tools/tech-tools.md` ✓ |
| `issues/personal/linux/fedora/2026-04-03_vlc-codec-missing-fedora42.md` | `issues/personal/linux/fedora/2026-04-03-vlc-codec-missing-fedora42.md` ✓ |

---

## 7. Quick Reference

**File name formula:**
```
// Dated (events, capture, issues, meetings)
YYYY-MM-DD-topic.md

// Ongoing (projects, people, interests)
CamelCase.md
```

**Folder name formula:**
```
lowercase-hyphenated/
```

**Frontmatter formula:**
```yaml
---
title: Title Case
created: YYYY-MM-DD
tags: [tag1, tag2]
---
```
