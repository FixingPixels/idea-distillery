# Brainstorms — Knowledge Base Schema

## Purpose

This is a personal idea vault for capturing, connecting, and compounding brainstorming sessions over time. The LLM writes and maintains all files under `wiki/`. The human brainstorms freely in conversation and directs queries. The human never edits wiki files directly.

The vault follows Andrej Karpathy's LLM Wiki concept: raw sources are immutable session logs, the wiki is the LLM-maintained knowledge layer that structures, cross-links, and synthesizes across them. Over time the wiki becomes a second brain for ideas — surfacing patterns, tracking idea evolution, and making prior sessions useful in future ones.

---

## Directory Layout

- `raw/` — Immutable brainstorm session logs written by the brainstorm-synthesis skill. Never modify these.
- `wiki/index.md` — Master catalog. Every wiki page must appear here.
- `wiki/log.md` — Append-only activity log.
- `wiki/summaries/` — One Idea Register per brainstorm session (structured output from the raw log).
- `wiki/ideas/` — One page per distinct idea. First-class directory. Ideas persist and evolve across sessions.
- `wiki/themes/` — Recurring problem spaces, domains, and topics that appear across multiple sessions.
- `wiki/patterns/` — Cross-session insights about thinking tendencies: recurring strengths, blind spots, and instincts.
- `wiki/journal/` — Weekly or periodic reflection entries synthesizing recent activity.
- `wiki/presentations/` — Marp slide decks generated from wiki content.

---

## File Naming

- All lowercase, hyphens for word separation: `financial-therapy-platform.md`, `session-2026-04-13.md`
- No spaces, no special characters, no uppercase
- Name should match the page title slug

---

## Page Format

Every wiki page uses this frontmatter and structure:

```yaml
---
title: "Page Title"
type: summary | idea | theme | pattern | journal
tags: [tag1, tag2, tag3]
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: ["raw/brainstorm-YYYY-MM-DD.md"]
confidence: high | medium | low
---
```

### Required Sections by Page Type

**Summary pages** (`wiki/summaries/`):

One per brainstorm session. This is the structured Idea Register — the primary output of the brainstorm-synthesis skill.

- `## Session Overview` — Date, number of ideas, top idea, one-sentence characterization of the session's theme
- `## Ideas` — All ideas from the session with status, lenses, viability scores, and key insight (per register template)
- `## Patterns` — Recurring weaknesses, recurring strengths, thematic through-line, most promising idea, most interesting discard, unasked question
- `## Next Steps` — Downstream skill handoffs or follow-on actions
- `## Source Metadata` — Link to `raw/` log file, session date

**Idea pages** (`wiki/ideas/`):

One per distinct idea. Ideas outlive individual sessions — this page is updated whenever an idea resurfaces or evolves.

- `## Concept` — What this idea is: problem it solves, target audience, core value proposition. Plain English, no hype.
- `## History` — Chronological log of sessions in which this idea appeared. Note status at each appearance and any pivots.
- `## Viability Assessment` — Current best scores on Desirability / Feasibility / Distinctiveness, with brief rationale. Update each session.
- `## Strengths` — What's genuinely working about this idea.
- `## Open Questions` — Unresolved questions that would most change the picture. The most important question goes first.
- `## Related Ideas` — Wiki links to ideas this one competes with, complements, or evolved from.
- `## Related Themes` — Wiki links to the themes this idea belongs to.
- `## Status** — `active` | `parked` | `discarded` | `evolved` (note what it evolved into)

**Theme pages** (`wiki/themes/`):

One per recurring problem space, domain, or topic that appears across sessions.

- `## Overview` — What this theme is and why it keeps coming up.
- `## Ideas in This Theme` — Wiki links to all ideas that belong here.
- `## Patterns Observed** — What the ideas in this theme tend to get right or wrong. Updated over time.
- `## Open Questions` — Unresolved questions about this space.
- `## Sources` — Which sessions surfaced this theme.

**Pattern pages** (`wiki/patterns/`):

Cross-session insights about how this person thinks. These are meta-level observations, not ideas themselves.

- `## Observation` — What the pattern is, stated plainly. One paragraph.
- `## Evidence** — Specific ideas or sessions that support this pattern. At least two data points before creating a page.
- `## Implication** — What this pattern means for how to brainstorm better or evaluate ideas differently.
- `## Sessions** — Which raw sessions this pattern was observed in.

**Journal pages** (`wiki/journal/`):

Weekly or periodic reflections synthesizing recent activity.

- `## Period** — Date range covered.
- `## Sessions This Period** — Links to raw logs and summaries.
- `## Ideas That Evolved** — Any status changes or pivots.
- `## Patterns Emerging** — New or strengthening meta-observations.
- `## Open Questions** — Anything unresolved worth carrying forward.
- `## Next Session Focus** — What to explore or pressure-test next.

---

## Linking Conventions

- Use Obsidian-style wiki links: `[[ideas/financial-therapy-platform]]`
- Always use relative paths from the `wiki/` root
- Every page must link to at least one other page — no orphans
- Idea pages must link to their theme(s) and related ideas
- Summary pages must link to every idea page mentioned in that session
- Pattern pages must link to the sessions and ideas that are their evidence base

---

## Tagging Taxonomy

**Status** — current state of an idea
- `active`, `parked`, `discarded`, `evolved`

**Domain** — the problem space or industry
- `saas`, `creator-economy`, `ai`, `fintech`, `health`, `education`, `productivity`, `consumer`, `b2b`, `media`

**Lens** — evaluation angle (from brainstorm-synthesis skill)
- `feasibility`, `audience`, `market-gap`, `timing`, `differentiation`, `compounding`, `monetization`, `risk`, `hidden-strength`

**Session** — which raw session(s) this page references
- `session-YYYY-MM-DD`

**Type**
- `idea`, `theme`, `pattern`, `summary`, `journal`

---

## Confidence Levels

- **high** — Well-established: appeared multiple sessions, has evidence, withstood pushback
- **medium** — Plausible and discussed, but not yet tested or revisited
- **low** — Single mention, assumption, or speculative; needs more sessions before acting on it

---

## Workflows

### Brainstorm Session (primary workflow)

Triggered automatically by the brainstorm-synthesis skill at the end of a session. The skill:

1. Saves the session log to `raw/brainstorm-YYYY-MM-DD.md`
2. Creates `wiki/summaries/brainstorm-YYYY-MM-DD.md` with the full Idea Register
3. For each `active` or `parked` idea: create `wiki/ideas/<idea-slug>.md` if it doesn't exist, or update the existing page with new session history and revised viability assessment
4. For each idea: identify its theme(s), creating `wiki/themes/<theme-slug>.md` if needed
5. Add cross-links in both directions: summary ↔ ideas ↔ themes
6. Update `wiki/index.md` — add new entries, update summaries of changed pages
7. Append to `wiki/log.md` with timestamp, session date, and pages created/updated
8. If a pattern is observed that already has a page, update its evidence; if a new pattern emerges across ≥2 data points, create `wiki/patterns/<pattern-slug>.md`

### Ingest

When the user says "ingest [file]" or points to a file in `raw/`:

1. Read the raw source completely
2. Run the Brainstorm Session workflow above against it
3. Flag any contradictions with existing idea pages (e.g., an idea previously discarded is now active)

### Query

When the user asks a question about ideas, themes, or patterns:

1. Read `wiki/index.md` to find relevant pages
2. Read those pages
3. Synthesize an answer citing specific pages with wiki links
4. If the answer surfaces a new insight worth preserving, create a synthesis entry or update an existing pattern page

### Weekly Review

When the user says "weekly review" or "what's the state of my ideas":

1. Read `wiki/log.md` to identify sessions and changes this week
2. Read all `active` idea pages
3. Read all pattern pages
4. Create a journal entry in `wiki/journal/YYYY-MM-DD.md` covering: sessions this period, ideas that evolved, patterns emerging, open questions, next session focus
5. Update `wiki/index.md` and `wiki/log.md`

### Lint

When the user says "lint" or "health check":

1. Read all wiki pages
2. Check for: orphan pages (no inbound links), idea pages missing required sections, ideas without theme links, pattern pages with only one data point (insufficient evidence), stale open questions (≥3 sessions old and unchanged), ideas listed as `active` that haven't appeared in ≥4 sessions
3. Fix what can be fixed automatically (add missing cross-links, update dates)
4. Report issues that need human judgment (e.g., should this idea be downgraded to `parked`?)
5. Suggest missing theme or pattern pages implied by existing content
6. Update `wiki/log.md`

---

## Rules

- Never modify files in `raw/`
- Always update `wiki/index.md` and `wiki/log.md` after any wiki change
- Every active idea must have a `wiki/ideas/` page — no ideas live only in summaries
- Prefer updating existing pages over creating duplicates; if a page grows beyond ~500 lines, split it
- Pattern pages require at least two sessions as evidence before creation — one data point is noise
- When a source provides specific viability scores or key insights, preserve the exact language — vague summaries are less useful than precise ones
- Use plain English; no startup jargon without definition
- All dates in ISO 8601 format: YYYY-MM-DD
- When in doubt about a claim, set confidence to `low` and note the uncertainty explicitly
- Ideas should be named by their concept, not by a cute brand name, unless the brand name was explicitly chosen — this keeps the index legible
