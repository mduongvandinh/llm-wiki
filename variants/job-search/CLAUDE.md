# Job Search Intel Wiki — Schema Extensions

> Extends the base CLAUDE.md schema with job search entity types.
> Merged automatically by: /llm-wiki setup job-search

## Additional Entity Types

### company (wiki/entities/)

```markdown
---
type: entity
category: company
website: "https://..."
stage: startup | scaleup | public | enterprise
size: "1-50" | "51-200" | "201-1000" | "1000+"
application_status: researching | applied | interviewing | offer | rejected | accepted
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
---

# Company Name

One-line: what they do, for whom.

## Company Overview
- **Founded:** YYYY
- **Stage / Funding:** [Series X / Public / Bootstrapped]
- **Size:** [Employee count estimate]
- **HQ / Remote policy:** [Location + remote details]
- **Business model:** [How they make money]

## Tech Stack
[Languages, frameworks, infrastructure — from job descriptions, engineering blog, StackShare]

## Engineering Culture
- [Culture signal 1 — with source]
- [Culture signal 2 — with source]
- [Culture signal 3 — with source]

## Interview Process
[What we know about their interview process — from Glassdoor, Reddit, personal contacts]

1. [Round 1]: [Format, duration, focus]
2. [Round 2]: [Format]
...

## Compensation
- **Base range:** $X–$Y (from levels.fyi, Reddit, Glassdoor)
- **Equity:** [RSUs, options — what we know]
- **Total comp:** [Estimate]

## Green Flags
- [Positive signal 1]
- [Positive signal 2]

## Red Flags / Watch Out For
- [Warning 1 — with source]
- [Warning 2]

## Recent News
- [YYYY-MM-DD]: [News item — funding, product, layoffs, etc.]

## My Application Status
- **Role:** [Position name]
- **Applied:** YYYY-MM-DD
- **Stage:** [Current interview stage]
- **Next step:** [What happens next]

## Liên kết
- [[role-company-name]]
- [[interviewer-name]]

## Nguồn
- [JD](../raw/articles/jd-company-role.md)
- [Glassdoor](../raw/articles/glassdoor-company.md)
```

### role (wiki/entities/)

```markdown
---
type: entity
category: role
company: company-name
status: open | applied | closed
applied_date: YYYY-MM-DD
created: YYYY-MM-DD
sources: []
---

# [Company] — [Role Title]

## Job Description Summary
[Key responsibilities in bullet points]

## Requirements
**Must have:**
- [Requirement 1]
- [Requirement 2]

**Nice to have:**
- [Requirement 1]

## Why I'm a Fit
- [My matching skill 1]
- [My matching skill 2]

## Gaps to Address
- [Area I need to prep for]

## Key Talking Points for Interview
- [Point 1 — connects my experience to their need]
- [Point 2]

## Liên kết
- [[company-name]]

## Nguồn
- [Job Description](../raw/articles/jd-company-role.md)
```

### interview_process (wiki/concepts/)

```markdown
---
type: concept
domain: interview_process
company: company-name
created: YYYY-MM-DD
sources: []
---

# Interview Process: [Company Name]

## Overview
[Total number of rounds, typical timeline, format]

## Rounds
1. **[Round Name]** (~Xh) — [Format: phone/video/onsite/take-home]
   - Focus: [Technical / Behavioral / System Design / etc.]
   - What to expect: [Details from research]
   - Common questions: [If known]

2. **[Round Name]** (~Xh) — [Format]
   - Focus: [Details]

## Known Questions / Topics
- [Frequently asked question 1]
- [Topic that often comes up]

## Tips from Community
- [Tip from Reddit/Glassdoor]
- [Tip from someone who went through it]

## Liên kết
- [[company-name]]

## Nguồn
- [Glassdoor reviews](../raw/articles/glassdoor-company.md)
- [Reddit thread](../raw/articles/reddit-company-interview.md)
```

## Ingest Rules (job-search)

1. **Company extraction:** Every JD creates/updates a company entity page
2. **Role extraction:** Each JD creates a role entity page linked to the company
3. **Culture signals:** Extract culture signals from reviews, blog posts, Reddit → add to company page
4. **Compensation data:** Extract salary ranges from levels.fyi, Reddit, Glassdoor → add to company page
5. **Interview patterns:** Extract interview format/questions from reviews/Reddit → create interview_process page
6. **Status tracking:** Company page `application_status` updates as user adds notes
7. **Cross-reference:** Role links to company; company links to all roles

## Sample File Naming

```
raw/articles/
├── jd-stripe-senior-swe-2026.md       # Job description
├── glassdoor-stripe-reviews.md         # Glassdoor review summary
├── reddit-stripe-interview-exp.md      # Reddit interview experience threads
├── stripe-engineering-blog.md          # Their engineering blog highlights
├── levels-fyi-stripe-comp.md           # Compensation data
└── jd-anthropic-product-manager.md    # Another company JD
```
