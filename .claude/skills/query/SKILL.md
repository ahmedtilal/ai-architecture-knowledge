---
name: query
description: Search and synthesize answers from the wiki — reads index, searches with qmd if available, and synthesizes answers with wikilinks. Use this skill whenever the user wants to query the wiki, search for information, asks "what does the wiki say about", or says /query.
---

# Query the Wiki

Search the wiki and synthesize an answer by following the Query workflow in `CLAUDE.md`.

## Arguments

This skill requires a question:
- `/query What are the key themes around sleep?`
- `/query Compare morning routine approaches`

## Workflow

1. Read `index.md` to identify relevant pages based on the question (primary navigation).

2. Check if `qmd` is available:
   ```bash
   which qmd
   ```
   - If available, run a supplementary search to catch pages the index scan might have missed:
     ```bash
     qmd search "relevant search terms" --dir wiki/
     ```
   - If not available, include this note in your response:
     > Tip: For better search coverage, consider installing [qmd](https://github.com/tobi/qmd) — a local markdown search engine with hybrid BM25/vector search.

3. Read the relevant wiki pages identified from both the index and qmd results.

4. Read `CLAUDE.md` and follow the **Query** workflow to synthesize an answer:
   - Use `[[wikilinks]]` to reference wiki pages
   - Attribute claims to their sources

5. If the answer is substantial or reusable (a comparison, a synthesis, a deep analysis), offer to file it as an analysis page:
   - "This answer seems worth keeping. Want me to file it as an analysis page in `wiki/analyses/`?"
   - If yes, create the analysis page following the template in CLAUDE.md, update `index.md`, append to `log.md`, and commit with message: `query: file analysis — <title>`

## Important

- `index.md` is always the primary navigation tool. `qmd` is supplementary.
- Read `CLAUDE.md` for the analysis page template and conventions.
