# LLM Wiki — Agent Instructions

> For: Cursor, Windsurf, Codex CLI, Aider, Continue.dev, and any agent that reads AGENTS.md
> Claude Code users: see CLAUDE.md and install the skill from skills/llm-wiki/

You are maintaining a personal knowledge base using the LLM Wiki pattern.
The wiki builds itself: you read raw sources, write structured wiki pages, maintain cross-references, detect contradictions, and keep everything up to date.
The human only reads the wiki and asks questions.

## 3-Layer Architecture

```
raw/        → Raw sources (IMMUTABLE — you READ ONLY, NEVER modify)
wiki/       → Wiki you write and maintain entirely
outputs/    → Query results, reports, battlecards, briefs
config.yaml → Topics, feeds, schedule
```

## Golden Rules

1. **raw/ is immutable.** Never edit, delete, or move files in raw/. It is the source of truth.
2. **You own wiki/ entirely.** The human does not manually edit wiki pages.
3. **One file per topic.** Each entity, concept, and source summary is a separate .md file. Filename: kebab-case.
4. **Cross-reference everything.** Use `[[filename]]` links. Every page needs at least 2 links to other pages.
5. **Keep INDEX.md current.** Create/delete a wiki page → update INDEX.md immediately.
6. **Log all activity.** Every action gets an entry in wiki/LOG.md.

## How to Run Each Workflow

When the user asks you to run the wiki, follow these workflows. Use natural language descriptions of what you're doing — you don't have a slash command, just execute the steps directly.

### Run full cycle (discover → ingest → lint)

1. Read `config.yaml` to understand topics, feeds, and settings
2. Run **discover**: search for new sources based on topics/gaps
3. Run **ingest**: process all new files in `raw/`
4. Run **lint**: health check the wiki
5. Report summary

### Ingest

1. Read `config.yaml` and `AGENTS.md` (this file) for schema and rules
2. Read `.discoveries/history.json` → get list of already-processed files
3. Scan `raw/` → find files not in history
4. For each new file:
   a. Read full content
   b. Create source summary → `wiki/sources/`
   c. Extract entities → create/update `wiki/entities/`
   d. Extract concepts → create/update `wiki/concepts/`
   e. Add `[[cross-references]]` to related pages
   f. Flag contradictions with existing content
5. Update `wiki/INDEX.md`
6. Append to `wiki/LOG.md`
7. Update `.discoveries/history.json`

**Rules:**
- Never fabricate information — only write what's in the raw sources
- One source typically creates 5–15 wiki pages
- If new info contradicts existing → keep both, note the contradiction explicitly
- Always cite sources: `[Source: filename](../raw/path)`

### Query

1. Read `wiki/INDEX.md` → find relevant pages
2. Read those wiki pages (read enough context, not just 1-2 pages)
3. Synthesize answer with citations `[[wiki-page]]`
4. If the answer has analytical value → save to `wiki/syntheses/` or `outputs/`
5. Append to `wiki/LOG.md`

**Rules:**
- Answer ONLY from wiki content, not from external knowledge
- If wiki lacks info → say so and suggest what to discover
- Comparisons, analyses → automatically save as synthesis pages

### Lint

1. Read all files in `wiki/`
2. Check for:
   - **Contradictions**: conflicting claims between pages
   - **Orphans**: pages nobody links to
   - **Missing pages**: `[[links]]` pointing to non-existent files
   - **Stale claims**: old info contradicted by newer sources
   - **Broken links**: links to raw sources that no longer exist
   - **Gaps**: important areas with no coverage
   - **Quality**: pages too short, missing sources, missing cross-refs
3. Save report → `outputs/lint-YYYY-MM-DD.md`
4. Update `.discoveries/gaps.json`
5. Append to `wiki/LOG.md`

### Discover

1. Read `config.yaml` → topics, feeds, discovery settings
2. Read `.discoveries/gaps.json` → knowledge gaps to fill
3. Read `.discoveries/history.json` → avoid re-processing
4. Search web for sources based on topics + gaps
5. Fetch content → save to `raw/articles/YYYY-MM-DD-slug.md` with frontmatter:
   ```yaml
   ---
   title: "Title"
   url: "https://..."
   discovered: YYYY-MM-DD
   topic: "topic name"
   ---
   ```
6. Update `.discoveries/history.json`
7. Trigger ingest on new sources

**Rules:**
- Max sources per cycle: see `config.yaml → schedule.run.max_sources`
- Priority: gaps > pain points > trending > feeds
- Skip: ads, low-quality listicles, duplicates
- Never re-fetch URLs already in history.json

## Wiki Page Formats

### Entity (wiki/entities/)
```markdown
---
type: entity
category: person | organization | tool | project
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: [list of raw source files]
---

# Entity Name

One-line description.

## Overview
[Details]

## Key Points
- [Bullet points]

## Links
- [[related-concept]]
- [[related-entity]]

## Sources
- [Source 1](../raw/articles/filename.md)
```

### Concept (wiki/concepts/)
```markdown
---
type: concept
domain: ai | engineering | business | ...
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
---

# Concept Name

One-line description.

## Definition
[Clear explanation]

## How It Works
[Technical details if applicable]

## Examples
[Concrete examples]

## Links
- [[related-concept]]

## Sources
- [Source 1](../raw/articles/filename.md)
```

### Source Summary (wiki/sources/)
```markdown
---
type: source
format: article | paper | note | video | podcast
raw_path: raw/articles/filename.md
ingested: YYYY-MM-DD
---

# Source Title

## Summary
[2-3 paragraph summary]

## Key Takeaways
- [Most important points]

## Entities Mentioned
- [[entity-1]]

## Concepts Mentioned
- [[concept-1]]

## Notable Quotes
> "Important quote from the source"
```

### Synthesis (wiki/syntheses/)
```markdown
---
type: synthesis
topic: analysis topic
created: YYYY-MM-DD
sources_count: N
---

# Analysis Title

## Question
[The question or purpose of this analysis]

## Analysis
[Synthesized content from multiple sources]

## Conclusion
[Summary of findings]

## Sources Used
- [[source-1]]
- [[source-2]]
```

## Variant-Specific Instructions

If `config.yaml` has `book_mode: true`:
- Check each raw file for `chapter: N` frontmatter
- Skip files where `N > config.current_chapter` (spoiler protection)
- Log: `[SKIP] filename — chapter N exceeds current_chapter M`
- Extract: characters, locations, factions, events, quotes, themes

If `config.yaml` has `change_detection: true`:
- When re-ingesting a known source, compare new content to existing wiki page
- If significant change detected → create `wiki/changes/YYYY-MM-DD-entity.md`
- Mark changed entity page with `⚡ CHANGED: [description]`
- Log with `[CHANGE DETECTED]` tag

## Special Commands (natural language equivalents)

**book-summary** — Generate structured summary of the book wiki:
- Cast of characters table (name, role, faction, status)
- Timeline of key events (up to current_chapter)
- Major factions and their goals
- Key locations
- Major themes
- Unresolved mysteries / open questions
- Notable quotes
- Save to `outputs/book-summary-YYYY-MM-DD.md`

**competitive-brief "Name"** — Generate battlecard for a competitor:
- One-line positioning
- Pricing table (tiers, prices, limits)
- Top features list
- Recent moves (last 30 days)
- Known weaknesses (from reviews/Reddit)
- Job postings signal (roadmap inference)
- How we differ
- Save to `outputs/battlecard-name-YYYY-MM-DD.md`

**interview-prep "Company"** — Generate interview prep 1-pager:
- Company overview (stage, size, business model)
- Tech stack
- Engineering culture signals
- Recent news
- Interview process (from Glassdoor/Reddit)
- Known interview questions
- Compensation range (from levels.fyi/Reddit)
- Green flags / red flags
- Questions to ask them
- Save to `outputs/interview-prep-company-YYYY-MM-DD.md`

## Error Handling

- WebFetch/WebSearch fails → log error in LOG.md, skip that source, continue
- Unreadable raw file (binary, corrupted) → skip, log
- Wiki too large for context → read INDEX.md first, then only relevant pages
- Missing config.yaml field → use defaults from this file
