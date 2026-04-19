---
title: "AI-First Engineering"
type: theme
source_count: 7
tags: [ai-first-engineering, harness-engineering, engineering-management, software-development, 20x-companies, platform-engineering]
---

## Overview

AI-first engineering is the practice of redesigning engineering processes, architecture, and organization around the assumption that AI is the primary builder — as opposed to AI-assisted approaches that bolt AI tools onto existing workflows.

## Synthesis

[[Peter Pang]] of [[CREAO]] draws a sharp distinction between AI-assisted and AI-first. AI-assisted means adding Copilot to an IDE or using ChatGPT for specs while keeping existing sprint cycles, Jira boards, and QA sign-offs intact — yielding 10–20% efficiency gains. AI-first means restructuring everything so AI does the building and engineers provide direction and judgment. The difference is "multiplicative" ([[Chat to Create AI Agents Ready to Work for You]]).

A key concept is **harness engineering**, named by OpenAI in February 2026: the primary job of an engineering team is no longer writing code but enabling agents to do useful work. When something fails, the fix is never "try harder" — it's identifying what capability is missing and making it legible and enforceable for the agent.

In practice, this requires: (1) architectural changes for AI legibility (e.g., monorepo so agents can see the full system), (2) automated testing infrastructure that moves at the same speed as implementation, (3) self-healing feedback loops for error detection and resolution, and (4) restructured roles — Architects who design constraints and evaluate AI output, and Operators who handle investigation and verification.

The human implications are significant: management time collapses, team anxiety rises during transition, senior engineers struggle more than juniors (habits to unlearn vs. empowerment from new tools), and the ability to criticize AI becomes more valuable than the ability to write code.

Pang also argues that AI-first must extend beyond engineering into every function (marketing, product, growth) — if one function operates at agent speed and another at human speed, the human-speed function constrains everything.

[[Index]]'s story ([[How Index Built an AI-First Data Analytics Platform with Mastra]]) illustrates a different path: rather than building AI-first from scratch, they're **layering AI onto a mature product** with 1,000+ existing customers. Their approach is to add a conversational agent layer on top of their existing drag-and-drop BI platform, powered by [[Mastra]]. This represents a product-level AI-first transition — the fundamental interface shifts from point-and-click to natural language, while the underlying data access layer remains the same.

Framework choice is a practical dimension of AI-first engineering: Index's switch from LangChain to Mastra for TypeScript-native type safety and opinionated design patterns reflects the principle that the harness around agents matters. An opinionated framework that reduces cognitive load aligns with the harness engineering concept — the developer's job is to direct agents, not fight infrastructure.

[[OpenAI]]'s guide ([[Building an AI-Native Engineering Team – Codex]]) operationalizes AI-first engineering across the full SDLC with a structured **Delegate / Review / Own** framework. This formalizes what CREAO discovered organically: the engineer's role shifts from implementer to reviewer, editor, and source of direction. The agent is the first-pass builder; the human owns architecture, product intent, and quality judgment.

OpenAI identifies concrete infrastructure for AI-first teams: **MCP servers** connect agents to external systems (issue trackers, design tools, logging, deployment), and **AGENTS.md** files provide persistent configuration for agent behavior and guardrails. This is the practical tooling layer that makes harness engineering actionable — not just a philosophy but a set of integration patterns.

Internally, OpenAI reports that development cycles that took weeks now take days, and routine tasks (dependency maintenance, feature flag cleanup, documentation) are fully delegated to Codex. This aligns with CREAO's experience of 99%+ AI-written code, though OpenAI frames it through the lens of a larger organization where engineers still own critical decisions rather than a small team where one architect directs everything.

[[Garry Tan]] ([[Thin Harness, Fat Skills]]) provides the most architecturally prescriptive pattern yet: **thin harness, fat skills**. The principle is directional — push intelligence up into markdown skill files, push execution down into deterministic tooling, keep the harness minimal (~200 lines). This concretizes harness engineering into a three-layer architecture: fat skills on top (90% of value), thin CLI harness in the middle, deterministic application on the bottom.

Tan argues the productivity gap (2x vs. 100x, per Steve Yegge) comes entirely from architecture, not model intelligence — the same model produces radically different outcomes depending on the harness. The anti-pattern is a "fat harness with thin skills": 40+ tool definitions eating half the context window, slow MCP round-trips, REST API wrappers. Instead, he advocates purpose-built tooling that's fast and narrow.

A key contribution is the concept of **resolvers** — routing tables that load the right context on demand rather than front-loading everything into the prompt. Tan shrank his own CLAUDE.md from 20,000 lines to ~200 lines of pointers. This addresses a practical problem all AI-first teams face: how to give agents access to institutional knowledge without drowning them in context.

The most novel idea is **self-improving skills**: after an event, a skill reads NPS surveys, diarizes mediocre responses, extracts patterns, and writes new rules back into the skill files. The next run uses them automatically. At YC, this reduced "OK" ratings from 12% to 4%. Skills become permanent upgrades that never degrade and automatically improve when models improve.

Tan's follow-up ([[Resolvers: The Routing Table for Intelligence]]) deepens the operational reality of maintaining AI-first systems at scale. The core insight is that **agent systems are organizations** — skills are employees, the resolver is the org chart, filing rules are internal process, and the whole thing requires active governance. Without it, systems experience **context rot**: resolvers decay within ~90 days as new skills are added without updating routing, trigger descriptions drift from actual user phrasing, and the routing table becomes a historical artifact rather than a live system.

Concrete maintenance infrastructure includes: **trigger evals** (test suites of sample inputs with expected skill outputs), **check-resolvable** (a meta-skill that audits reachability — found 15% of capabilities were "dark" in Tan's 40+ skill system), and shared filing rules mandating every skill consult the resolver before writing. The endgame is self-healing resolvers that learn from observed task dispatch traffic via reinforcement learning. This directly addresses the question of how AGENTS.md configurations evolve: they must be actively maintained, tested, and eventually self-correcting.

Tan's [[The New Way To Build A Startup]] extends AI-first engineering from the engineering org to the entire company. He calls these **"20x companies"** — tiny teams that beat 20x-larger incumbents by automating every internal function (engineering, support, marketing, sales, hiring, QA, ops, design) rather than just one or two. This is the company-scale analogue of what [[Peter Pang]] called for when he argued AI-first must extend beyond engineering: here it is operationalized with three reproducible patterns. First, the **AI teammate** — Giga ML's "Atlas" agent runs across the product (browsing, editing policies, writing code) and serves DoorDash plus 10+ Fortune 500s alongside a single human FTE. Second, the **unified AI source of truth** — Legion Health's care-ops interface pulls patient history, scheduling, insurance, and messages into one view, enabling 4x patient growth with zero net new hires (one clinical lead, one patient-support person, one billing person, where traditional healthcare has full call-center departments). Third, **custom per-employee agents** — Phase Shift (12 people, AR automation) asks employees to document their manual tasks and then builds bespoke agents for each workflow, which has let them avoid hiring for entire functions like design (they use Magic Patterns instead). The framing evolves Parker Conrad's "compound startup" from product breadth to internal automation breadth; leanness becomes the competitive moat, and not hiring the next department becomes a core strategy.

[[Karl Stoney]]'s account from [[Autotrader]] ([[From PR Review Bot + ATAI CLI to an Autonomous Work Queue]]) reframes harness engineering through a **platform-engineering lens**: the harness is not a CLI script per developer but an org-wide queue, worker pool, and policy layer that any team can plug into. Where Tan's `~200`-line CLAUDE.md is a personal harness and Pang's CREAO is a company-scale process redesign, Stoney's `atai-api` is the **shared infrastructure** that sits between them — orchestration (intake, policy, scheduling, state) cleanly separated from execution (`atai` workers running `copilot-cli` on Kubernetes). The architectural lesson: agent platforms inherit the same separations that have always made systems scalable, and ad hoc per-trigger scripts collapse the moment a second ingestion source or a scheduled run is added.

A second contribution is the **deliberate framing of measurement**. Stoney explicitly rejects "10x productivity" and "hours saved" calculations — even when his own numbers (e.g., "500 PR reviews/week × 30 min = 250 hours") would support them — and argues the only honest metric for an opt-in platform is **adoption**. He compares AI infrastructure to CI/CD and cloud: the goal was never to work fewer hours but to remove monotony so humans focus on judgment. This is a counter-voice to existing wiki sources that lean on multiplier framings (CREAO's 99% AI-written code, Tan's 20x companies) and gives the theme a more nuanced position on how to evaluate AI-first transitions.

A third, often-overlooked dimension Stoney makes explicit: **agent reliability is downstream of platform legibility**. Autotrader's pre-existing investment in their Data Platform and Delivery Platform — first-class APIs that return deterministic answers about every service — is what lets their agents succeed. "AI doesn't have to guess anything." This concretises a thread implicit in Pang and Tan: the harness includes not just the prompt and tools but every internal API, dashboard, and CLI the agent might need to consult. Companies without that prior platform work will not get the same results from the same agent stack.

## Contradictions

**Contested:** [[Chat to Create AI Agents Ready to Work for You]] and [[The New Way To Build A Startup]] frame AI-first engineering through productivity multipliers (99% AI-written code; 20x companies) and treat per-FTE leverage as a defining outcome. [[From PR Review Bot + ATAI CLI to an Autonomous Work Queue]] explicitly rejects multiplier and "hours saved" framings as vanity metrics and argues that adoption of an opt-in platform is the only honest measure. The two views are not strictly logically incompatible — multipliers describe *outcomes*, adoption describes *evidence* — but they reflect a real disagreement about what makes an AI-first program credible.

## Open Questions

- How does AI-first engineering scale beyond small teams (25 people)? Does the Architect/Operator model hold at 100+ engineers? (The 20x companies thesis partially sidesteps this — don't scale to 100+. Autotrader's platform model offers a different answer: don't restructure roles, instead provide an internal agent platform that any team can consume.)
- What happens when the Architect becomes a bottleneck — is there a way to distribute that role?
- How do you handle the senior engineer transition without losing institutional knowledge?
- Is "vibe coding" (prompting until something works) ever appropriate, or always an anti-pattern in production systems?
- How do self-improving skill loops avoid drift — what prevents compounding rule additions from degrading skill quality over time?
- What does the RL loop for self-healing resolvers look like in practice — how much traffic data is needed, and how do you prevent the resolver from over-fitting to recent patterns?
- When do 20x companies *have* to hire? Are there functions that resist automation entirely, or does the frontier keep pushing outward?
- How do you avoid bus-factor risk when critical institutional knowledge lives inside agent systems managed by a handful of people?
- What is the right success metric for an internal agent platform — adoption (Stoney), tasks completed (CREAO), per-FTE leverage (Tan), or something else? Different metrics produce different platform decisions.
- How transferable is Autotrader's queue-and-worker pattern to organisations *without* a mature underlying platform (deterministic service APIs, standardised CI/CD)? Stoney's claim is that the agent stack only works because the platforms below it work.

## Sources

- [[Chat to Create AI Agents Ready to Work for You]]
- [[How Index Built an AI-First Data Analytics Platform with Mastra]]
- [[Building an AI-Native Engineering Team – Codex]]
- [[Thin Harness, Fat Skills]]
- [[Resolvers: The Routing Table for Intelligence]]
- [[The New Way To Build A Startup]]
- [[From PR Review Bot + ATAI CLI to an Autonomous Work Queue]]
