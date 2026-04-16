# Show HN Draft

**Title:** Show HN: I built a personal wiki that maintains itself using LLM agents

---

## Post body

I got tired of bookmarking articles I never read again.

The pattern that actually works for me: drop raw sources into a folder, let an AI read everything, and have it build a structured wiki — with cross-references, contradiction detection, and the ability to answer questions. Inspired by [Andrej Karpathy's LLM Wiki concept](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

I built it out and shared it a few months ago. People were interested, so I spent time extending it into practical variants for specific use cases. Sharing the full version now.

**How it works:**

```
raw/          → You drop articles, notes, PDFs here
wiki/         → AI reads and builds structured wiki pages
outputs/      → Query results, reports, summaries
config.yaml   → Your topics, feeds, schedule
```

Four commands do everything:
- `ingest` — reads new files, extracts entities/concepts, builds cross-referenced pages
- `query` — answers questions from the wiki (not from the AI's training data)
- `discover` — finds new sources from the web based on your topics and knowledge gaps
- `lint` — health check: find contradictions, orphan pages, stale info

**Why not just use RAG?**

RAG re-retrieves fresh every query. This compiles knowledge once and maintains it. The difference matters when you want things like: "has anything changed since last week?" or "what do I know about X across all my sources?" RAG doesn't build cross-references or detect contradictions. The wiki does.

**Three variants I built:**

1. **book-companion** — Drop your reading notes in as you go. The AI extracts characters, locations, factions, timelines, themes. Spoiler protection (skips chapters ahead of where you are). `/book-summary` generates a structured cast table + timeline on demand. I included a pre-built demo wiki from Dune (Chapters 1-3) so you can see the output without running anything.

2. **competitive-intel** — Track competitor pricing, features, job postings. Change detection flags when something updates. `/competitive-brief "Linear"` generates a battlecard. Includes sample data from the PM tools market.

3. **job-search** — Accumulate company research, JDs, Glassdoor threads, Reddit discussion. `/interview-prep "Stripe"` synthesizes everything into a prep 1-pager. Includes sample data for Stripe and Anthropic.

**Works with multiple AI tools:**

- Claude Code: `/llm-wiki run` (slash command, supports `/loop 1h` for auto-updating)
- Cursor / Windsurf: reads `AGENTS.md` automatically
- Gemini CLI: `gemini "Read GEMINI.md and run full cycle"`
- GitHub Copilot: reads `.github/copilot-instructions.md` automatically
- Codex CLI, Aider, Continue.dev: all supported

**Cost:** A typical run (discover 5 sources + ingest + lint) costs $0.10–0.40 with Claude Sonnet. Running every 2 hours = ~$50/month. Or run manually when you have new stuff.

**GitHub:** [github.com/mduongvandinh/llm-wiki](https://github.com/mduongvandinh/llm-wiki)

Happy to answer questions about specific use cases or how the ingest pipeline works.

---

## Anticipated HN Questions & Answers

**Q: Why not just use NotebookLM / Perplexity / ChatGPT with files?**

Those answer questions from your docs. They don't build and maintain a cross-referenced knowledge base over time. They don't detect contradictions between sources added 3 months apart. They don't have a "what changed since my last run?" concept. The wiki accumulates and compounds.

**Q: Doesn't the AI hallucinate things into the wiki?**

Ingest and query are constrained: write only what's in the raw sources, cite everything. The wiki page format enforces this — every claim has a `[Source: filename]` citation. If something can't be sourced, the AI is instructed to say so and suggest what to discover.

**Q: What happens when the wiki gets large?**

It reads `wiki/INDEX.md` first — a one-page catalog — then loads only the relevant pages. Even 500+ page wikis query efficiently because you never load everything at once. The INDEX approach is intentional.

**Q: Isn't this just an expensive way to do Obsidian?**

Obsidian is for manual note-taking. This is for automatic synthesis across sources you'd never have time to manually connect. Different inputs, different workflows.

**Q: Why markdown files instead of a proper database?**

So it works with every AI tool, every editor, every sync system. Obsidian reads it. GitHub renders it. You can grep it. You can version-control it. The simplicity is the point.

**Q: Can it handle PDFs / videos / audio?**

Currently: markdown and text files natively. PDFs need a preprocessing step (extract text first). Video/audio transcripts work fine once you have the text. The raw/ folder just needs text files — the format doesn't matter.

**Q: What model do you use?**

Claude Sonnet for main work (good balance of quality and cost). The discover step can use a cheaper model. Gemini 1.5 Pro and GPT-4o also work — just slower on long ingest sessions in my testing.
