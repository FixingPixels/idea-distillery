# idea-distillery

> An LLM-powered Obsidian vault that turns raw brainstorming sessions into a compounding knowledge base — automatically.

You talk. The agent writes. Over time, a structured second brain emerges from your raw sessions: interconnected ideas, recurring themes, emerging patterns, and a wiki that gets smarter the more you use it.

Inspired by [Andrej Karpathy's LLM Wiki concept](https://www.youtube.com/watch?v=zVEb19AwkqM): raw sources stay immutable, and the LLM maintains the structured knowledge layer on top of them.

---

## How it works

Every brainstorming session produces two things:

1. **A raw log** — an immutable record of the conversation, saved to `raw/`
2. **A wiki update** — the agent synthesizes the session into structured pages under `wiki/`: idea pages, theme pages, pattern pages, cross-links, and an updated index

The wiki is the agent's artifact. You never edit it directly. You brainstorm freely and let the system build up over time.

```
AGENTS.md / CLAUDE.md     ← the schema the agent follows (identical, for cross-tool support)
skills/
  brainstorm-synthesis/   ← the skill that runs the synthesis workflow
references/
  brainstorm-coach.md     ← in-session ideation rules (starts empty, self-populates)
  writing-coach.md        ← in-session prose rules (starts empty, self-populates)
_inbox/                   ← staging zone for quick captures (promote → raw/)
scheduling/               ← run the weekly review / lint / inbox jobs on a cadence
raw/                      ← immutable session logs (the agent writes, never edits)
wiki/
  index.md                ← master catalog of all pages
  log.md                  ← append-only activity log
  summaries/              ← one Idea Register per session   (+ _template.md)
  ideas/                  ← one page per distinct idea       (+ _template.md)
  themes/                 ← recurring problem spaces         (+ _template.md)
  patterns/               ← meta-observations about how you think (+ _template.md)
  journal/                ← weekly reflections               (+ _template.md)
  presentations/          ← Marp slide decks generated from wiki content
```

Each `wiki/<type>/_template.md` is the exact page format for that type — useful reference, and exempt from the wiki's own linking rules.

---

## Setup

### 1. Get the vault and open it in Obsidian

Copy this `idea-distillery/` folder anywhere you like, then in Obsidian choose **Open folder as vault** and select it. It ships with a minimal `.obsidian/` config so it opens cleanly (Graph view, backlinks, and search are pre-enabled).

> Prefer git? Initialize a repo in the folder (`git init`) so your second brain is versioned and portable.

### 2. Install the brainstorm-synthesis skill

The skill in `skills/brainstorm-synthesis/` is the engine that makes the wiki compound. Install it into your agent's skills location:

| Tool | Where to put it |
|------|-----------------|
| **Claude Code** | copy `skills/brainstorm-synthesis/` → `.claude/skills/brainstorm-synthesis/` |
| **Cursor** | copy `skills/brainstorm-synthesis/` → `.cursor/skills/brainstorm-synthesis/` |
| **Claude desktop (Cowork)** | zip the folder and install via **Settings → Plugins → Install from file** |

```bash
# Claude Code
mkdir -p .claude/skills && cp -R skills/brainstorm-synthesis .claude/skills/

# Cursor
mkdir -p .cursor/skills && cp -R skills/brainstorm-synthesis .cursor/skills/

# Claude desktop (Cowork) — build the installable zip
cd skills && zip -r ../brainstorm-synthesis.zip brainstorm-synthesis && cd ..
```

`AGENTS.md` (and the identical `CLAUDE.md`) is loaded automatically as project instructions by agents that look for it — no extra configuration needed. If your tool doesn't auto-load it, point your project's system/instructions at `AGENTS.md`.

### 3. (Optional) Obsidian plugins worth enabling

- **Dataview** — query your ideas and themes as a database; makes the wiki significantly more powerful
- **Graph view** — already enabled; great for seeing idea connections
- **Marp Slides** — only if you want the agent to generate presentation decks from your wiki content

### 4. Start clean

The vault ships with a small **worked example** (a sample session and the pages it produced) so you can see the format and cross-linking before your first session. Every example file carries `example: true` in its frontmatter.

When you're ready, wipe it. The easiest way is to tell your agent:

> *"Start clean — remove all the example content and reset the index and log."*

Or do it manually:

```bash
# delete every file flagged as example content (including the sample raw log)
grep -rl '^example: true' . | xargs rm
```

Then clear the **(example)** rows from `wiki/index.md` and the example entries from `wiki/log.md`. (Asking the agent handles all of this in one step.)

---

## Usage

### Starting a session

Just start talking. There's no special command — describe an idea, a problem you keep turning over, a question you can't drop. The agent engages as a thinking partner, applying the in-session rules in `references/brainstorm-coach.md` (empty at first; it populates as patterns emerge).

### Ending a session and triggering synthesis

When you're done, say something like:

> *"Let's synthesize this session"* or *"Wrap this up and update the wiki"*

The agent will:
- Save the session log to `raw/`
- Create or update idea pages, theme pages, and the session summary in `wiki/`
- Update the index and activity log
- Surface any cross-session patterns that have emerged

### Other commands

| What you say | What happens |
|---|---|
| `"weekly review"` | Creates a journal entry summarizing recent sessions and active ideas |
| `"ingest [filename]"` | Processes a raw log (or `_inbox/` capture) that hasn't been synthesized yet |
| `"lint"` | Audits the wiki for orphan pages, missing links, stale open questions |
| `"what's the state of [idea]"` | Pulls the current idea page and gives you a summary |
| `"what patterns are you seeing"` | Synthesizes cross-session meta-observations |

---

## The wiki schema

Each page type follows a defined structure (full details in `AGENTS.md`):

- **Summaries** — the Idea Register for a session: all ideas, viability scores, key insights, patterns observed
- **Ideas** — concept, history across sessions, viability assessment, open questions, related ideas and themes
- **Themes** — recurring problem spaces with all related ideas and what they tend to get right or wrong
- **Patterns** — cross-session insights about how you think; require at least two sessions of evidence before creation
- **Journal** — weekly reflections covering ideas that evolved, emerging patterns, and next session focus

The two **coach files** in `references/` are the system's self-improvement loop: as confirmed patterns accumulate in `wiki/patterns/`, the agent distills them into active in-session rules. They start empty and earn their rules over time.

---

## Git workflow

Since the agent writes files during sessions, committing after each synthesis keeps your history clean and your ideas versioned:

```bash
git add wiki/ raw/
git commit -m "session: [brief description of what you explored]"
```

This also makes the vault a portable, shareable second brain you can take anywhere.

---

## Scheduled jobs (optional)

Some workflows are worth running on a cadence rather than by hand — a **weekly
review**, a periodic **lint**, or **promoting** whatever piled up in `_inbox/`.

If you use **Claude Cowork**, just ask: *"every Monday, run the weekly review."*
The built-in scheduler handles it.

Otherwise, the `scheduling/` folder makes the same jobs run unattended anywhere.
The pattern: each job is a prompt in `scheduling/jobs/*.md` (the single source of
truth), and you point any scheduler at it —

- **GitHub Actions** — recommended if the vault is a git repo; a ready
  `scheduling/github-actions/weekly-review.yml` runs on a cron and commits wiki
  edits back, no always-on machine needed.
- **Claude Code headless + cron / launchd / Task Scheduler** — run the job
  locally with one command from the vault directory.
- **Direct API script** — fallback if you don't run Claude Code.

Full setup, copy-paste snippets for each platform, and the job→cadence table are
in [`scheduling/README.md`](scheduling/README.md).

---

## Philosophy

Most note-taking systems require you to do the synthesis. You capture raw thoughts, then spend time organizing, tagging, linking, and summarizing — work that often doesn't happen.

idea-distillery inverts this. The raw session is the only thing you produce. Everything structured — the wiki pages, the cross-links, the pattern observations — is generated and maintained by the agent. Your job is to think out loud. The system's job is to remember, organize, and connect.

Over time the wiki becomes genuinely useful: a living record of how your ideas evolved, what themes keep pulling your attention, and which ideas survived repeated scrutiny.

---

## Credits

Built on [Andrej Karpathy's LLM Wiki concept](https://www.youtube.com/watch?v=zVEb19AwkqM) — the insight that an LLM should maintain the structured knowledge layer while raw sources remain immutable.
