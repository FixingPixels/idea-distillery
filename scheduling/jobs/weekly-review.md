# Job: Weekly Review

You are running unattended in the idea-distillery vault. There is no human to
ask — make reasonable decisions and finish the job.

1. Read `AGENTS.md` for the vault schema and workflow definitions.
2. Execute the **Weekly Review** workflow exactly as defined there:
   - Read `wiki/log.md` to identify sessions and changes this week
   - Read all `active` idea pages and all pattern pages
   - Create a journal entry `wiki/journal/YYYY-MM-DD.md` (today's date) covering
     sessions this period, ideas that evolved, patterns emerging, open questions,
     and next-session focus
   - Flag any `_inbox/` items that have been sitting unprocessed
   - Update `wiki/index.md` and `wiki/log.md`
3. If nothing has changed since the last review, write a short journal entry
   saying so rather than inventing activity.
4. Commit your changes:
   `git add -A && git commit -m "chore(weekly-review): journal entry for <date>"`
   (Skip the commit if there is nothing to commit.)

Do not modify anything in `raw/`. Do not delete files unless the workflow
explicitly calls for it.
