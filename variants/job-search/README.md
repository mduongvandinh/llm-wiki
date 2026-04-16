# 💼 LLM Wiki — Job Search Intel Variant

> Research once. Know everything. Walk into every interview prepared.

Stop re-Googling the same company before every call. This wiki accumulates everything you learn about each target company — JDs, Glassdoor intel, Reddit threads, engineering blogs — and synthesizes it into a 1-pager when you need it most.

---

## Demo

Drop 2 JDs + company research → get:

```
Wiki: 16 pages
Companies (2):  Stripe, Anthropic
Roles (2):      Senior SWE (Stripe), Product Manager (Anthropic)
Processes (2):  Stripe Interview Process, Anthropic Interview Process
Concepts (4):   Distributed Systems Prep, PM Interview Framework,
                Negotiation Tactics, Remote Work Policies
```

**Before a Stripe interview:**
```
/llm-wiki interview-prep "Stripe"

→ # Interview Prep: Stripe
  [One-liner]: Payment infrastructure for the internet (~$50B, pre-IPO)

  Tech Stack: Ruby (core), Go (infra), MySQL, Redis, AWS

  Engineering Culture:
  - Low ego, high standards
  - Strong writing culture (design docs expected)
  - On-call is real — teams own what they ship

  Interview Process:
  1. Recruiter screen (30 min)
  2. Technical phone screen — 1-2 LeetCode medium (CoderPad)
  3. Take-home project (3-4 hrs, optional)
  4. Virtual on-site: coding + system design + behavioral + bar raiser

  Known Questions:
  - "Design an idempotent payments API"
  - "Tell me about a time you owned a project end-to-end"

  Compensation: L4 Senior ~$420-560K total (levels.fyi)

  Questions to Ask:
  - "What does on-call look like for this team?"
  - "How do you balance reliability vs feature work?"
```

---

## Who Is This For?

- **Engineers job searching** — track 10+ companies at once without losing your mind
- **Senior / Staff level candidates** — research-intensive searches where company fit matters as much as compensation
- **Career switchers** — building knowledge about a new industry while applying

---

## Setup (3 commands)

```bash
# 1. Clone the repo
git clone https://github.com/mduongvandinh/llm-wiki my-job-search
cd my-job-search

# 2. Set up job-search variant (includes Stripe + Anthropic sample data)
/llm-wiki setup job-search

# 3. Get your first interview prep
/llm-wiki interview-prep "Stripe"
```

---

## How to Use for Your Own Job Search

### Step 1 — Add target companies to config.yaml

```yaml
target_companies:
  - name: "Google"
    website: "https://google.com"
    role_applying: "Senior Software Engineer"
    status: "researching"
```

### Step 2 — Drop in your research

For each company, create files in `raw/articles/`:

**Job description:**
```markdown
---
title: "Google — Senior SWE L5"
company: "Google"
role: "Senior Software Engineer"
---
[Paste the JD here with your notes]
```

**Company research:**
```markdown
---
title: "Google — Company Research"
company: "Google"
---
[Glassdoor reviews, Reddit threads, engineering blog highlights, levels.fyi data]
```

### Step 3 — Ingest and build the wiki

```bash
/llm-wiki ingest
```

Wiki builds company pages, role pages, and interview process pages automatically.

### Step 4 — Let it discover more

```bash
/llm-wiki run
```

Auto-discovers recent news about your target companies, Reddit interview experiences, and community intel.

### Step 5 — Generate interview preps

```bash
# Night before the interview:
/llm-wiki interview-prep "Google"
# → Saved to outputs/interview-prep-google-2026-04-14.md
```

---

## Key Commands

| Command | Description |
|---------|-------------|
| `/llm-wiki setup job-search` | One-time setup with sample data |
| `/llm-wiki ingest` | Process new JDs and research files |
| `/llm-wiki run` | Full cycle: discover + ingest + lint |
| `/llm-wiki interview-prep "Company"` | Generate 1-pager interview prep |
| `/llm-wiki query "..."` | Ask anything about your target companies |
| `/llm-wiki digest` | Daily summary of new intel found |
| `/llm-wiki status` | See coverage for each target company |

---

## Example Queries That Work Great

```bash
/llm-wiki interview-prep "Stripe"
/llm-wiki query "How does Stripe interview process compare to Anthropic?"
/llm-wiki query "Which of my target companies has the best work-life balance?"
/llm-wiki query "What's the typical take-home assignment at these companies?"
/llm-wiki query "Compare compensation packages across all my targets"
/llm-wiki query "What technical topics should I focus on for infrastructure roles?"
```

---

## Application Tracker

Update each company's `application_status` in `config.yaml` or in the company entity page:

```
researching → applied → interviewing → offer → accepted/rejected
```

Run `/llm-wiki status` to see a dashboard of all companies and their stages.

---

## Sample Data Included

This variant ships with research for 2 companies:
- **Stripe** — Senior SWE JD + full company research (culture, interview process, comp data)
- **Anthropic** — Product Manager JD + notes

Replace with your actual targets to get started.

---

## Privacy Note

Your job search data is 100% local. Nothing is uploaded anywhere. The wiki lives on your machine, used only by Claude Code locally.

---

*Part of [llm-wiki](https://github.com/mduongvandinh/llm-wiki) — personal knowledge base that thinks for you.*
