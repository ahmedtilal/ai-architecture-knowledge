---
title: "Resolvers: The Routing Table for Intelligence"
type: source
source_type: article
date_ingested: 2026-04-15
original_path: "raw/articles/Resolvers The Routing Table for Intelligence.md"
tags: [resolvers, agent-architecture, harness-engineering, context-management]
---

## Summary

[[Garry Tan]] follows up on [[Thin Harness, Fat Skills]] with a deep dive into resolvers — the concept he says "got almost no attention" but "matters most." A resolver is a routing table for context: when task type X appears, load document Y first. Tan argues resolvers are "invisible when they work, and catastrophic when they don't."

The article opens with a confession: Tan's CLAUDE.md grew to 20,000 lines — every quirk, pattern, and lesson crammed into one file. The model's attention degraded. The fix was ~200 lines: a numbered decision tree with pointers to documents. The system immediately improved — faster, more accurate, fewer hallucinations. Not because the model got smarter, but because it stopped being drowned in noise.

A misfiling incident revealed the deeper problem. An agent filed a policy analysis article in `sources/` (for raw data) instead of `civic/` (for political analysis), because the ingest skill had hardcoded a default path rather than consulting the resolver. An audit of 13 brain-writing skills found only 3 referenced the resolver. The other 10 had hardcoded paths — each a potential misfiling. The fix was a shared filing rules document (`_brain-filing-rules.md`) and a mandate that every skill reads `RESOLVER.md` before creating any page.

Tan then describes the **invisible skill problem**: capabilities that exist but can't be reached because the resolver doesn't know about them. A signature-tracking system worked perfectly but never fired because the resolver had no trigger for "check my signatures." After a month of building, 40+ skills existed — 15% were unreachable ("dark capabilities"). He built `check-resolvable`, a meta-skill that walks the chain from AGENTS.md → skill file → code and finds dead links. It runs weekly as a linter.

The article introduces **context rot**: resolvers decay over time as new skills are added without updating the routing table, trigger descriptions drift from how users actually phrase requests, and the resolver becomes a historical artifact. Tan proposes the endgame: a reinforcement learning loop where the resolver rewrites itself based on observed task dispatch traffic — which skills fired, which didn't, which tasks had no match.

Resolvers are **fractal** — they exist at every layer: skill resolvers (AGENTS.md maps tasks to skills), filing resolvers (RESOLVER.md maps content to directories), and context resolvers (internal routing within each skill). Claude Code already implements this: every skill's description field is itself a resolver.

Tan reframes the entire pattern as **management**: skills are employees, the resolver is the org chart, filing rules are internal process, check-resolvable is audit/compliance, and trigger evals are performance reviews. "The problem isn't that models aren't smart enough. The problem is that we've been building organizations with no management layer."

He announces open-source tools embodying this architecture: **GBrain** (knowledge layer with built-in resolver pattern), **GStack** (fat skills in markdown, 72K+ GitHub stars), and **OpenClaw/Hermes Agent** (thin harness conductor).

## Key Takeaways

- Resolvers replace context cramming: 200 lines of routing replaced 20,000 lines of instructions, with immediate improvements in speed, accuracy, and fewer hallucinations
- The silent failure mode of agent systems isn't hallucination — it's slow drift where information goes to the wrong place and connections don't form ("junk drawer" knowledge base)
- Every skill that writes to a knowledge base must consult a shared resolver — hardcoded paths in individual skills lead to systematic misfiling
- **Dark capabilities** (15% in Tan's system) — skills that exist but are unreachable because the resolver doesn't know about them — are worse than missing capabilities because they create an illusion of capability
- `check-resolvable` as a weekly linter: walk the full chain from resolver to skill to code, find dead links
- **Trigger evals**: a test suite of sample inputs with expected skill outputs, catching false negatives (skill should fire but doesn't) and false positives (wrong skill fires)
- **Context rot**: resolvers decay within ~90 days as skills are added without updating routing; the endgame is self-healing resolvers that learn from task dispatch traffic
- Resolvers are fractal — they compose at every layer (skill routing, file routing, intra-skill routing)
- Agent systems are organizations: the resolver is the management layer (org chart + escalation logic + audit)

## Relevance

This is the most operationally detailed source in the wiki, providing concrete failure modes and maintenance patterns for agent systems at scale (40+ skills, 25,000 files, 200 inputs/day). It directly answers several open questions from [[AI Agents]] about failure modes and context management. The context rot concept and self-healing resolver proposal connect to the self-improving skills pattern from [[Thin Harness, Fat Skills]]. The "agents as organization" framing deepens [[AI-First Engineering]]'s restructured roles concept.

## Links

- [[Garry Tan]] — author, practitioner running this at 200 inputs/day
- [[AI Agents]] — resolver as the governance layer for agent systems
- [[AI-First Engineering]] — resolver patterns as operational infrastructure for AI-first teams
- [[Thin Harness, Fat Skills]] — predecessor article; this expands the resolver definition
