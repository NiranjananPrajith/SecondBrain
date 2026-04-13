---
title: Research Organization Guide
created: 2026-04-13
tags: [guide, vault, research]
---

# Research Organization Guide

How to organize research documents in the SecondBrain vault.

---

## Core Principle

Research is a **process**, not a destination. A research folder should reflect the full lifecycle: initial questions → research inputs → outputs → synthesis → action.

---

## Folder Structure

For any project or interest area with active research:

```
area/
  research/
    queries/        # Research briefs, questions, prompts sent to AI tools
    reports/        # Raw research outputs from AI tools or manual research
    synthesis/      # (optional) Your own synthesis, analysis, key takeaways
```

### Folder Names

- Always `research/` — lowercase, no hyphen, singular
- Subfolders: `queries/`, `reports/`, `synthesis/` — lowercase, plural
- Location: inside the relevant `projects/` or `interests/` folder

### Examples in This Vault

**Project-specific research (active work):**
```
projects/RVR/research/
  queries/
    2026-04-13-india-advertising-research-brief.md
  reports/
    2026-04-09-indic-ai-roleplay-market.md
    2026-04-13-india-instagram-advertising-strategy.md
```

**Interest-area research (ongoing curiosity):**
```
interests/AI/research-summaries/
  2026-04-09-frontier-ai-landscape.md
  2026-04-09-indic-ai-roleplay-market.md
```

---

## Document Types

### Queries (`queries/`)

The **inputs** to a research process. These are your questions, prompts, briefs, or search queries.

Examples:
- A Gemini Deep Research brief sent to an AI
- A list of questions for web research
- A hypothesis to investigate
- A competitor analysis framework

**Naming:** `YYYY-MM-DD-topic-research-brief.md` or `YYYY-MM-DD-topic-questions.md`

### Reports (`reports/`)

The **outputs** from a research process. These are the raw results, findings, or AI-generated analysis.

Examples:
- Gemini Deep Research output
- Web search compilation
- Competitive landscape analysis
- Market research findings

**Naming:** `YYYY-MM-DD-topic-report.md` or `YYYY-MM-DD-topic-analysis.md`

### Synthesis (`synthesis/` — optional)

Your own **processed output**. Where you distill reports into actionable insights, a brief for stakeholders, or a refined plan.

Use this when:
- Multiple reports exist and you need a consolidated view
- You want to capture "so what" alongside "what"
- The research directly informs a project decision

**Naming:** `YYYY-MM-DD-topic-synthesis.md`

---

## When to Create a Research Folder

Create `research/` when:

1. **Active project with research needs** — A project has multi-stage or ongoing research (e.g., RVR's advertising strategy research)
2. **5+ research files in a single area** — If research files start piling up without structure
3. **Research is multi-stage** — Inputs and outputs need to be tracked separately (e.g., Gemini brief → Gemini report)

Do not create a research folder for:
- A single research file that won't grow
- One-off questions or captures (use `inbox/` instead)

---

## Workflow: AI-Assisted Research (Gemini Deep Research)

1. **Create query** → `queries/YYYY-MM-DD-topic-research-brief.md`
   - Write a detailed brief with context, constraints, and specific research questions
   - Include the product/project context so the AI has full background

2. **Run research** → Copy the brief into Gemini Deep Research

3. **Save report** → Save the AI output to `reports/YYYY-MM-DD-topic-report.md`
   - If the output is very long (>500 lines), consider splitting into multiple files

4. **Synthesize** → Create `synthesis/YYYY-MM-DD-topic-synthesis.md` (if needed)
   - Extract key findings relevant to the project
   - Note actionable next steps
   - Link to where the research will be used

---

## Maintenance

- **Review quarterly** — Move stale research to an `archive/` subfolder if it's no longer relevant
- **Delete empty folders** — If a subfolder becomes empty after cleanup, delete it
- **Update references** — If a research folder moves or renames, update any files that reference it
- **Keep it alive** — Research folders in active projects should be reviewed and updated as the project evolves

---

## Quick Reference

| Document Type | Purpose | Folder | Naming Pattern |
|---|---|---|---|
| Research brief / questions | Inputs to research | `queries/` | `YYYY-MM-DD-topic-brief.md` |
| Research report / findings | Outputs from research | `reports/` | `YYYY-MM-DD-topic-report.md` |
| Synthesis / analysis | Your distilled takeaways | `synthesis/` | `YYYY-MM-DD-topic-synthesis.md` |
