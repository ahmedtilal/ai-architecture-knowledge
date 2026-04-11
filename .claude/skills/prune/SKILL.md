---
name: prune
description: Clean up the wiki — remove low-value pages, merge near-duplicates, fix dead links, and clean up stale index entries. All changes require user confirmation before executing. Use this skill whenever the user wants to prune, clean up, consolidate, or declutter the wiki, or says /prune.
---

# Prune the Wiki

Perform a destructive cleanup of the wiki by following the Prune workflow in `CLAUDE.md`. All changes require user confirmation.

## Workflow

1. Read `CLAUDE.md` and follow the **Prune** workflow exactly.

2. Scan the wiki and identify:
   - **Low-value pages:** Entity and theme pages in `wiki/entities/` and `wiki/themes/` with `source_count: 1` that appear to have been created from a single source and not enriched since. Read their content to confirm they don't contain independently valuable synthesis.
   - **Overlapping pages:** Pages with similar titles or heavily overlapping content that should be merged. Read both pages to confirm they cover the same ground.
   - **Dead wikilinks:** `[[wikilinks]]` across all wiki pages that point to pages that don't exist.
   - **Stale index entries:** Entries in `index.md` for pages that no longer exist.

3. Present all proposed changes grouped by type:

```
## Prune Proposal

### Pages to Delete
- `wiki/entities/some-entity.md` — only mentioned in one source, no independent value
- ...

### Pages to Merge
- Merge `wiki/themes/meditation.md` into `wiki/themes/mindfulness.md` — overlapping content
- ...

### Dead Wikilinks to Fix
- `wiki/sources/some-source.md` links to [[Nonexistent Page]]
- ...

### Stale Index Entries
- `index.md` references [[Removed Page]] which no longer exists
- ...

Approve all, or tell me which changes to skip.
```

4. Wait for user confirmation. The user may approve all, reject all, or selectively approve.

5. Execute only the approved changes:
   - For deletions: delete the file, remove from `index.md`, remove or update wikilinks in other pages that pointed to it
   - For merges: combine content into the surviving page, update its `source_count`, redirect all inbound wikilinks across the wiki to the surviving page, delete the merged-away page, update `index.md`
   - For dead wikilinks: remove the broken link or replace with plain text
   - For stale index entries: remove from `index.md`

6. Append a structured entry to `log.md`.

7. Commit all changes with message: `prune: <summary of changes>`

## Important

- **Never act without user confirmation.** Always present the full proposal first.
- Read `CLAUDE.md` for the prune workflow details, naming conventions, and log format.
- When merging pages, preserve all source attributions and contradictions from both pages.
