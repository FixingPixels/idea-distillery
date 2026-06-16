---
title: "Activity Log"
type: log
---

# Activity Log

Append-only record of all wiki changes.

## Format

Each entry follows this format:
```
### YYYY-MM-DD HH:MM — [Action Type]
- **Source/Trigger**: what initiated the action
- **Pages created**: list of new pages
- **Pages updated**: list of updated pages
- **Notes**: any contradictions flagged, decisions made
```

---

### 2026-01-15 00:00 — Brainstorm Session (example)
- **Source/Trigger**: sample session `raw/brainstorm-2026-01-15-example.md` (shipped with the scaffold to demonstrate the format)
- **Pages created**: `summaries/brainstorm-2026-01-15-example`, `ideas/community-tool-library`, `ideas/skill-swap-board`, `themes/local-community-coordination`
- **Pages updated**: `index`
- **Notes**: Example content. Delete it — and this entry — before your first real session (see README → Start clean).

### 2026-01-19 00:00 — Weekly Review (example)
- **Source/Trigger**: sample weekly review
- **Pages created**: `journal/2026-01-19-example`
- **Pages updated**: `index`
- **Notes**: Example content. One session on record, so no `wiki/patterns/` page was created (the ≥2-data-point bar was not met).
