---
title: "AI Agents"
type: theme
source_count: 4
tags: [ai-agents, autonomous-systems, agent-platforms]
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

## Contradictions

_None identified._

## Open Questions

- What are the failure modes when agents operate with too much autonomy? Where are the human checkpoints most critical?
- How do agent systems handle novel problems that don't fit established patterns?
- What is the reliability ceiling for self-healing loops — do they converge or oscillate?
- How does persistent memory across agent sessions work in practice, and what are the risks of stale context?
- How does the 7-month doubling rate for sustained reasoning hold up — is it accelerating, plateauing, or task-dependent?
- How do self-improving skill loops avoid drift — can compounding rule additions degrade quality without human pruning?

## Sources

- [[Chat to Create AI Agents Ready to Work for You]]
- [[How Index Built an AI-First Data Analytics Platform with Mastra]]
- [[Building an AI-Native Engineering Team – Codex]]
- [[Thin Harness, Fat Skills]]
