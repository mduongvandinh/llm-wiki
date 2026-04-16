---
title: "Stripe — Company Research"
company: "Stripe"
category: company_research
sources: ["glassdoor", "reddit", "engineering-blog", "levels-fyi"]
collected: "2026-04-14"
---

# Stripe — Company Research

## Company Overview
- **Founded:** 2010 by Patrick and John Collison
- **HQ:** San Francisco + Dublin (dual HQ)
- **Size:** ~10,000 employees (2026)
- **Stage:** Pre-IPO (last valued at ~$50B; IPO expected but not confirmed)
- **Business:** Payment infrastructure — online payment processing, billing, treasury, fraud detection
- **Revenue model:** Transaction fees (~2.9% + $0.30 per transaction)
- **Customers:** Amazon, Shopify, Lyft, Salesforce, thousands of startups

## Tech Stack (from job descriptions + engineering blog)
- **Languages:** Ruby (core), Go (infrastructure), JavaScript/TypeScript (frontend)
- **Infrastructure:** AWS, proprietary internal tools
- **Databases:** MySQL, Redis, custom distributed storage
- **Notable:** Stripe has significant internal tooling built in-house; strong "build vs buy" culture
- **ML:** Heavy investment in ML for fraud detection (Radar)

## Engineering Culture

**Strong signals (positive):**
- "Work on hard problems" — genuinely complex systems at scale
- Strong writing culture: design docs and RFCs expected for meaningful changes
- High code review bar: PRs are thoroughly reviewed, comments are substantive
- On-call is taken seriously: teams are responsible for what they ship
- Internal mobility is common: engineers move between teams without stigma

**Culture notes (from Glassdoor/Reddit):**
- Stripe employees often describe culture as "low ego, high standards"
- Meetings are uncommon; async communication (Slack, docs) preferred
- "Stripe is run like a startup that survived" — scrappy in good ways, but systems are getting more corporate as they scale
- Engineering managers came from IC tracks; few pure-management-track MBAs

## Interview Process (from Glassdoor + Reddit r/cscareerquestions)

### Typical Timeline: 3-5 weeks total

1. **Recruiter screen** (30 min, phone)
   - Background, motivation, basic logistics
   - Compensation expectations discussed early

2. **Technical phone screen** (45-60 min, CoderPad)
   - 1-2 LeetCode-style coding problems (medium difficulty)
   - Focus: correct solution + clean code + thinking out loud
   - Common topics: arrays, strings, hash maps, basic graph/tree

3. **Take-home or Async Project** (3-4 hours)
   - Small coding project or system design problem to complete at home
   - Emphasis on code quality, documentation, testing
   - Not always included — depends on role/level

4. **Virtual On-site** (4-5 hours, split over 1-2 days)
   - **Coding round** (45 min) — 1-2 problems, higher difficulty
   - **System design** (45 min) — Design a distributed system (payments context often used)
   - **Behavioral / Leadership** (45 min) — STAR format; Stripe values ownership
   - **Bar raiser round** (45 min) — Cross-functional evaluator; harder to pass

### Known Interview Topics
- System design: distributed systems, idempotency, consistency models
- Coding: medium-hard algorithms; clean, tested, documented code expected
- Behavioral: "Tell me about a time you owned a project end-to-end"
- Stripe-specific values: User focus, pragmatic optimism, move with urgency

## Compensation (from levels.fyi, Reddit — approximate 2026)
| Level | Base | Equity (4yr) | Bonus | Total |
|-------|------|-------------|-------|-------|
| L3 (SWE) | $185-210K | $200-350K | 10% | ~$290-380K |
| L4 (Senior SWE) | $220-260K | $350-600K | 12% | ~$420-560K |
| L5 (Staff SWE) | $270-320K | $600K-1.2M | 15% | ~$580-800K |

*Note: Equity is pre-IPO; actual value depends on IPO outcome*

## Recent News
- 2026-Q1: Launched Stripe Stablecoin API — entering crypto payments
- 2025-Q4: Acquired Bridge (stablecoin infrastructure company) for $1.1B
- 2025-Q3: Expanded Stripe Tax to 50+ countries
- 2025-Q2: Stripe Revenue Recognition GA — growing finance product suite
- 2025-Q1: Announced new data center infrastructure for 99.9999% uptime target

## Green Flags
- Pre-IPO equity with strong valuation upside
- Genuinely hard technical problems (not CRUD apps)
- Strong alumni network (ex-Stripe engineers are highly valued)
- Engineering-led culture; ICs have real influence
- Good work-life balance by SV standards (no "crunch culture")

## Red Flags / Watch Out For
- IPO uncertainty: equity timeline unclear; been "IPO-ing soon" for several years
- Growing pains: more process as company scales; some complain it's "less startup-y"
- Compensation vs. FAANG: base salaries slightly below Google/Meta/Apple
- On-call can be intense: payments never stop; incidents at 3am are real
- Hiring bar is high: interview process has high rejection rate (< 10% offer rate)

## Questions to Ask Them
- "What does a typical on-call rotation look like for this team?"
- "How do you balance reliability work vs feature development?"
- "What's the biggest technical challenge the Payments Infrastructure team is working on?"
- "How does the IPO timeline affect equity compensation conversations?"
- "What does career progression from Senior to Staff look like here?"
