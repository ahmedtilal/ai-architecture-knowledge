---
title: "Chat to Create AI Agents Ready to Work for You"
type: source
source_type: article
date_ingested: 2026-04-14
original_path: "raw/articles/Chat to create AI agents ready to work for you..md"
tags: [ai-first-engineering, harness-engineering, ai-agents, engineering-management, devops]
---

## Summary

Peter Pang, CTO of [[CREAO]], describes how his 25-person agent platform company restructured its entire engineering process around AI. Rather than simply adding AI tools to existing workflows (AI-assisted), CREAO went AI-first — redesigning architecture, processes, and team roles so that AI is the primary builder and humans provide direction and judgment. They unified a fragmented multi-repo codebase into a single monorepo specifically so AI agents could see and reason about the full system.

Their stack includes a six-phase CI/CD pipeline via GitHub Actions, three parallel Claude-powered code review passes on every PR, feature flags via Statsig for same-day ship-and-kill cycles, and a self-healing feedback loop where Claude analyzes CloudWatch/Sentry errors each morning, auto-generates triaged Linear tickets, and auto-closes them after verified fixes. The result: over 99% of code written by AI, daily releases (previously every 4–6 weeks), and 5 core engineers doing the equivalent work of 100+.

Pang describes a new engineering org with two roles: the Architect (designs SOPs, testing infrastructure, and system boundaries — requires deep critical thinking) and the Operator (handles bug investigation, UI refinement, PR review). He notes junior engineers adapted faster than seniors, and that AI-native operations must extend beyond engineering into marketing, product, and growth to avoid creating new bottlenecks.

## Key Takeaways

- **AI-first ≠ AI-assisted.** Bolting AI onto existing processes yields 10–20% gains; redesigning processes around AI as the primary builder yields multiplicative gains.
- **Harness engineering** (a concept OpenAI named in February 2026) is the principle that engineering's primary job is enabling agents to do useful work, not writing code.
- **Monorepo for AI legibility.** A fragmented codebase is invisible to agents; a unified one is legible and actionable.
- **Self-healing feedback loops** — automated error detection, triage, fix verification, and ticket closure — are the centerpiece of their system.
- **Build the testing harness before scaling agents.** Fast AI without fast validation is fast-moving technical debt.
- **The Architect role** is the most critical and hardest to fill — requires the ability to criticize AI, find holes in its proposals, and design the constraints it operates within.
- **Junior engineers adapted faster** than senior engineers, who carried habits to unlearn. Adaptability matters more than accumulated skill.
- **One-person companies will become common** if one architect with agents can do the work of 100 people.
- **Model capability is the clock.** Pang attributes CREAO's shift to the last two months of model improvements (Opus 4.6 over 4.5).

## Relevance

This article is a detailed case study of [[AI-First Engineering]] in practice at a real company. It provides concrete evidence for the harness engineering thesis — that the engineer's role is shifting from code production to decision quality, system design, and AI oversight. The self-healing loop pattern connects to broader themes around [[AI Agents]] and autonomous systems. The human side (management collapse, team anxiety, role redefinition) adds nuance beyond the technical architecture.

## Links

- [[CREAO]]
- [[Peter Pang]]
- [[AI-First Engineering]]
- [[AI Agents]]
