# Scheduled jobs

Some vault workflows are worth running on a cadence rather than by hand — a
**weekly review**, a periodic **lint**, or **promoting** whatever piled up in
`_inbox/`. These aren't a special feature; they're the same workflows defined in
`AGENTS.md`, just run unattended.

The trick to making this portable is to separate the two things a scheduler
fuses together:

- **What runs** — a prompt, kept in `scheduling/jobs/*.md`. This is the single
  source of truth. Every backend below feeds the *same* file to the agent.
- **What triggers it** — a scheduler. Pick whichever fits your setup.

| Job | Prompt file | Suggested cadence |
|-----|-------------|-------------------|
| Weekly review | `jobs/weekly-review.md` | Mondays |
| Lint / health check | `jobs/lint.md` | Monthly |
| Promote inbox | `jobs/promote-inbox.md` | Daily, or as captures arrive |

---

## If you use Claude Cowork

You don't need any of this. Just tell the agent, e.g. *"every Monday at 9am, run
the weekly review job in `scheduling/jobs/weekly-review.md`."* Cowork's built-in
scheduler handles the rest. The options below are for everyone else.

---

## Option A — GitHub Actions (recommended if the vault is a git repo)

Best fit for a shared/versioned vault: scheduling lives in the repo, no machine
has to stay awake, and wiki edits are committed back automatically.

A ready workflow is in [`github-actions/weekly-review.yml`](github-actions/weekly-review.yml).
Setup:

1. Push the vault to a GitHub repo.
2. Add a repo secret `ANTHROPIC_API_KEY`
   (**Settings → Secrets and variables → Actions**).
3. Copy `scheduling/github-actions/weekly-review.yml` to
   `.github/workflows/weekly-review.yml` in the repo root.

To add the lint or inbox jobs, copy that file and change three things: the
`cron:` schedule, the `jobs/*.md` path in the prompt, and the commit message.

> **Cron-auth caveat.** Scheduled runs don't receive the GitHub App's OIDC write
> token, which trips a `User does not have write access` error if you try to use
> the app-based auth path. The workflow avoids this by setting
> `permissions: contents: write` and authenticating with `ANTHROPIC_API_KEY`
> instead. Keep both and scheduled runs work.

---

## Option B — Claude Code headless + your OS scheduler

The most direct equivalent for someone working locally. The whole job is one
command run *from the vault directory* (so `AGENTS.md`/`CLAUDE.md` and the skill
load exactly as in an interactive session):

```bash
cd /path/to/idea-distillery
claude -p "$(cat scheduling/jobs/weekly-review.md)" \
  --permission-mode acceptEdits \
  --allowedTools "Read,Write,Edit,Glob,Grep,Bash"
```

Wrap that in whatever your OS uses. **Requirements:** Claude Code installed and
authenticated, and the machine awake at the scheduled time.

**cron (Linux / macOS)** — `crontab -e`, then:

```cron
# Weekly review, Mondays 09:00
0 9 * * 1 cd /path/to/idea-distillery && claude -p "$(cat scheduling/jobs/weekly-review.md)" --permission-mode acceptEdits --allowedTools "Read,Write,Edit,Glob,Grep,Bash" >> scheduling/last-run.log 2>&1
```

**launchd (macOS, survives reboots / catches missed runs)** — save as
`~/Library/LaunchAgents/com.idea-distillery.weekly-review.plist`, then
`launchctl load` it:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key><string>com.idea-distillery.weekly-review</string>
  <key>WorkingDirectory</key><string>/path/to/idea-distillery</string>
  <key>ProgramArguments</key>
  <array>
    <string>/bin/bash</string>
    <string>-lc</string>
    <string>claude -p "$(cat scheduling/jobs/weekly-review.md)" --permission-mode acceptEdits --allowedTools "Read,Write,Edit,Glob,Grep,Bash"</string>
  </array>
  <key>StartCalendarInterval</key>
  <dict><key>Weekday</key><integer>1</integer><key>Hour</key><integer>9</integer><key>Minute</key><integer>0</integer></dict>
</dict>
</plist>
```

**Task Scheduler (Windows)** — create a task, trigger *Weekly → Monday → 9:00*,
action *Start a program*:

```
Program/script:  cmd.exe
Arguments:       /c cd /d C:\path\to\idea-distillery && claude -p "%CD%\scheduling\jobs\weekly-review.md content" --permission-mode acceptEdits
```

(Windows `cmd` can't `cat` inline — easiest is to point `-p` at the file's text
via PowerShell: `claude -p (Get-Content -Raw scheduling\jobs\weekly-review.md) ...`.)

---

## Option C — Direct API script (no Claude Code)

If you don't run Claude Code at all, a small script can hit the Anthropic API
with the job prompt plus the vault files and write back the results, scheduled by
any of the triggers above. This is the most assembly and the least recommended —
reach for A or B first.

---

## Notes

- Each job prompt tells the agent to **commit its own changes**, so every backend
  leaves a clean git trail. Option A pushes; for B you may want a `git push` step
  too if you sync to a remote.
- Keep the prompts in `scheduling/jobs/` as the source of truth. If you change a
  workflow in `AGENTS.md`, the prompts pick it up automatically — they reference
  the workflow by name rather than restating it.
- Unattended runs can't ask you questions, so the prompts are written to make
  safe defaults and to *surface* (never auto-decide) anything that needs human
  judgment.
