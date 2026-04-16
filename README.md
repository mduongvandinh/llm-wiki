# LLM Wiki

**A personal knowledge base that builds and maintains itself.**

Drop articles, notes, PDFs, or Reddit threads into a folder. Your AI reads everything, builds a structured wiki with cross-references, detects contradictions between sources, and keeps it all up to date — automatically.

You just read and ask questions.

> Inspired by [Andrej Karpathy's LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f). Extended with auto-discovery, Reddit scanning, GitHub tracking, and ready-made variants for common use cases.

---

## Why not just use RAG?

| | Traditional RAG | LLM Wiki |
|---|---|---|
| Knowledge | Re-retrieved fresh every query | Compiled once, maintained continuously |
| Cross-references | None | Built and maintained automatically |
| Contradictions | Not detected | Flagged immediately on ingest |
| Compounding | Flat | Grows richer over time |
| Infrastructure | Vector DB + embeddings | Just folders + markdown |

---

## Quick Start

### Option A — Start with a ready-made variant (recommended)

Don't configure from scratch. Pick a variant that matches your use case — config, schema, and demo data are pre-tuned:

```bash
git clone https://github.com/mduongvandinh/llm-wiki.git my-wiki
cd my-wiki

# Install skill (Claude Code only — other tools skip this)
cp -r skills/llm-wiki ~/.claude/skills/llm-wiki

# Pick your variant:
/llm-wiki setup book-companion      # 📚 Reading companion
/llm-wiki setup competitive-intel   # 🎯 Track competitors
/llm-wiki setup job-search          # 💼 Job search research
```

### Option B — Start from scratch

```bash
git clone https://github.com/mduongvandinh/llm-wiki.git my-wiki
cd my-wiki

cp config.example.yaml config.yaml
# Edit config.yaml: set your topics, keywords, feeds
```

Then drop content into `raw/articles/` and run your first cycle.

---

## Variants

Pre-configured templates for specific use cases. One command setup, includes demo data.

| | Variant | Use Case | Key Command |
|---|---------|----------|-------------|
| 📚 | [book-companion](variants/book-companion/README.md) | Build a companion wiki as you read — characters, timeline, factions, lore auto-extracted | `book-summary` · [demo](demo/book-companion/) |
| 🎯 | [competitive-intel](variants/competitive-intel/README.md) | Track competitors, detect pricing/feature changes, generate battlecards | `competitive-brief "Linear"` |
| 💼 | [job-search](variants/job-search/README.md) | Research target companies, prep interview 1-pagers | `interview-prep "Stripe"` |

→ [See all variants →](variants/README.md)

---

## Using with Your AI Tool

LLM Wiki works with any AI coding assistant. The wiki is just folders and markdown — the AI reads `CLAUDE.md` (or `AGENTS.md`) to understand the schema and rules, then operates on files.

### Claude Code

The primary supported tool. Has a slash command skill for the best experience.

**Install the skill:**
```bash
cp -r skills/llm-wiki ~/.claude/skills/llm-wiki
```

**Commands:**
```bash
/llm-wiki setup book-companion     # Set up a variant
/llm-wiki run                       # Full cycle: discover → ingest → lint
/llm-wiki ingest                    # Process new files in raw/
/llm-wiki query "What is X?"        # Ask questions against the wiki
/llm-wiki discover                  # Auto-find new sources from the web
/llm-wiki lint                      # Health check: contradictions, orphans, gaps
/llm-wiki status                    # Overview of wiki state
/llm-wiki digest                    # Daily brief: new sources, insights, changes
/llm-wiki book-summary              # Structured book summary (book-companion variant)
/llm-wiki competitive-brief "Name"  # Battlecard for a competitor
/llm-wiki interview-prep "Company"  # Interview prep 1-pager
```

**Auto-run:**
```bash
/loop 1h /llm-wiki run    # Runs every hour in current session
/loop 2h /llm-wiki run    # Every 2 hours
```

---

### Cursor

Cursor reads `AGENTS.md` from the project root automatically. No installation needed.

**Open the project in Cursor, then chat:**
```
Follow AGENTS.md. Run full cycle: discover new sources, ingest into wiki, then lint.
```
```
Follow AGENTS.md. Ingest everything new in raw/ and update the wiki.
```
```
Follow AGENTS.md. Query the wiki: what do we know about [topic]?
```
```
Follow AGENTS.md. Run competitive-brief for "Linear" and save to outputs/.
```

---

### GitHub Copilot (VS Code)

Copilot reads `.github/copilot-instructions.md` automatically when you open the project. No installation needed.

**Open project in VS Code, then use Copilot Chat:**
```
Read CLAUDE.md then run discover, ingest, and lint for this wiki.
```
```
Based on the wiki schema in CLAUDE.md, ingest the new files in raw/articles/.
```
```
Query the wiki: summarize everything we know about [topic].
```

> **Note:** Copilot Chat doesn't support background loops. Run manually or use system scheduler.

---

### Windsurf (Codeium)

Windsurf reads `AGENTS.md` or `.windsurfrules` from the project root.

**Chat in Windsurf:**
```
Read AGENTS.md. Run the full llm-wiki cycle: discover → ingest → lint.
```
```
Read AGENTS.md. I've added new files to raw/articles/. Please ingest them into the wiki.
```

---

### Gemini CLI

Gemini CLI reads `GEMINI.md` from the project root (or falls back to `AGENTS.md`).

**Create `GEMINI.md`** (copy from `AGENTS.md`):
```bash
cp AGENTS.md GEMINI.md
```

**Then run:**
```bash
gemini "Read GEMINI.md and run full cycle: discover, ingest, lint"
gemini "Read GEMINI.md and query the wiki: what do we know about [topic]?"
```

---

### Codex CLI (OpenAI)

Codex reads `AGENTS.md` from the project root.

```bash
codex "Read AGENTS.md and run full cycle: discover → ingest → lint"
codex "Read AGENTS.md and ingest new files in raw/"
codex "Read AGENTS.md and query: summarize the wiki on [topic]"
```

---

### Aider

Aider works best with explicit file references.

```bash
aider --message "Read CLAUDE.md to understand the schema. Run ingest on all new files in raw/articles/. Update wiki/ accordingly." CLAUDE.md raw/articles/*.md
```

---

### Continue.dev

Add `CLAUDE.md` to Continue's context, then chat:
```
@CLAUDE.md Ingest the new articles in raw/ and update the wiki with entities, concepts, and cross-references.
```

---

### Tool Comparison

| Tool | Config File | Skill/Slash Command | Auto-loop | Best For |
|------|------------|-------------------|-----------|----------|
| Claude Code | `CLAUDE.md` | ✅ `/llm-wiki` | ✅ `/loop` | Full experience, all features |
| Cursor | `AGENTS.md` | ❌ chat only | ❌ | IDE-integrated workflow |
| GitHub Copilot | `.github/copilot-instructions.md` | ❌ chat only | ❌ | VS Code users |
| Windsurf | `AGENTS.md` | ❌ chat only | ❌ | VS Code alternative |
| Gemini CLI | `GEMINI.md` | ❌ chat only | via cron | Google ecosystem |
| Codex CLI | `AGENTS.md` | ❌ chat only | via cron | OpenAI preference |
| Aider | `CLAUDE.md` | ❌ explicit | via cron | Terminal-only |

> All tools use the same `raw/`, `wiki/`, `outputs/` folder structure. The AI config file tells each tool how to operate — same content, different filenames.

---

## Auto-Run Without Claude Code

For tools without `/loop`, use your system scheduler.

**Windows — Task Scheduler** (run once as Admin):
```powershell
powershell -ExecutionPolicy Bypass -File "scripts/setup-scheduler.ps1"
```

**macOS / Linux — crontab:**
```bash
# Claude Code (headless)
0 */2 * * * cd /path/to/my-wiki && claude --print "/llm-wiki run" >> outputs/auto-run.log 2>&1

# Codex
0 */2 * * * cd /path/to/my-wiki && codex "Read AGENTS.md and run full cycle" >> outputs/auto-run.log 2>&1

# Gemini CLI
0 */2 * * * cd /path/to/my-wiki && gemini "Read GEMINI.md and run full cycle" >> outputs/auto-run.log 2>&1
```

---

## How It Works

```
┌─────────────────────────────────────────────────┐
│              YOU  (read & ask)                   │
├──────────────┬──────────────┬────────────────────┤
│   wiki/      │   outputs/   │   wiki-viewer.html │
│  (Obsidian)  │  (reports)   │   (browser view)   │
├──────────────┴──────────────┴────────────────────┤
│              LLM  (writes & maintains)            │
├──────────┬──────────┬──────────┬─────────────────┤
│ discover │  ingest  │  query   │      lint        │
│ (find)   │ (process)│ (answer) │  (health check)  │
├──────────┴──────────┴──────────┴─────────────────┤
│   raw/        │  config.yaml   │   CLAUDE.md      │
│  (sources)    │  (settings)    │   (schema/rules) │
└───────────────┴────────────────┴──────────────────┘
```

**Main flow:** `discover` → `raw/` → `ingest` → `wiki/` → `query` → `outputs/`

**Self-improvement loop:** `lint` finds gaps → `discover` finds sources → `ingest` fills them → wiki grows richer

### Core Workflows

**`discover`** — Searches the web for new sources based on your topics in `config.yaml`. Can scan Reddit for pain points, track GitHub trending repos, follow RSS feeds, and snowball from references found in existing sources.

**`ingest`** — Reads each new file in `raw/`. Extracts entities, concepts, and key claims. Creates wiki pages. Adds cross-references. Flags contradictions with existing content. One source typically creates 5–15 wiki pages.

**`query`** — Reads `wiki/INDEX.md`, finds relevant pages, synthesizes an answer with citations. Valuable answers are automatically saved as synthesis pages.

**`lint`** — Scans the entire wiki for: contradictions, orphan pages, broken links, stale claims, and knowledge gaps. Updates `.discoveries/gaps.json` so `discover` knows what to find next.

---

## Adding Sources

### Drop a file manually

Create a markdown file in `raw/articles/` with frontmatter, then run `ingest`:

```markdown
---
title: "Article Title"
url: "https://example.com/article"
author: "Author Name"
discovered: 2026-04-14
topic: "your topic"
---

Article content here...
```

For **book notes** (book-companion variant), add `chapter: N`:
```markdown
---
title: "Dune Chapter 5 Notes"
chapter: 5
book: "Dune"
---
```

For **competitor data** (competitive-intel variant), add `competitor: "Name"`:
```markdown
---
title: "Linear — Pricing Page"
competitor: "Linear"
---
```

### Obsidian Web Clipper

1. Install [Obsidian Web Clipper](https://obsidian.md/clipper) browser extension
2. See an interesting article → click clip → save to `raw/articles/`
3. Run ingest

### Auto-discovery

Configure `config.yaml` with topics, Reddit subreddits, GitHub orgs, and RSS feeds — the `discover` command handles the rest.

---

## Viewing Your Wiki

**Obsidian** (recommended): `File → Open vault → select your wiki folder` → Graph View shows connections

**Browser**: Open `wiki-viewer.html` — mobile-first dark theme dashboard with graph view

**Any markdown editor**: All wiki pages are plain `.md` files

---

## Folder Structure

```
my-wiki/
├── CLAUDE.md                    ← Schema + rules for Claude Code
├── AGENTS.md                    ← Schema + rules for Cursor, Codex, Windsurf, Aider
├── GEMINI.md                    ← Schema + rules for Gemini CLI (copy of AGENTS.md)
├── .github/
│   └── copilot-instructions.md  ← Schema for GitHub Copilot
├── config.yaml                  ← Your topics, feeds, schedule
├── config.example.yaml          ← Template to start from
├── wiki-viewer.html             ← Browser-based wiki viewer
│
├── variants/                    ← Ready-made use case templates
│   ├── book-companion/          ← 📚 Reading companion
│   ├── competitive-intel/       ← 🎯 Competitor tracking
│   └── job-search/              ← 💼 Job search research
│
├── raw/                         ← YOUR SOURCES (LLM never modifies these)
│   ├── articles/                ← Web articles, blog posts, papers
│   ├── reddit/                  ← Reddit threads, pain points
│   ├── notes/                   ← Personal notes
│   └── media/                   ← Screenshots, diagrams
│
├── wiki/                        ← LLM-maintained wiki (don't edit manually)
│   ├── INDEX.md                 ← Master catalog of all wiki pages
│   ← LOG.md                     ← Activity log
│   ├── entities/                ← People, orgs, tools, projects
│   ├── concepts/                ← Ideas, patterns, methodologies
│   ├── sources/                 ← Summary of each raw source
│   └── syntheses/               ← Analysis, comparisons, rankings
│
├── outputs/                     ← Query results, reports, battlecards, briefs
├── .discoveries/                ← Auto-discovery metadata
│   ├── feeds.json               ← Sources being tracked
│   ├── gaps.json                ← Knowledge gaps to fill
│   └── history.json             ← Processed sources (dedup)
│
└── skills/llm-wiki/SKILL.md     ← Claude Code skill definition
```

---

## Configuration

All behavior is controlled by `config.yaml`. Copy `config.example.yaml` to get started.

Key sections:

```yaml
wiki:
  name: "My Wiki"
  language: "en"   # Any language — wiki content will be in this language

topics:
  - name: "Topic Name"
    keywords: ["keyword1", "keyword2"]
    priority: high

feeds:
  reddit:
    enabled: true
    subreddits_pain_points:
      - sub: "r/SaaS"
  hackernews:
    enabled: true
    min_score: 50
  github_trending:
    enabled: true
    languages: ["python", "typescript"]

schedule:
  run:
    loop_interval: "1h"   # For /loop in Claude Code
```

→ See `config.example.yaml` for the full reference with all options.

---

## Commands Reference

| Command | Description |
|---------|-------------|
| `setup <variant>` | Initialize wiki from a variant template (book-companion, competitive-intel, job-search) |
| `init "Topic"` | Add a new topic to an existing wiki |
| `ingest` | Process all new files in `raw/` |
| `query "..."` | Answer a question using the wiki; saves valuable answers |
| `discover` | Auto-find new sources from the web based on topics |
| `run` | Full cycle: discover → ingest → lint |
| `lint` | Health check: contradictions, orphans, gaps, stale content |
| `status` | Show wiki stats and health overview |
| `digest` | Daily brief: new sources, top insights, detected changes |
| `pain-rank` | Rank pain points by business opportunity (Reddit data) |
| `book-summary` | Structured book summary: cast, timeline, themes, mysteries |
| `competitive-brief "Name"` | Battlecard 1-pager for a specific competitor |
| `interview-prep "Company"` | Interview prep 1-pager for a target company |

For tools without slash commands, use the equivalent natural language prompt:
```
Read CLAUDE.md (or AGENTS.md). Then [describe what you want].
```

---

## FAQ

**What AI model do I need?**
Any capable model with tool use (web search, file read/write). Claude Sonnet/Opus recommended. Works with GPT-4o, Gemini 1.5 Pro, and others via their respective CLI tools.

**How much does it cost to run?**
A typical `/llm-wiki run` cycle (discover + ingest 5 sources + lint) costs $0.10–0.40 depending on wiki size and source length. Running every 2 hours = ~$50/month.

**My wiki is getting large. How do I query it efficiently?**
The skill reads `wiki/INDEX.md` first to find relevant pages, then reads only those. Even large wikis (500+ pages) query efficiently because you never load everything at once.

**Can I use this for a team?**
Yes — put the wiki folder in a shared Git repo. Each team member runs ingest on their own sources; the wiki is a shared output. Conflicts are resolved by `lint`.

**Does it hallucinate?**
The query and ingest commands only use information from your `raw/` sources. They cannot add claims not supported by your sources. If something is uncertain, the wiki page will say so.

**Can I write in a language other than English?**
Yes. Set `wiki.language` in `config.yaml`. The LLM will write all wiki content in that language while keeping file names and frontmatter in English.

---

## Credits

- Original pattern: [Andrej Karpathy — LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
- Inspiration: [Nick Spisak — How to Build Your Second Brain](https://x.com/NickSpisak_/status/2040448463540830705)
- Built for: [Claude Code](https://claude.ai/code) (Anthropic) + compatible with all major AI coding assistants
- Wiki viewer: [Obsidian](https://obsidian.md/)

## License

MIT — use it, fork it, build variants, share back.
