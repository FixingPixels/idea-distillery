# idea-distillery

> A Claude-powered Obsidian vault that turns raw brainstorming sessions into a compounding knowledge base — automatically.

You talk. Claude writes. Over time, a structured second brain emerges from your raw sessions: interconnected ideas, recurring themes, emerging patterns, and a wiki that gets smarter the more you use it.

Inspired by [Andrej Karpathy's LLM Wiki concept](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f), built for thinkers who want a system that does the synthesis work for them.

---

## How it works

Every brainstorming session produces two things:

1. **A raw log** — an immutable record of the conversation, saved to `raw/`
2. **A wiki update** — Claude synthesizes the session into structured pages under `wiki/`: idea pages, theme pages, pattern pages, cross-links, and an updated index

The wiki is Claude's artifact. You never edit it directly. You just brainstorm freely and let the system build up over time.

```
raw/                    ← immutable session logs (Claude writes, never edits)
wiki/
  index.md             ← master catalog of all pages
  log.md               ← append-only activity log
  summaries/           ← one Idea Register per session
  ideas/               ← one page per distinct idea, updated across sessions
  themes/              ← recurring problem spaces that span multiple sessions
  patterns/            ← meta-observations about how you think
  journal/             ← weekly reflections synthesizing recent activity
  presentations/       ← Marp slide decks generated from wiki content
```

---

## Setup

### 1. Clone the repo and open it as an Obsidian vault

```bash
git clone https://github.com/your-username/idea-distillery.git
```

Open Obsidian → **Open folder as vault** → select the cloned directory.

### 2. Install the brainstorm-synthesis skill

**Option A — Claude desktop app (Cowork mode, recommended)**

Install the skill from the zip file in the repo root:

1. In the Claude desktop app, go to **Settings → Plugins**
2. Click **Install from file** and select `brainstorm-synthesis.zip`
3. Create a new project and set your vault folder as the workspace directory

The `CLAUDE.md` in the repo root is automatically loaded as project instructions — no additional configuration needed.

**Option B — Claude Code (CLI)**

The skill is also available at `./claude/skills/brainstorm-synthesis` and will be picked up automatically by Claude Code when run from the vault directory.

### 3. (Optional) Install recommended Obsidian plugins

- **Dataview** — query your ideas and themes as a database; makes the wiki significantly more powerful
- **Graph view** — built-in, but worth enabling to visualize idea connections
- **Marp Slides** — required only if you want Claude to generate presentation decks from your wiki content

---

## Usage

### Starting a session

Just start talking. There's no special command to begin — describe an idea, a problem you're thinking about, or a question you can't stop turning over. Claude will engage with it as a thinking partner.

### Ending a session and triggering synthesis

When you're done, say something like:

> *"Let's synthesize this session"* or *"Wrap this up and update the wiki"*

Claude will:
- Save the session log to `raw/`
- Create or update idea pages, theme pages, and the session summary in `wiki/`
- Update the index and activity log
- Surface any cross-session patterns that have emerged

### Other commands

| What you say | What happens |
|---|---|
| `"weekly review"` | Creates a journal entry summarizing recent sessions and active ideas |
| `"ingest [filename]"` | Processes a raw log that hasn't been synthesized yet |
| `"lint"` | Audits the wiki for orphan pages, missing links, stale open questions |
| `"what's the state of [idea]"` | Pulls the current idea page and gives you a summary |
| `"what patterns are you seeing"` | Synthesizes cross-session meta-observations |

---

## The wiki schema

Each page type follows a defined structure:

- **Summaries** — the Idea Register for a session: all ideas, viability scores, key insights, patterns observed
- **Ideas** — concept, history across sessions, viability assessment, open questions, related ideas and themes
- **Themes** — recurring problem spaces with all related ideas and what they tend to get right or wrong
- **Patterns** — cross-session insights about how you think; require at least two sessions of evidence before creation
- **Journal** — weekly reflections covering ideas that evolved, emerging patterns, and next session focus

Full schema details are in `CLAUDE.md`.

---

## Git workflow

Since Claude writes files during sessions, committing after each synthesis step keeps your history clean and your ideas version-controlled:

```bash
git add wiki/ raw/
git commit -m "session: [brief description of what you explored]"
```

This also makes the repo a portable, shareable second brain you can take anywhere.

---

## Philosophy

Most note-taking systems require you to do the synthesis. You capture raw thoughts, then spend time organizing, tagging, linking, and summarizing them — work that often doesn't happen.

idea-distillery inverts this. The raw session is the only thing you produce. Everything structured — the wiki pages, the cross-links, the pattern observations — is generated and maintained by Claude. Your job is to think out loud. The system's job is to remember, organize, and connect.

Over time, the wiki becomes genuinely useful: a living record of how your ideas have evolved, what themes keep pulling your attention, and which ideas have survived repeated scrutiny.

---

## Credits

Built on [Andrej Karpathy's LLM Wiki concept](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f). The core insight — that an LLM should maintain the structured knowledge layer while raw sources remain immutable — is his.

The vault schema and workflow were adapted from [tonbistudio/llm-wiki](https://github.com/tonbistudio/llm-wiki), which translated Karpathy's concept into a working Obsidian implementation. idea-distillery extends that foundation with Claude-native brainstorming skills, structured idea evaluation, and cross-session pattern recognition.