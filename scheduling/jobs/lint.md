# Job: Lint / Health Check

You are running unattended in the idea-distillery vault. There is no human to
ask — fix what is safely fixable and report the rest in the commit body.

1. Read `AGENTS.md` for the vault schema and workflow definitions.
2. Execute the **Lint** workflow exactly as defined there:
   - Read all wiki pages
   - Check for: orphan pages (no inbound links), idea pages missing required
     sections, ideas without theme links, pattern pages with only one data point,
     stale open questions (≥3 sessions old and unchanged), and ideas marked
     `active` that haven't appeared in ≥4 sessions
   - Auto-fix what is safe: add missing cross-links, correct dates, repair
     formatting
   - Update `wiki/log.md`
3. Anything that needs human judgment (e.g. "should this idea be downgraded to
   `parked`?") goes into the commit message body as a checklist — do NOT make
   those judgment calls yourself.
4. Commit your changes:
   `git add -A && git commit -m "chore(lint): auto-fixes + issues for review"`
   (Skip the commit if there is nothing to commit.)

Do not modify anything in `raw/`. Never downgrade, discard, or delete an idea
automatically — surface it for review instead.
