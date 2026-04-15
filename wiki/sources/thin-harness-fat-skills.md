---
title: "Thin Harness, Fat Skills"
type: source
source_type: article
date_ingested: 2026-04-15
original_path: "raw/articles/Thin Harness, Fat Skills.md"
tags: [harness-engineering, agent-architecture, skill-files, ai-first-engineering]
---

## Summary

[[Garry Tan]], president of Y Combinator, argues that the difference between 2x and 100x productivity with AI agents isn't model intelligence — it's architecture. He cites Steve Yegge's claim that agent users are "10x to 100x as productive as engineers using Cursor and chat today, and roughly 1000x as productive as Googlers were back in 2005." The key insight: the 2x and 100x people use the same models. The difference is the harness wrapping the model.

Tan proposes a specific architectural pattern called **thin harness, fat skills**, built on five definitions:

1. **Skill files** — reusable markdown documents that teach the model a process (not a task). Skills work like method calls: same procedure, different arguments, radically different capabilities. "Markdown is a more perfect encapsulation of capability than rigid source code."
2. **The harness** — the thin program running the LLM: loop, file I/O, context management, safety. The anti-pattern is a fat harness with thin skills (40+ tool definitions, slow MCP round-trips, REST wrappers eating context).
3. **Resolvers** — routing tables for context. When task type X appears, load document Y first. Claude Code's built-in resolver matches user intent to skill descriptions automatically. Tan's CLAUDE.md shrank from 20,000 lines to ~200 lines of pointers after he adopted resolvers.
4. **Latent vs. deterministic** — every step must be classified correctly. Latent space handles judgment, synthesis, pattern recognition. Deterministic handles exact computation. Confusing the two is "the most common mistake in agent design." An LLM can seat 8 people accounting for social dynamics; ask it to seat 800 and it hallucinates.
5. **Diarization** — the model reads everything about a subject and writes a structured profile. "No SQL query produces this. No RAG pipeline produces this." The model must hold contradictions in mind and synthesize structured intelligence.

These compose into a three-layer architecture: fat skills on top (90% of value), thin CLI harness in the middle (~200 lines), deterministic application on the bottom.

Tan demonstrates this with a real YC system for Startup School (July 2026, 6,000 founders): `/enrich-founder` diarizes founder profiles catching gaps between what founders say and what they're actually building; `/match-breakout`, `/match-lunch`, and `/match-live` use the same matching skill with different parameters for different contexts; `/improve` reads NPS surveys after events and rewrites skill files with new rules, creating a self-improving loop (12% "OK" ratings dropped to 4%).

## Key Takeaways

- The productivity multiplier comes from architecture (harness design), not model intelligence — same model, 50x different outcomes
- **Thin harness, fat skills**: push intelligence up into markdown skill files, push execution down into deterministic tooling, keep the harness minimal
- Skill files are method calls: same process, different parameters, radically different capabilities
- The anti-pattern is a fat harness with thin skills — too many tools, slow MCP, context bloat
- Resolvers route context on demand instead of front-loading everything into the prompt (CLAUDE.md: 20,000 lines → 200 lines of pointers)
- Latent vs. deterministic is the most critical classification — putting the wrong work on the wrong side causes failures
- Diarization (structured synthesis from reading all sources) is the capability that makes AI useful for real knowledge work — not RAG, not embedding search
- Skills are permanent upgrades: they never degrade, run at 3 AM, and automatically improve when models improve
- Self-improving loops: skills that read feedback and rewrite themselves based on outcomes

## Relevance

This is the most architecturally specific source in the wiki, providing a concrete design pattern (thin harness, fat skills) that operationalizes the [[AI-First Engineering]] and harness engineering concepts from other sources. It directly extends [[OpenAI]]'s harness engineering concept with a prescriptive architecture. The self-improving skill loop is a novel contribution — no other source describes skills that rewrite themselves based on measured outcomes. The latent vs. deterministic distinction provides a useful design heuristic for all agent systems in [[AI Agents]].

## Links

- [[Garry Tan]] — author, YC president, practitioner of this architecture
- [[AI-First Engineering]] — thin harness/fat skills is a specific architectural pattern within AI-first engineering
- [[AI Agents]] — skill files, resolvers, and the latent/deterministic distinction as agent design principles
