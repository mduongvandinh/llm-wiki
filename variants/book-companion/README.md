# 📚 LLM Wiki — Book Companion Variant

> Read books. Drop notes. Get a personal wiki that thinks with you.

As you read each chapter, drop your notes into a folder. The wiki automatically builds character pages, tracks relationships, maps locations, and surfaces connections you might have missed — like having a fan wiki that grows with you as you read.

---

## Demo

**Before:** 3 chapter note files in a folder.

**After:**

```
Wiki: 19 pages
Characters (8): Paul Atreides, Lady Jessica, Duke Leto, Thufir Hawat,
                Gurney Halleck, Reverend Mother Mohiam, Stilgar, Dr. Kynes
Factions (5):   House Atreides, House Harkonnen, Bene Gesserit,
                The Fremen, Spacing Guild
Locations (3):  Caladan, Arrakis, Arrakeen
Concepts (6):   Melange, Gom Jabbar, Kwisatz Haderach,
                Stillsuit, Missionaria Protectiva, The Voice
```

**Browse the pre-built output:** [`demo/book-companion/`](../../demo/book-companion/) — fully generated wiki you can read right now without running anything.

**Query example:**
```
/llm-wiki query "What is the relationship between Jessica and the Bene Gesserit?"

→ Lady Jessica is a fully trained Bene Gesserit who defied the sisterhood's
  orders to bear only daughters — she bore Paul instead, out of love for
  Duke Leto. The Reverend Mother is aware of this defiance and considers Paul
  both a potential Kwisatz Haderach and a danger, born "one generation too early."
  Jessica now recognizes the Missionaria Protectiva prophecies on Arrakis and
  realizes she and Paul fit them perfectly... [Sources: ch01, ch02, ch03]
```

---

## Who Is This For?

- **Complex fiction readers** — Dune, ASOIAF, Mistborn, Wheel of Time, Malazan... any book with many characters and deep lore
- **Non-fiction learners** — Business books, history, science — build a structured knowledge base as you read
- **Book clubs** — Share your wiki with the group; everyone can query it before discussions

---

## Setup (3 commands)

```bash
# 1. Clone the repo
git clone https://github.com/mduongvandinh/llm-wiki my-book-wiki
cd my-book-wiki

# 2. Set up the book-companion variant (includes Dune sample data)
/llm-wiki setup book-companion

# 3. Start asking questions
/llm-wiki query "Who are the main factions in Dune?"
```

That's it. The setup command copies the config, creates the folder structure, ingests the sample Dune data, and builds the initial wiki automatically.

---

## How to Use for Your Own Book

### Step 1 — Create your chapter notes

For each chapter you read, create a file in `raw/articles/`:

```markdown
---
title: "My Book — Chapter 5 Notes"
chapter: 5
book: "My Book Title"
---

# Chapter 5: The Turning Point

## Summary
[What happened]

## Characters
**Character Name**
- [What we learned about them]
- [What they did]

## Key Quotes
> "Memorable quote" — Character Name

## Questions / Mysteries
- [Something I don't understand yet]
```

### Step 2 — Ingest your notes

```bash
/llm-wiki ingest
```

The wiki builds automatically. Characters, locations, factions — all extracted and cross-linked.

### Step 3 — Ask questions

```bash
/llm-wiki query "What do we know about [character]?"
/llm-wiki query "Timeline of events involving [faction]"
/llm-wiki query "What are the unresolved mysteries so far?"
```

### Step 4 — Update your chapter progress

As you advance in the book, update `config.yaml`:

```yaml
current_chapter: 10   # Was 3, now at chapter 10
```

This unlocks ingesting further chapters you've already noted down, while maintaining spoiler protection.

### Step 5 — Get a full summary

```bash
/llm-wiki book-summary
```

Generates: cast list, key events timeline, major themes, open questions, notable quotes.

---

## Spoiler Protection

Files with `chapter: N` in their frontmatter will only be ingested when `config.yaml → current_chapter >= N`.

This means you can write notes for multiple future chapters in advance, drop them all in `raw/articles/`, and they'll be ingested gradually as you update `current_chapter`.

---

## Key Commands

| Command | Description |
|---------|-------------|
| `/llm-wiki setup book-companion` | One-time setup with Dune sample data |
| `/llm-wiki ingest` | Process new chapter notes |
| `/llm-wiki query "..."` | Ask anything about the book |
| `/llm-wiki book-summary` | Structured summary: cast, timeline, themes, mysteries |
| `/llm-wiki lint` | Find orphan pages, missing cross-links |
| `/llm-wiki status` | See wiki health and page count |

---

## Example Queries That Work Great

```bash
/llm-wiki query "Who is Paul Atreides and what makes him special?"
/llm-wiki query "What is the political situation between the Emperor and House Atreides?"
/llm-wiki query "Explain the importance of spice/melange"
/llm-wiki query "List all characters from House Atreides"
/llm-wiki query "What are Paul's dreams about?"
/llm-wiki query "Compare House Atreides and House Harkonnen"
/llm-wiki book-summary
```

---

## Customization

**Change the book:** Update `wiki.name` in `config.yaml` to your book title. Delete sample data from `raw/articles/`. Start adding your own chapter notes.

**Change the language:** Update `wiki.language` in `config.yaml`. Wiki content will be generated in your preferred language.

**Multiple books:** Create separate wiki folders for each book. Each gets its own `config.yaml` with `book_mode: true`.

---

## Sample Data Included

This variant ships with 3 chapters of **Dune** (Frank Herbert) notes:
- Chapter 1: The Gom Jabbar test — Paul meets the Reverend Mother
- Chapter 2: House Atreides prepares to leave Caladan
- Chapter 3: Arrival on Arrakis — meeting the Fremen

These are reading notes / summaries written in the reader's own words, suitable for ingestion and demonstration.

---

*Part of [llm-wiki](https://github.com/mduongvandinh/llm-wiki) — personal knowledge base that thinks for you.*
