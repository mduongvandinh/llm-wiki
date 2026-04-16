# LLM Wiki — Gemini Instructions

> For: Gemini CLI (`gemini` command)
> This file is the Gemini-specific version of AGENTS.md.

You are maintaining a personal knowledge base using the LLM Wiki pattern.
The wiki builds itself: you read raw sources, write structured wiki pages, maintain cross-references, detect contradictions, and keep everything up to date.
The human only reads the wiki and asks questions.

## How to Use This File

When the user runs `gemini "Read GEMINI.md and [task]"`, you should:
1. Read this entire file first to understand the schema and rules
2. Read `config.yaml` to understand the user's topics and settings
3. Execute the requested task following the workflows below

## Architecture

```
raw/        → Raw sources — READ ONLY, never modify
wiki/       → Wiki you write and maintain
outputs/    → Query results, reports, briefs
config.yaml → Your topics, feeds, and schedule
```

## Core Rules

- **Never modify raw/**. This is the source of truth.
- **One file per topic** in wiki/. Filename: kebab-case.md
- **Cross-reference** with `[[filename]]` links. Every page needs ≥2 links.
- **Update INDEX.md** every time you create or delete a wiki page.
- **Log everything** to wiki/LOG.md.
- **Never fabricate** — only write what's supported by raw sources.

## Workflows

### Full Cycle
```
discover new sources → ingest into wiki → lint health check → report
```

### Ingest
For each new file in raw/ (not in .discoveries/history.json):
1. Create source summary → wiki/sources/
2. Extract entities → wiki/entities/
3. Extract concepts → wiki/concepts/
4. Add cross-references to related pages
5. Flag contradictions with existing content
6. Update wiki/INDEX.md and wiki/LOG.md
7. Add to .discoveries/history.json

### Query
1. Read wiki/INDEX.md → find relevant pages
2. Read those pages
3. Synthesize answer with [[citations]]
4. Save valuable analyses to wiki/syntheses/

### Lint
Check wiki/ for: contradictions, orphan pages, broken links, stale claims, knowledge gaps.
Save report to outputs/lint-YYYY-MM-DD.md. Update .discoveries/gaps.json.

### Discover
Search web for sources matching topics in config.yaml and gaps in .discoveries/gaps.json.
Save fetched content to raw/articles/YYYY-MM-DD-slug.md. Then run ingest.

## Wiki Page Templates

See AGENTS.md for full templates (entity, concept, source summary, synthesis pages).
GEMINI.md intentionally stays concise — refer to AGENTS.md for detailed templates.

## Variant Modes

**book_mode: true** (book-companion variant)
- Check `chapter: N` in raw file frontmatter
- Skip files where N > config.current_chapter (spoiler protection)
- Extract: characters, locations, factions, events, quotes, themes
- Command: "Generate book-summary" → structured cast + timeline + themes

**change_detection: true** (competitive-intel variant)
- On re-ingest of known source: compare new vs existing wiki page
- If changed: create wiki/changes/YYYY-MM-DD-entity.md, mark page ⚡ CHANGED
- Command: "competitive-brief for [name]" → battlecard 1-pager

**job-search variant**
- Command: "interview-prep for [company]" → interview prep 1-pager

## Example Gemini CLI Commands

```bash
# Full cycle
gemini "Read GEMINI.md and config.yaml. Run full cycle: discover, ingest, lint. Report what changed."

# Ingest only
gemini "Read GEMINI.md. Ingest all new files in raw/articles/ into the wiki."

# Query
gemini "Read GEMINI.md. Query the wiki: what do we know about [topic]? Cite your sources."

# Book companion
gemini "Read GEMINI.md. Generate a book-summary for the current wiki state."

# Competitive intel
gemini "Read GEMINI.md. Generate a competitive-brief for Linear."

# Job search
gemini "Read GEMINI.md. Generate an interview-prep 1-pager for Stripe."
```

## Auto-Run via Cron

```bash
# Every 2 hours
0 */2 * * * cd /path/to/my-wiki && gemini "Read GEMINI.md and run full cycle: discover, ingest, lint" >> outputs/auto-run.log 2>&1
```
