# Idea Register — Output Template

This is the canonical output format for the brainstorm-synthesis skill. Follow this structure exactly. Adapt section content, never section structure.

---

## Frontmatter (YAML)

```yaml
---
session: YYYY-MM-DD
idea_count: [total ideas including discarded]
active: [count of active ideas]
parked: [count of parked ideas]
discarded: [count of discarded ideas]
top_idea: "[idea name]"
tags: [brainstorm, idea-register]
---
```

---

## Document Structure

```markdown
# Idea Register — [Date]

## Ideas

### [Idea Name]
**Status:** active | parked | discarded | evolved  
**Lenses:** [comma-separated lens tags]  
**Viability:** [D/F/Di score, e.g. 2/3/1] *(omit for discarded)*

[2-4 sentence description of the idea as it stood at end of session. What is it, who is it for, what's the core value proposition. For evolved ideas, note what it started as.]

**Key insight:** [The sharpest thing said about this idea — a strength, a flaw, or a reframe. One sentence.]

**Why discarded:** [For discarded ideas only — the reason it was ruled out. Be specific.]

---

[Repeat for each idea]

---

## Patterns

### Recurring weaknesses
[2-4 sentences. What lenses consistently scored low or generated the most friction? Be specific — name the ideas.]

### Recurring strengths  
[2-4 sentences. What does this person think well about? What comes naturally?]

### Thematic through-line
[1-3 sentences. The underlying interest or problem driving this session, even if the ideas varied on the surface.]

### Most promising idea
**[Idea name]** — [2-3 sentences. Why this one. Honest, not flattering.]

### Most interesting discard *(omit if none)*
**[Idea name]** — [1-2 sentences. Why it was ruled out and why that might be worth revisiting.]

### Unasked question
[The single question that would most change the picture on the session's best idea. This is the call-to-action for next session.]

---

## Next Steps

| Idea | Recommended next skill | Why |
|------|----------------------|-----|
| [Idea name] | startup-competitors | [one-line reason] |
| [Idea name] | startup-positioning | [one-line reason] |
| [Idea name] | startup-design | [one-line reason] |

*(Omit table if no ideas are ready for downstream work.)*

---

## Session Notes *(optional)*
[Any context worth preserving — constraints the user mentioned, adjacent ideas that didn't make the cut, links or references dropped during conversation.]
```

---

## Example Entry

```markdown
### Tender — Financial Therapy Platform
**Status:** active  
**Lenses:** audience, market-gap, monetization, timing  
**Viability:** 3/2/2

A platform targeting solopreneurs experiencing financial anxiety — combining structured financial coaching with emotional support. Positioned between a financial advisor (too formal, too expensive) and therapy (not finance-specific). Core value: making money conversations feel safe rather than shameful.

**Key insight:** The audience is highly motivated and underserved, but the monetization model needs work — solopreneurs are price-sensitive and the ROI of "feeling better about money" is hard to quantify upfront.

---
```

---

## Formatting Rules

- Use `---` horizontal rules between idea entries
- Bold only labels (`**Status:**`, `**Key insight:**`, etc.) — never mid-sentence
- Viability scores as `D/F/Di` (Desirability / Feasibility / Distinctiveness) with a brief legend on first use
- Keep each idea entry under ~150 words
- Patterns section can be longer — this is where the depth lives
- No bullet lists inside idea entries — prose only
- Dates in ISO format (YYYY-MM-DD) throughout