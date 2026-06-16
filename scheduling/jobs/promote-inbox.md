# Job: Promote Inbox

You are running unattended in the idea-distillery vault. There is no human to
ask — process every staged capture and finish the job.

1. Read `AGENTS.md` for the vault schema and workflow definitions.
2. List the files in `_inbox/` (ignore `README.md`). If there are none, stop —
   nothing to do.
3. For each capture, execute the **Capture Inbox (promotion)** workflow exactly
   as defined in `AGENTS.md`:
   - Read the capture completely
   - Write an immutable log to `raw/brainstorm-YYYY-MM-DD.md` (use the capture
     date; append a short slug if multiple captures share a date)
   - Run the Brainstorm Session synthesis workflow against the new raw log
     (idea/theme/pattern pages, cross-links, index + log updates)
   - Delete the original item from `_inbox/` only once it is safely promoted
4. Commit your changes:
   `git add -A && git commit -m "chore(inbox): promoted <n> capture(s)"`
   (Skip the commit if there is nothing to commit.)

If a capture is ambiguous or looks half-finished, promote it anyway but set the
resulting pages to `confidence: low` and note the uncertainty — do not discard
it. Never modify existing files in `raw/`.
