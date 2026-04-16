# LLM Wiki — Variants

Pre-configured templates for specific use cases. Each variant is a `config.yaml` + schema + sample data + README tuned for one domain.

**One command to set up any variant:**

```bash
/llm-wiki setup <variant-name>
```

---

## Available Variants

### 📚 book-companion

> Read books. Drop notes. Get a personal wiki that thinks with you.

Build a companion wiki as you read — characters, timelines, factions, themes, all cross-linked automatically. Works for fiction (fantasy, sci-fi, literary) and non-fiction alike.

**Best for:** Complex novels, long non-fiction series, book clubs
**Unique commands:** `book-summary`, spoiler protection
**Sample data:** Dune (Frank Herbert), Chapters 1-3

```bash
/llm-wiki setup book-companion
/llm-wiki book-summary
```

[→ Full guide](book-companion/README.md)

---

### 🎯 competitive-intel

> Your competitors change. Your wiki should too.

Auto-track competitor pricing, features, and strategy. Get change alerts before you lose a deal. Generate battlecards on demand.

**Best for:** Startup founders, product managers, sales teams
**Unique commands:** `competitive-brief`, change detection
**Sample data:** PM tools market (Linear, Notion, market landscape)

```bash
/llm-wiki setup competitive-intel
/llm-wiki competitive-brief "Linear"
```

[→ Full guide](competitive-intel/README.md)

---

### 💼 job-search

> Research once. Know everything. Walk into every interview prepared.

Accumulate company research, JDs, Glassdoor intel, and Reddit threads — synthesize into a 1-pager when you need it most.

**Best for:** Engineers and PMs job searching, career switchers
**Unique commands:** `interview-prep`, application status tracking
**Sample data:** Stripe (Senior SWE) + Anthropic (Product Manager)

```bash
/llm-wiki setup job-search
/llm-wiki interview-prep "Stripe"
```

[→ Full guide](job-search/README.md)

---

## How Variants Work

Each variant extends the base llm-wiki with:

| File | Purpose |
|------|---------|
| `config.yaml` | Pre-tuned topics, feeds, discovery settings |
| `CLAUDE.md` | Schema extensions (new entity/concept types) |
| `sample-data/` | Ready-to-ingest demo files |
| `README.md` | Use case guide with examples |

The `setup` command handles everything:

```
/llm-wiki setup <name>
  ↓
Copy config.yaml → wiki root
Merge CLAUDE.md schema extensions
Copy sample-data → raw/articles/
Run ingest automatically
Print "Wiki ready" + suggested queries
```

---

## Contributing a Variant

Have a use case that works well? Contributions welcome.

A good variant needs:
1. `config.yaml` — tuned for the domain
2. `CLAUDE.md` — any new entity/concept types needed
3. `sample-data/` — 3-5 anonymized example files that demonstrate the use case
4. `README.md` — tagline, demo output, setup steps, example queries

Open a PR with your variant folder. See existing variants for the expected format.

---

## Roadmap

| Variant | Status | Domain |
|---------|--------|--------|
| book-companion | ✅ Available | Fiction/Non-fiction reading |
| competitive-intel | ✅ Available | Business / Startup |
| job-search | ✅ Available | Career |
| academic-research | 🔜 Planned | Research / Academia |
| health-tracker | 🔜 Planned | Personal health |
| trip-planner | 💡 Idea | Travel |
| language-learning | 💡 Idea | Education |

*Ideas and PRs for new variants are welcome.*
