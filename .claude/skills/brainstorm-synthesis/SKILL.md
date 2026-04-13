---
name: brainstorm-synthesis
description: Synthesizes ideas from a brainstorming session into a structured idea register — capturing each idea, tagging it by evaluation lens, scoring rough viability, and surfacing cross-idea patterns and recurring themes. Use this skill whenever the user wants to capture what was discussed in a brainstorming session, summarize ideas explored, build an idea log or register, find patterns across ideas, identify recurring weaknesses or strengths, export brainstorm output to Obsidian or markdown, or says anything like "summarize what we covered", "capture these ideas", "what patterns are you seeing", "log this session", "export to Obsidian", or "let's wrap up this session". Trigger even if the user doesn't use the word "synthesis" — any request to consolidate, capture, or reflect on ideas from a brainstorming conversation is the signal.
---

# Brainstorm Synthesis

Captures, structures, and pattern-matches ideas from a brainstorming session. The output is a portable **Idea Register** — a markdown document suitable for Obsidian, a project file, or standalone reference.

## How It Works

```
SCAN SESSION → TAG IDEAS → SCORE VIABILITY → SURFACE PATTERNS → OUTPUT REGISTER
```

The skill reads the current conversation, extracts every idea discussed (including discarded or half-formed ones), structures each entry, then synthesizes cross-idea observations. Runtime: typically one pass with no user interruption needed.

---

## Phase 1: Session Scan

Read the full conversation history. Extract every distinct idea that was discussed, floated, or referenced — including:

- Ideas the user introduced
- Ideas Claude suggested or refined
- Variations or pivots on a core idea
- Ideas that were rejected or parked (flag these separately — they're still useful data)

**Don't filter yet.** Capture everything. Weak ideas that get discarded still reveal something about the user's direction.

Group closely related ideas (e.g., variations of the same concept) under a parent entry rather than listing them as separate ideas unless the variations are meaningfully distinct.

---

## Phase 2: Tag Each Idea

For each idea, apply one or more **lens tags** from the evaluation framework. Only tag lenses that were actually discussed or that clearly apply — don't force coverage.

| Tag | What it covers |
|-----|----------------|
| `feasibility` | Doability given realistic constraints |
| `audience` | Clarity and reachability of target user |
| `market-gap` | Whether a real gap exists vs. crowded space |
| `timing` | Whether the world is ready for this now |
| `differentiation` | What makes it distinct and defensible |
| `compounding` | Network effects, lock-in, data advantages |
| `monetization` | Natural payment model and pricing pressure |
| `risk` | Most likely failure modes |
| `hidden-strength` | Underappreciated upside |

Also apply a **status tag** to each idea:

- `active` — still live and worth pursuing
- `parked` — not dismissed, but not the focus
- `discarded` — ruled out during session (note why)
- `evolved` — started as one thing, became another (link to parent)

---

## Phase 3: Score Viability

Give each `active` or `parked` idea a rough **Viability Score** on three dimensions. These are fast, honest assessments — not rigorous scores.

| Dimension | 1 | 2 | 3 |
|-----------|---|---|---|
| **Desirability** | Unclear who wants this | Someone wants it | Clear motivated audience |
| **Feasibility** | Major blockers unclear | Doable with significant effort | Path to v1 is visible |
| **Distinctiveness** | Commodity space | Has some angle | Genuinely different |

Record as three digits: e.g., `2/3/1`. Don't average them — the pattern is more informative than a single number. A `1/3/3` idea (unknown audience, easy to build, genuinely different) tells a different story than a `3/1/3` idea.

Skip scoring for `discarded` ideas.

---

## Phase 4: Surface Patterns

After all ideas are tagged and scored, step back and write a **Patterns** section. This is the highest-value part of the output.

Look for:

**Recurring weaknesses** — Does the same lens keep scoring low? E.g., "Three of four ideas have unclear monetization paths" or "Audience definition was the sticking point in every idea today."

**Recurring strengths** — What does this person consistently think well about? E.g., "Strong instinct for timing — ideas tend to land in emerging rather than saturated spaces."

**Thematic through-line** — Is there a single underlying interest or problem driving multiple ideas, even if the surface topics vary?

**The most promising idea** — Name it explicitly. Briefly say why — not flattery, just the honest read.

**The most interesting discard** — If an idea was ruled out but has latent potential, flag it. Discards often get abandoned for the wrong reason.

**Unasked question** — The one question that would most change the picture on the session's best idea. This is the call-to-action for the next session.

---

## Phase 5: Output the Idea Register

Produce the Idea Register markdown document following the format in `references/register-template.md`.

### Output location

This vault follows the Karpathy LLM Wiki pattern. Write two files:

1. **Raw session log** → `raw/brainstorm-YYYY-MM-DD.md`
   A faithful capture of what was discussed this session: the ideas floated, key quotes or framings, context the user mentioned, and any constraints or pivots that happened. This is the immutable source record — write it once, never edit it. It does not need to be a verbatim transcript; it should be a complete, honest log that a future reader could reconstruct the session from.

2. **Idea Register** → `wiki/summaries/brainstorm-YYYY-MM-DD.md`
   The fully structured output from Phases 1–4 above. This is what gets cross-linked into the wiki.

After writing both files, run the **Brainstorm Session workflow** defined in `CLAUDE.md`:
- Create or update `wiki/ideas/<idea-slug>.md` for each `active` or `parked` idea
- Create or update `wiki/themes/<theme-slug>.md` for each theme identified
- Add cross-links between summary, idea, and theme pages
- Update `wiki/index.md` and append to `wiki/log.md`

If `wiki/index.md` or `wiki/log.md` do not yet exist, create them before updating.

---

## Behavior Notes

**Don't ask for input before starting.** The session history is the source. Read it and go. Only pause if something is genuinely ambiguous (e.g., two very similar ideas and it's unclear if they're the same thing).

**Be honest in scores.** A `1/1/1` idea shouldn't be inflated. The register is a working tool, not a validation document.

**Keep entries tight.** Each idea entry should be readable in 30 seconds. Depth goes in the Patterns section, not the per-idea entries.

**Session continuity.** Before generating a new register, read `wiki/index.md` (if it exists) to identify prior sessions. Check `wiki/ideas/` for any ideas that are `parked` — carry them forward and note if any resurfaced in this session. This is how the vault compounds across sessions.

**Downstream handoffs.** At the end of the register, note which ideas — if any — are ready for deeper work with a downstream skill:
- `startup-competitors` — if a market gap idea needs competitive validation
- `startup-positioning` — if differentiation is the live question
- `startup-design` — if an idea is ready for full build-out planning