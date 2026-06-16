# _inbox — Capture Staging

Drop zone for raw, unprocessed brainstorm captures — quick notes, phone dumps, voice
memos transcribed to text, half-formed ideas. Anything caught on the go lands here
first, in whatever shape it arrives. No formatting required.

This is **staging**, not the archive. It is deliberately separate from `raw/`, which
holds immutable, dated, processed session logs.

## How items get here

- **Manual:** paste or write a `.md` file directly into this folder.
- **On the go:** any capture pipeline you wire up (a phone shortcut, an email-to-file
  hook, a sync from another app) can drop files here.

## Promotion (inbox → vault)

When an inbox item is ingested (on demand, or flagged by the weekly review):

1. Read the capture completely.
2. Promote it to an immutable log at `raw/brainstorm-YYYY-MM-DD.md` (use the capture
   date; if multiple captures share a date, append a short slug:
   `raw/brainstorm-YYYY-MM-DD-<slug>.md`).
3. Run the **brainstorm-synthesis** skill / Brainstorm Session workflow against the
   new raw log (summary, ideas, themes, cross-links, index + log updates).
4. Remove the original item from `_inbox/` once it is safely promoted.

Items left in `_inbox/` are unprocessed by definition. The weekly review notes any
that have been sitting here so nothing rots.

> Files in this folder are **not** part of the immutable record until promoted to
> `raw/`. Edit or delete them freely while they are still here.
