# LLM Wiki — GitHub Copilot Instructions

You are maintaining a personal knowledge base using the LLM Wiki pattern.
The wiki builds itself from raw sources. You read sources, write structured wiki pages, maintain cross-references, detect contradictions, and keep everything current.
The human only reads the wiki and asks questions.

## Architecture

```
raw/        → Raw sources — READ ONLY, never modify these files
wiki/       → Wiki you write and maintain entirely
outputs/    → Query results, reports, battlecards, briefs
config.yaml → Topics, feeds, schedule settings
```

## Core Rules

1. **Never modify files in raw/**. This is the immutable source of truth.
2. **One .md file per entity/concept**. Filename: kebab-case.md
3. **Cross-reference** with `[[filename]]` wiki links. Every page needs ≥2 links.
4. **Update wiki/INDEX.md** every time you create or delete a wiki page.
5. **Log everything** to wiki/LOG.md: format `## [YYYY-MM-DD HH:mm] action | description`
6. **Never fabricate** — only write what's supported by raw sources. Cite everything.

## Ingest Workflow

When asked to ingest or process new sources:

1. Read this file + config.yaml for schema and settings
2. Check .discoveries/history.json for already-processed files
3. Find new files in raw/ not in history
4. For each new file:
   - Create source summary in wiki/sources/
   - Extract entities → create/update pages in wiki/entities/
   - Extract concepts → create/update pages in wiki/concepts/
   - Add [[cross-references]] to related existing pages
   - Flag contradictions with existing content (note both versions, don't delete old)
5. Update wiki/INDEX.md
6. Append entry to wiki/LOG.md
7. Add processed files to .discoveries/history.json

## Query Workflow

When asked a question about the wiki:

1. Read wiki/INDEX.md to find relevant pages
2. Read those pages (read enough for full context)
3. Synthesize answer with [[citations]] to wiki pages
4. If the answer is analytical or comparative → save to wiki/syntheses/ or outputs/
5. Append to wiki/LOG.md

Answer ONLY from wiki content. If wiki lacks info, say so and suggest what to discover.

## Lint Workflow

When asked to check wiki health:

1. Scan all files in wiki/
2. Check for: contradictions, orphan pages (nobody links to them), broken [[links]], stale claims, knowledge gaps
3. Save report to outputs/lint-YYYY-MM-DD.md
4. Update .discoveries/gaps.json with identified gaps
5. Append to wiki/LOG.md

## Discover Workflow

When asked to find new sources:

1. Read config.yaml → topics, keywords, feeds
2. Read .discoveries/gaps.json → knowledge gaps to fill
3. Read .discoveries/history.json → avoid duplicates
4. Search for relevant sources
5. Save fetched content to raw/articles/YYYY-MM-DD-slug.md with frontmatter:
   ```yaml
   ---
   title: "Title"
   url: "https://..."
   discovered: YYYY-MM-DD
   topic: "topic name"
   ---
   ```
6. Run ingest on newly discovered files

## Wiki Page Formats

### Entity (wiki/entities/name.md)
```markdown
---
type: entity
category: person | organization | tool | project
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: [raw-file-1.md]
---

# Entity Name

One-line description.

## Overview
[Details]

## Key Points
- [Points]

## Links
- [[related-concept]]
- [[related-entity]]

## Sources
- [Source name](../raw/articles/filename.md)
```

### Concept (wiki/concepts/name.md)
```markdown
---
type: concept
domain: ai | engineering | business | science | other
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
---

# Concept Name

One-line description.

## Definition
[Clear explanation]

## How It Works
[Details]

## Examples
[Concrete examples]

## Links
- [[related-concept]]

## Sources
- [Source](../raw/articles/filename.md)
```

### Source Summary (wiki/sources/name.md)
```markdown
---
type: source
format: article | paper | note | book | video | podcast
raw_path: raw/articles/filename.md
ingested: YYYY-MM-DD
---

# Source Title

## Summary
[2-3 paragraph summary]

## Key Takeaways
- [Most important points]

## Entities Mentioned
- [[entity-name]]

## Concepts Mentioned
- [[concept-name]]

## Notable Quotes
> "Important quote"
```

## Variant-Specific Behavior

### book-companion (config: book_mode: true)
- Check `chapter: N` in raw file frontmatter before ingesting
- Skip files where N > config.yaml `current_chapter` (spoiler protection)
- Extract: characters, locations, factions, events, quotes, themes
- "book-summary" command: generate cast table, timeline, themes, open questions

### competitive-intel (config: change_detection: true)
- On re-ingest: compare new content vs existing wiki page
- If changed: create wiki/changes/YYYY-MM-DD-name.md, mark page ⚡ CHANGED
- "competitive-brief [name]": generate battlecard (positioning, pricing, features, weaknesses, recent moves)

### job-search
- "interview-prep [company]": generate 1-pager (overview, culture, process, compensation, questions to ask)

## Example Prompts

```
"Read CLAUDE.md then ingest all new files in raw/articles/ into the wiki"
"Read CLAUDE.md and answer: what do we know about [topic]?"
"Read CLAUDE.md and run lint — check for contradictions and orphan pages"
"Read CLAUDE.md and generate a book-summary for the current wiki"
"Read CLAUDE.md and create a competitive-brief for Linear"
"Read CLAUDE.md and generate an interview-prep 1-pager for Stripe"
```
