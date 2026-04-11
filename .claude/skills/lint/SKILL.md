---
name: lint
description: Health-check the wiki — find contradictions, orphan pages, missing pages, stale claims, and linking gaps. Use this skill whenever the user wants to lint the wiki, run a health check, do maintenance, audit the wiki, or says /lint.
---

# Lint the Wiki

Run a read-only health check on the wiki by following the Lint workflow in `CLAUDE.md`.

## Workflow

1. Read `CLAUDE.md` and follow the **Lint** checklist exactly.
2. Scan all pages in `wiki/` for the issues listed in the checklist.
3. Auto-fix trivial issues only:
   - Add missing cross-references (e.g., a source summary mentions an entity but doesn't wikilink to it)
   - Fix broken wikilinks to renamed pages
4. Report all findings in a structured summary, grouped by issue type:

```
## Lint Report

### Contradictions
- ...

### Orphan Pages (no inbound links)
- ...

### Missing Pages (referenced but don't exist)
- ...

### Stale Claims
- ...

### Linking Gaps (source summaries with no theme/entity links)
- ...

### Suggestions
- New questions to investigate: ...
- Sources to look for: ...
```

5. If any auto-fixes were applied, commit with message: `lint: auto-fix <description>`

## Important

- This is primarily read-only. Only fix trivial linking issues automatically.
- Substantive changes (creating pages, resolving contradictions, deleting content) are reported as suggestions — do not act on them without user instruction.
- Read `CLAUDE.md` for the full lint checklist and conventions.
