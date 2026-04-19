---
title: "AI Agents"
type: theme
source_count: 7
tags: [ai-agents, autonomous-systems, agent-platforms, ai-teammate, agent-orchestration]
---

## Overview

AI agents are autonomous systems that can plan, execute, and iterate on tasks with minimal human intervention — including code generation, deployment, error diagnosis, and self-healing workflows.

## Synthesis

[[CREAO]]'s experience ([[Chat to Create AI Agents Ready to Work for You]]) provides a production case study of agents operating across the full software lifecycle. Their agents decompose tasks, plan implementation, write code, generate tests, and open PRs. Separate agent workflows handle code review (three parallel Claude passes), error triage (clustering production errors, scoring severity, generating investigation tickets), and health monitoring (daily CloudWatch analysis delivered to the team).

The key architectural insight is that agents work best when the system is designed for their legibility: a unified monorepo (not fragmented services), structured logging, deterministic CI pipelines, and queryable signals. [[Peter Pang]] frames this as harness engineering — the environment around the agent matters more than the agent's raw capability.

Multiple independent teams (OpenAI, Anthropic, and others) have converged on common principles for effective agent systems: structured context, specialized agents, persistent memory, and execution loops. Model capability is described as "the clock driving this" — each generation of models (e.g., Opus 4.6 over 4.5) unlocks new levels of autonomous work.

The vision extends to one-person companies: if one architect with agents can do the work of 100 people, many companies won't need a second employee.

[[Index]]'s implementation ([[How Index Built an AI-First Data Analytics Platform with Mastra]]) demonstrates a different agent pattern: a **supervisor agent architecture** where a central router coordinates specialized nested workflows and agents. Their system includes data analyst agents for SQL generation and execution, visualization workflows for chart creation, and API integration tools for third-party data sources. This contrasts with CREAO's full-lifecycle agents — Index's agents are focused on a specific domain (data analytics) and sit as a conversational layer on top of an existing product.

A key enabler is **schema-aware agents**: Index's data access layer generates a unified schema across diverse sources (Salesforce, Stripe, PostHog, relational databases via BigQuery), and agents ingest this schema to produce accurate SQL. The consistent workflow — write SQL, get dataset, execute — makes agents more reliable by constraining their action space.

The choice of agent framework matters practically: Index found that TypeScript-native frameworks like [[Mastra]] with strong type safety and built-in playgrounds significantly accelerated development compared to Python-first frameworks with TypeScript bindings (LangChain).

[[OpenAI]]'s guide ([[Building an AI-Native Engineering Team – Codex]]) provides the broadest view of coding agents across the full SDLC. They frame agent capability through a concrete benchmark: METR found frontier models sustaining **2 hours 17 minutes** of continuous reasoning (as of August 2025), with task length doubling every ~7 months. This trajectory means agents can now produce full features end-to-end — data models, APIs, UI, tests, and documentation — in a single coordinated run.

OpenAI structures agent involvement across seven SDLC phases (Plan, Design, Build, Test, Review, Document, Deploy & Maintain) using a consistent **Delegate / Review / Own** framework. In each phase, agents handle first-pass mechanical work while engineers shift to architecture, product intent, and quality judgment. Key infrastructure patterns include **MCP servers** as the integration layer connecting agents to issue trackers, design tools, logging, and deployment systems, and **AGENTS.md** files as persistent configuration for agent behavior and guardrails.

A notable insight on testing: as agents remove barriers to code generation, tests become *more* important as the source of truth agents iterate against. OpenAI also recommends using models specifically trained for code review rather than general-purpose models, which tend to nitpick with low signal-to-noise ratio.

[[Garry Tan]] ([[Thin Harness, Fat Skills]]) introduces several agent design principles. The most fundamental is the **latent vs. deterministic** distinction: every step in an agent system must be classified as either latent (judgment, synthesis, pattern recognition) or deterministic (same input, same output). Confusing the two is "the most common mistake in agent design" — an LLM can seat 8 dinner guests accounting for social dynamics, but seating 800 requires combinatorial optimization that belongs in deterministic code.

Tan's **skill files** formalize how to encode agent capabilities: markdown documents that describe a *process* (not a task), working like method calls with parameters. The same `/investigate` skill with different arguments produces a medical research analyst or a forensic financial investigator. **Resolvers** route context on demand — loading the right document when the right task appears, rather than front-loading everything into the prompt.

**Diarization** is presented as the capability that makes AI genuinely useful for knowledge work: the model reads all sources about a subject and writes a structured profile, holding contradictions in mind and synthesizing intelligence. "No SQL query produces this. No RAG pipeline produces this." In practice at YC, diarization caught that a founder claiming "Datadog for AI agents" was actually building a FinOps tool (80% of commits in the billing module) — a gap no embedding search would find.

The most novel pattern is **self-improving agents**: after execution, a skill reads outcome data (NPS surveys), diarizes mediocre results, extracts patterns, and rewrites its own rules. The next run incorporates the lessons automatically.

Tan's follow-up ([[Resolvers: The Routing Table for Intelligence]]) reveals the failure modes that emerge at scale (40+ skills, 25,000 files, 200 inputs/day). The most insidious is not hallucination but **silent drift**: information filed in the wrong place, connections that don't form, knowledge bases that gradually become junk drawers. An audit found only 3 of 13 brain-writing skills consulted the resolver — the other 10 had hardcoded paths.

**Dark capabilities** are another failure mode: skills that exist but can't be reached because the resolver doesn't know about them. 15% of Tan's system's capabilities were unreachable. This is worse than missing capabilities because it creates an illusion of competence. The fix is `check-resolvable` — a meta-skill that walks AGENTS.md → skill file → code weekly to find dead links.

**Context rot** describes how resolvers decay: within ~90 days, new skills are added without updating routing, trigger descriptions drift from how users actually phrase requests, and the system works only because the operator manually invokes skills by name. Tan proposes self-healing resolvers powered by RL on task dispatch traffic as the endgame for agent governance.

Resolvers are **fractal** — they compose at every layer: skill resolvers (AGENTS.md maps tasks to skills), filing resolvers (RESOLVER.md maps content to directories), and context resolvers (internal routing within each skill). Claude Code already implements this pattern: every skill's description field is itself a resolver.

[[The New Way To Build A Startup]] introduces a third operational archetype beyond full-SDLC agents (CREAO) and supervisor agents (Index): the **AI teammate** — an agent that operates as a full-time virtual employee across an entire product. Giga ML's "Atlas" is the canonical example: it can use browsers, edit policies, write code, and service accounts, which lets the company serve DoorDash and 10+ Fortune 500s (each handling 500k–1M calls/day) alongside a single human FTE. Where coding agents are measured in tasks-completed and supervisor agents in successful routings, AI teammates are measured in **accounts covered per human FTE**. This archetype complements rather than replaces the others; a 20x company often runs all three in parallel. A related pattern from the same source is **per-employee custom agents** (Phase Shift's approach): rather than one general-purpose teammate, each human documents their manual work and gets a bespoke agent tailored to their workflow — a bottom-up version of Tan's skill-file pattern deployed company-wide rather than inside a single engineer's harness.

[[Karl Stoney]]'s account of [[Autotrader]]'s `atai` / `skippr` platform ([[From PR Review Bot + ATAI CLI to an Autonomous Work Queue]]) adds the **platform-engineering view of agent orchestration** — the layer underneath all of the above. Once an organisation has multiple ingestion sources (GitHub webhooks, Jira mentions, internal API enqueues) and multiple execution timings (now, `runAt`, `runEvery`), one-webhook-one-script collapses; **jobs become the first-class primitive** on a FIFO queue, with identity, dedupe keys, scheduling, status, and result/usage as standard fields. Autotrader's architecture splits cleanly into `atai-api` (orchestration, policy, queue, state, UI) and `atai` (a containerised worker pool running `copilot-cli` on Kubernetes). This separation is itself a design principle: orchestration logic and execution logic decouple, and worker mortality stops mattering once the queue tracks state.

Two operational details are worth recording. First, the **CLI-and-SDK choice matters**: Stoney is explicit that `gemini-cli` was not a patch on `copilot-cli` for headless container execution, and that GitHub's `copilot-sdk` (with first-class event hooks like `session.on('assistant.message', …)`) eliminated the prior brittle pattern of running CLIs and scraping log files. CLI scraping is now a smell. Second, the **`push` ingestion pattern** introduces a new agent-as-checkpoint behaviour: when an engineer pushes code (without opening a PR), `skippr` reviews it, and on detecting critical issues raises a GitHub issue *and pauses the pipeline* to block promotion to further environments. This is a concrete answer to the open question of where human checkpoints belong — escalate to a human-readable artefact and halt downstream automation, rather than merging or silently failing. Stoney also reports a self-fixing instance: when `skippr` couldn't run tests because its container lacked `dotnet`, it raised a PR against itself to fix its own runtime environment.

The **direct-API enqueue path** is the most generative of his ingestion patterns, because it lets the platform team stop being a bottleneck: any monitoring system, deploy pipeline, or incident tool can `POST` a job and trigger an agent workflow (debug a 500-error spike, run post-deploy exploratory testing, do incident pre-investigation in Slack, send a daily 8am service-health digest). Crucially, Stoney attributes the speed of this build-out not to AI but to Autotrader's prior investment in their **Data Platform and Delivery Platform** — first-class APIs that return deterministic answers about services, so agents never have to guess. This reinforces the harness-engineering thesis: agent reliability is a function of the legibility of the systems around the agent.

## Contradictions

_None identified._

## Open Questions

- What are the failure modes when agents operate with too much autonomy? Where are the human checkpoints most critical? (Stoney offers one answer: escalate to a human-readable issue *and* pause the pipeline at the point of critical-issue detection on `push`.)
- How do agent systems handle novel problems that don't fit established patterns?
- What is the reliability ceiling for self-healing loops — do they converge or oscillate?
- How does persistent memory across agent sessions work in practice, and what are the risks of stale context?
- How does the 7-month doubling rate for sustained reasoning hold up — is it accelerating, plateauing, or task-dependent?
- What does the RL loop for self-healing resolvers look like in practice — how much traffic is needed before the resolver can meaningfully self-correct?
- How should agent platforms measure success? Stoney rejects "hours saved" framings entirely and uses adoption as the only honest metric — does that hold up at organisations that need to justify spend to non-engineering leadership?
- What is the right boundary between "agents as job workers" (Autotrader's queue model) and "agents as long-running teammates" (Giga ML's Atlas)? Are they architecturally compatible or fundamentally different runtimes?

## Sources

- [[Chat to Create AI Agents Ready to Work for You]]
- [[How Index Built an AI-First Data Analytics Platform with Mastra]]
- [[Building an AI-Native Engineering Team – Codex]]
- [[Thin Harness, Fat Skills]]
- [[Resolvers: The Routing Table for Intelligence]]
- [[The New Way To Build A Startup]]
- [[From PR Review Bot + ATAI CLI to an Autonomous Work Queue]]
