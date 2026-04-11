---
name: ingest
description: Process a raw source into the wiki — creates summary, updates entities, themes, personal pages, index, and log. Use this skill whenever the user wants to ingest a source, process a document, add something to the wiki, or says /ingest. Also triggers when the user drops a new file into raw/ and wants it processed.
---

# Ingest a Source

Process a raw source document into the wiki by following the Ingest workflow in `CLAUDE.md`.

## Arguments

This skill accepts an optional file path argument:
- `/ingest raw/articles/some-article.md` — process the specified source
- `/ingest` (no argument) — scan for unprocessed sources

## Workflow

### If a path argument is provided:

1. Verify the file exists in `raw/`.
2. Read `CLAUDE.md` and follow the **Ingest** workflow exactly.
3. Commit all changes with message: `ingest: <source title>`

### If no argument is provided:

1. Read `index.md` and collect all `original_path` values from source summaries in `wiki/sources/`.
2. Scan `raw/` recursively for all files (excluding `.gitkeep` and files in `raw/assets/`).
3. List any files in `raw/` that don't have a corresponding source summary.
4. If unprocessed files are found, present the list and ask the user which to process.
5. For each selected file, read `CLAUDE.md` and follow the **Ingest** workflow.
6. Commit all changes with message: `ingest: <number> sources processed`

## Important

- Read `CLAUDE.md` for page templates, frontmatter fields, naming conventions, and linking rules.
- Every ingest must update `index.md` and append to `log.md`.
- Use the judgment calls guidance in CLAUDE.md to decide which entities and themes deserve their own pages.
