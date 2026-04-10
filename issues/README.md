# Issues — Guidelines

All incidents, bugs, and problems are documented here for future reference.

---

## Folder Structure

```
issues/
├── vps/
│   ├── redvelvetrenders/    # RVR server incidents
│   ├── curatorhub/          # CuratorHub server incidents
│   └── (other projects)/
├── openclaw/                # OpenClaw AI assistant issues
├── personal/                 # Personal device issues
│   └── linux/
│       └── fedora/           # Fedora workstation/laptop issues
└── (other categories as needed)
```

---

## Filename Format

```
YYYY-MM-DD_short-title-of-issue.md
```

**Examples:**
- `2026-04-03_n8n-crash-loop-cpu-spike.md`
- `2026-04-01_pairing-required-error.md`
- `2026-03-17_vps-hostinger-security-update.md`

---

## Issue File Template

Every issue file should include:

1. **Header** — Date, status, affected system/server
2. **Overview** — What happened in 1-3 sentences
3. **Root Cause** — Why it happened (if known)
4. **Timeline** — Chronological sequence of events
5. **Actions Taken** — Steps taken to resolve
6. **Final Status** — Resolution state
7. **Impact** — Downtime, data loss, user impact
8. **Action Items** — Follow-up tasks to prevent recurrence
9. **Related Files** — Config files, logs, or docs referenced

---

## Status Values

| Status | Meaning |
|--------|---------|
| `RESOLVED` | Fully fixed and confirmed working |
| `INVESTIGATING` | Still being looked into |
| `MONITORING` | Fixed but watching for recurrence |
| `PENDING` | Fix identified but not yet applied |

---

## What to Document

**Always document:**
- Server outages (planned or unplanned)
- Security incidents
- Data loss or corruption
- Critical bugs that affected users
- Service degradation lasting > 30 minutes

**Consider documenting:**
- Non-critical bugs that took significant time to fix
- Third-party service disruptions
- Performance issues

---

## Tips

- Write for your future self — include enough detail that you understand it months later
- Include exact commands used (in code blocks) so you can reuse them
- Note any irreversible changes (e.g., "deleted Docker completely")
- If the same issue recurs, link to the previous incident
