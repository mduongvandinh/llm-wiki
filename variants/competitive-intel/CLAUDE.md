# Competitive Intelligence Wiki — Schema Extensions

> Extends the base CLAUDE.md schema with competitive intelligence entity types.
> Merged automatically by: /llm-wiki setup competitive-intel

## Additional Entity Types

### competitor (wiki/entities/)

```markdown
---
type: entity
category: competitor
website: "https://..."
founded: YYYY
stage: seed | series-a | series-b | public
size: "1-10" | "11-50" | "51-200" | "201-500" | "500+"
last_updated: YYYY-MM-DD
change_detected: false
created: YYYY-MM-DD
sources: []
---

# Competitor Name

One-line positioning statement (their words or our summary).

## Company Overview
- **Founded:** YYYY
- **HQ:** [City, Country]
- **Size:** [Employee count estimate]
- **Funding:** [Total raised / Stage]
- **Target market:** [Who they sell to]

## Positioning
[How they describe themselves — from their website/marketing]

## Pricing
| Tier | Price | Key Features | Limits |
|------|-------|-------------|--------|
| Free | $0 | [features] | [limits] |
| Pro | $X/mo | [features] | [limits] |
| Enterprise | Custom | [features] | — |

## Top Features
- [Feature 1]: [Description]
- [Feature 2]: [Description]
- [Feature 3]: [Description]

## Known Weaknesses (from reviews & Reddit)
- [Weakness 1 — with source]
- [Weakness 2 — with source]

## Recent Moves
- [YYYY-MM-DD]: [Change/announcement]
- [YYYY-MM-DD]: [Change/announcement]

## Job Postings Signal
[What roles they're hiring for → implies roadmap direction]

## Liên kết
- [[market-position-name]]
- [[related-competitor]]

## Nguồn
- [Source 1](../raw/articles/competitor-name-pricing.md)
- [Source 2](../raw/articles/competitor-name-reviews.md)
```

### change_log (wiki/changes/)

```markdown
---
type: change_log
entity: competitor-name
detected: YYYY-MM-DD
field: pricing | features | positioning | jobs | funding
severity: major | minor
---

# Change Detected: [Competitor Name] — YYYY-MM-DD

## What Changed
[Clear description of the change]

## Previous State
[What it was before]

## New State
[What it is now]

## Significance
[Why this matters — competitive implication]

## Source
[URL or file where change was detected]
```

### market_position (wiki/concepts/)

```markdown
---
type: concept
domain: market_position
created: YYYY-MM-DD
sources: []
---

# [Market Segment / Position Name]

## Who Plays Here
- [[competitor-1]] — [brief positioning]
- [[competitor-2]] — [brief positioning]

## Key Differentiators in This Segment
- [Differentiator 1]
- [Differentiator 2]

## Our Position
[How we compare / where we fit]

## Liên kết
- [[competitor-1]]
- [[competitor-2]]
```

## Ingest Rules (competitive-intel)

1. **Source categorization:** Tag each raw file with `competitor: "name"` in frontmatter
2. **Pricing detection:** When ingesting pricing pages, always extract into structured table in competitor page
3. **Change detection:** On re-ingest of known source, compare with existing wiki page — flag differences
4. **Job postings:** Extract open roles → add to competitor's "Job Postings Signal" section
5. **Review aggregation:** From G2, Glassdoor, Reddit reviews → add to "Known Weaknesses" with source attribution
6. **Change log:** When change detected → create `wiki/changes/YYYY-MM-DD-competitor.md`

## Change Detection Trigger Words

When ingesting, flag as change if these patterns appear vs previous state:
- Pricing: "new pricing", "price change", "now $", "starting at $", "free plan"
- Features: "launching", "new feature", "now available", "introducing", "releasing"
- Strategy: "pivoting", "focusing on", "discontinuing", "sunsetting", "acquired"
- Funding: "raised", "series", "funding", "acquired", "IPO", "merger"

## Sample File Naming

```
raw/articles/
├── competitor-linear-pricing-2026.md
├── competitor-linear-reviews-g2.md
├── competitor-notion-changelog-q1.md
├── competitor-asana-job-postings.md
└── market-pm-tools-overview.md
```
