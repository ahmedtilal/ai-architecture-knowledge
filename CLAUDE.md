# Personal LLM Wiki — Schema

This file governs how you (the LLM) operate this wiki. Follow these instructions precisely when ingesting sources, answering queries, or maintaining the wiki.

## Directory Structure

```
raw/                    # Immutable source documents — READ ONLY, never modify
  articles/             # Web clips, essays
  journals/             # User's own writing — daily journals, reflections
  podcasts/             # Transcripts, show notes
  videos/               # Transcripts, notes
  books/                # Highlights, chapter notes
  assets/               # Downloaded images referenced by sources

wiki/                   # LLM-generated and maintained pages — you own this
  sources/              # One summary page per ingested source
  entities/             # People, books, tools, organizations
  themes/               # Cross-cutting topics (Sleep, Productivity, etc.)
  personal/             # Pages about the user — goals, beliefs, open questions
  analyses/             # Filed query outputs — comparisons, deep dives

index.md                # Content catalog — you maintain this
log.md                  # Chronological operation log — you append to this
```

### Boundary Rules

- **Never** modify anything in `raw/`. It is the immutable source of truth.
- You own everything in `wiki/`, `index.md`, and `log.md`.
- This file (`CLAUDE.md`) is co-evolved by the user and you together.

## Page Templates

### Source Summary (`wiki/sources/`)

Filename: kebab-case of the source title (e.g., `why-we-sleep-chapter-3.md`).

```yaml
---
title: "Source Title"
type: source
source_type: article | journal | podcast | video | book
date_ingested: YYYY-MM-DD
original_path: "raw/category/filename.md"
tags: [tag1, tag2]
---
```

Content structure:
1. **Summary** — 2-4 paragraph summary of key points
2. **Key Takeaways** — bulleted list of the most important claims or insights
3. **Relevance** — how this connects to existing themes, entities, or personal goals
4. **Links** — must include `[[wikilinks]]` to at least one theme or entity page

### Entity Page (`wiki/entities/`)

Filename: kebab-case of the entity name (e.g., `andrew-huberman.md`).

```yaml
---
title: "Entity Name"
type: entity
entity_type: person | book | tool | organization
source_count: 1
tags: [tag1, tag2]
---
```

Content structure:
1. **Overview** — who/what this is, in 1-2 sentences
2. **Key Ideas** — ideas, contributions, or significance associated with this entity
3. **Mentions** — list of source summaries that reference this entity, with brief context for each
4. **Related** — links to related entities and themes

When updating an existing entity page with a new source, increment `source_count`, add to Mentions, and update Key Ideas if the new source adds substantive information.

### Theme Page (`wiki/themes/`)

Filename: kebab-case of the theme (e.g., `sleep.md`).

```yaml
---
title: "Theme Name"
type: theme
source_count: 1
tags: [tag1, tag2]
---
```

Content structure:
1. **Overview** — what this theme covers, in 1-2 sentences
2. **Synthesis** — the current understanding across all sources. Write this as a coherent narrative, not a list of per-source summaries. Attribute claims to sources using `[[wikilinks]]`.
3. **Contradictions** — any conflicting claims between sources (see Contradiction Handling below)
4. **Open Questions** — what remains unclear or worth investigating further
5. **Sources** — list of all source summaries touching this theme

When updating with a new source, increment `source_count`, integrate new information into Synthesis, and check for contradictions.

### Personal Page (`wiki/personal/`)

Filename: kebab-case of the topic (e.g., `current-goals.md`).

```yaml
---
title: "Page Title"
type: personal
tags: [tag1, tag2]
---
```

Content structure: flexible — these pages are about the user's goals, habits, beliefs, and open questions. Update them when sources or conversations surface relevant insights. Always note what source triggered the update.

### Analysis Page (`wiki/analyses/`)

Filename: kebab-case of the analysis title (e.g., `morning-routines-comparison.md`).

```yaml
---
title: "Analysis Title"
type: analysis
date_created: YYYY-MM-DD
tags: [tag1, tag2]
---
```

Content structure: flexible — comparisons, deep dives, syntheses. Must include `[[wikilinks]]` to referenced wiki pages.

## Operations

### Ingest

Triggered when the user says "ingest this", "process this", or points to a source in `raw/`.

**Workflow:**

1. Read the source document fully.
2. Create a source summary page in `wiki/sources/`.
3. Identify entities mentioned in the source:
   - If an entity page exists in `wiki/entities/`, update it (increment `source_count`, add to Mentions, update Key Ideas if warranted).
   - If no entity page exists and the entity is significant (not just a passing mention), create one.
4. Identify themes touched by the source:
   - If a theme page exists in `wiki/themes/`, update it (increment `source_count`, integrate into Synthesis, check for contradictions).
   - If no theme page exists and the theme is substantive, create one.
5. Check if anything in the source relates to personal pages in `wiki/personal/`. Update if relevant.
6. Update `index.md` — add entries for all new pages, update summaries for modified pages.
7. Append a structured entry to `log.md` (see Log Format below).

**Judgment calls:** Not every person, book, or concept mentioned in a source deserves its own page. Create entity/theme pages for things that are substantive and likely to appear across multiple sources. Passing mentions can be noted in the source summary without their own page.

### Query

Triggered when the user asks a question about the wiki's content.

**Workflow:**

1. Read `index.md` to identify relevant pages.
2. Read those pages.
3. Synthesize an answer using `[[wikilinks]]` to referenced pages.
4. If the answer is substantial or reusable (a comparison, a synthesis, a deep analysis), offer to file it as an analysis page in `wiki/analyses/`.

### Lint

Triggered when the user says "lint the wiki", "health check", or "maintenance".

**Checklist:**

1. **Contradictions** — scan theme pages for claims that conflict across sources.
2. **Orphan pages** — find wiki pages with no inbound `[[wikilinks]]` from other pages.
3. **Missing pages** — find concepts or entities mentioned frequently in `[[wikilinks]]` but lacking their own page.
4. **Stale claims** — flag claims on theme pages that newer sources have superseded.
5. **Linking gaps** — find source summaries that don't link to any theme or entity.
6. **Suggestions** — suggest new questions to investigate or new sources to look for based on gaps in the wiki.

Report findings. Fix what can be fixed automatically (e.g., add missing cross-references). Flag what needs user input (e.g., resolving contradictions, deciding whether to create a page for a borderline entity).

### Prune

Triggered when the user says "prune the wiki", "clean up", or "consolidate pages".

**Workflow:**

1. **Identify low-value pages:** Find entity and theme pages with `source_count: 1` that haven't been updated since creation. These are candidates for removal — their content can be folded back into the source summary that spawned them.
2. **Find overlapping pages:** Identify near-duplicate or highly overlapping pages (e.g., "Meditation" and "Mindfulness") that should be merged into one.
3. **Clean up dead references:** Find wikilinks pointing to pages that don't exist, and stale entries in `index.md` for removed pages.
4. **Present all proposed changes to the user for confirmation.** Group by type:
   - Pages to delete (with reason)
   - Pages to merge (showing what gets combined into what)
   - Dead wikilinks to fix
   - Index entries to clean up
5. After user confirms (all or selectively), execute approved changes:
   - Delete approved pages
   - Merge approved page pairs — combine content into the surviving page, update all inbound wikilinks across the wiki to point to the surviving page
   - Remove or fix dead wikilinks
   - Update `index.md`
   - Append a structured entry to `log.md`

**Key rule:** Prune is destructive. Never act without user confirmation. Present proposed changes as a list the user can approve or reject individually.

## Conventions

### Naming

- **Filenames:** kebab-case (e.g., `andrew-huberman.md`, `morning-routines-comparison.md`)
- **Page titles:** title case in frontmatter `title` field
- **Tags:** lowercase, no spaces (e.g., `sleep`, `self-improvement`, `cognitive-science`)

### Linking

- Always use Obsidian-style `[[Page Title]]` wikilinks.
- Every source summary must link to at least one theme or entity page.
- Every entity and theme page must link back to its source summaries.

### Contradiction Handling

When a new source conflicts with an existing claim on a theme page, do not silently overwrite. Flag both views with source attribution:

```markdown
**Contested:** [[Source A]] claims X, while [[Source B]] claims Y.
```

Place contested items in the **Contradictions** section of the theme page.

### Log Format

Each entry in `log.md`:

```markdown
## [YYYY-MM-DD] operation | Description

- Source: `raw/category/filename.md`
- Pages created: [[Page One]], [[Page Two]]
- Pages updated: [[Page Three]], [[Page Four]]
```

Operation types: `init`, `ingest`, `query`, `lint`, `prune`, `update`.

### Index Format

`index.md` organizes pages by type. Each entry is a wikilink followed by a one-line summary:

```markdown
## Sources
- [[Source Title]] — one-line summary of key takeaway

## Entities
- [[Entity Name]] — one-line description

## Themes
- [[Theme Name]] — one-line description of what this theme covers

## Personal
- [[Page Title]] — one-line description

## Analyses
- [[Analysis Title]] — one-line description of what this analyzes
```

Keep entries within each section sorted alphabetically.
