# 🎯 LLM Wiki — Competitive Intelligence Variant

> Your competitors change. Your wiki should too.

Stop manually updating a Notion page nobody reads. This wiki automatically discovers competitor news, detects pricing changes, and generates battle-ready briefs — so you walk into every sales call knowing more than the other side.

---

## Demo

Drop 3 competitor overviews → get:

```
Wiki: 14 pages
Competitors (3): Linear, Notion, Asana
Products (3):    Linear Issues, Notion AI, Asana Goals
Market Maps (2): Engineering Tools, All-in-One Platforms
```

**Get a battlecard:**
```
/llm-wiki competitive-brief "Linear"

→ # Competitive Brief: Linear
  One-line: "Fast, opinionated PM for engineering teams"

  Pricing: Free ($0, 250 issues) | Standard ($8/user) | Plus ($14/user)

  Top Features: Speed (<50ms UI), Cycles, Git integration

  Recent Moves:
  - 2025-Q4: Launched "Linear Asks" async Q&A
  - 2025-Q3: AI issue summarization

  Weaknesses: No time tracking, weak reporting, not for non-eng teams

  Job Signal: Hiring Enterprise AEs → pushing upmarket

  How We Differ: [your differentiators here]
```

**Automatic change detection:**
```
[CHANGE DETECTED] Notion — 2026-04-10
  Pricing: Plus tier changed from $8 → $10/user/mo
  Significance: 25% price increase may drive price-sensitive SMBs to evaluate alternatives
```

---

## Who Is This For?

- **Startup founders** who need to know what competitors are shipping
- **Product managers** tracking feature gaps and positioning
- **Sales teams** who need fresh battlecards before every call
- **VCs / analysts** monitoring a market

---

## Setup (3 commands)

```bash
# 1. Clone the repo
git clone https://github.com/mduongvandinh/llm-wiki my-intel-wiki
cd my-intel-wiki

# 2. Set up competitive-intel variant (includes PM tools sample data)
/llm-wiki setup competitive-intel

# 3. Get your first battlecard
/llm-wiki competitive-brief "Linear"
```

---

## How to Use for Your Own Market

### Step 1 — Define your competitors in config.yaml

```yaml
competitors:
  - name: "Your Competitor"
    website: "https://their-site.com"
    track: [pricing, features, jobs, blog]
```

### Step 2 — Seed with initial data

Drop what you know about each competitor into `raw/articles/`:

```markdown
---
title: "CompetitorX — Overview"
competitor: "CompetitorX"
---

# CompetitorX

## Pricing
[What you know about their pricing]

## Features
[Their main features]

## Weaknesses
[What customers complain about]
```

Then: `/llm-wiki ingest`

### Step 3 — Run discovery

```bash
/llm-wiki run
```

The wiki auto-searches for new competitor news, pricing pages, job postings, and reviews. Ingests and updates competitor pages automatically.

### Step 4 — Schedule auto-updates

```bash
/loop 6h /llm-wiki run
```

Every 6 hours: discover → ingest → lint. You get change alerts when something important shifts.

### Step 5 — Generate battlecards

```bash
/llm-wiki competitive-brief "CompetitorX"
# → Saved to outputs/battlecard-competitorx-2026-04-14.md
```

---

## Change Detection

When the wiki re-discovers a source it has seen before, it compares the new content against the existing wiki page. If it detects significant changes (pricing, features, strategy), it:

1. Creates `wiki/changes/YYYY-MM-DD-competitor.md` with a diff
2. Marks the competitor entity page with `⚡ CHANGED`
3. Highlights the change in the next `/llm-wiki digest`

This means you find out about competitor pricing changes **before** you lose a deal to it.

---

## Key Commands

| Command | Description |
|---------|-------------|
| `/llm-wiki setup competitive-intel` | One-time setup with PM tools sample data |
| `/llm-wiki run` | Full cycle: discover + ingest + lint |
| `/llm-wiki competitive-brief "Name"` | Generate battlecard for one competitor |
| `/llm-wiki query "..."` | Ask anything about competitors |
| `/llm-wiki digest` | Daily summary of changes detected |
| `/llm-wiki status` | Wiki health and coverage summary |

---

## Example Queries That Work Great

```bash
/llm-wiki competitive-brief "Linear"
/llm-wiki query "How does Notion pricing compare to Linear?"
/llm-wiki query "What are the most common complaints about Notion?"
/llm-wiki query "Which competitors are pushing into enterprise?"
/llm-wiki query "What features do we have that competitors lack?"
/llm-wiki query "Recent changes detected in the last 30 days"
```

---

## Sample Data Included

This variant ships with sample data for the **project management tools market**:
- `competitor-linear-overview.md` — pricing, features, weaknesses, recent moves
- `competitor-notion-overview.md` — pricing, features, weaknesses, AI strategy
- `market-pm-tools-landscape.md` — full market map, trends, switching patterns

Replace with your own market data to get started.

---

## Customization

**Different market:** Update `topics` and `competitors` in `config.yaml`. Delete sample data. Add your own.

**More sources:** Add Reddit subreddits, GitHub orgs, RSS feeds in `config.yaml`.

**Different language:** Update `wiki.language` in `config.yaml`.

---

*Part of [llm-wiki](https://github.com/mduongvandinh/llm-wiki) — personal knowledge base that thinks for you.*
