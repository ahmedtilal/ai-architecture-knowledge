---
title: "AI Agents"
type: theme
source_count: 1
tags: [ai-agents, autonomous-systems, agent-platforms]
---

## Overview

AI agents are autonomous systems that can plan, execute, and iterate on tasks with minimal human intervention — including code generation, deployment, error diagnosis, and self-healing workflows.

## Synthesis

[[CREAO]]'s experience ([[Chat to Create AI Agents Ready to Work for You]]) provides a production case study of agents operating across the full software lifecycle. Their agents decompose tasks, plan implementation, write code, generate tests, and open PRs. Separate agent workflows handle code review (three parallel Claude passes), error triage (clustering production errors, scoring severity, generating investigation tickets), and health monitoring (daily CloudWatch analysis delivered to the team).

The key architectural insight is that agents work best when the system is designed for their legibility: a unified monorepo (not fragmented services), structured logging, deterministic CI pipelines, and queryable signals. [[Peter Pang]] frames this as harness engineering — the environment around the agent matters more than the agent's raw capability.

Multiple independent teams (OpenAI, Anthropic, and others) have converged on common principles for effective agent systems: structured context, specialized agents, persistent memory, and execution loops. Model capability is described as "the clock driving this" — each generation of models (e.g., Opus 4.6 over 4.5) unlocks new levels of autonomous work.

The vision extends to one-person companies: if one architect with agents can do the work of 100 people, many companies won't need a second employee.

## Contradictions

_None yet — only one source._

## Open Questions

- What are the failure modes when agents operate with too much autonomy? Where are the human checkpoints most critical?
- How do agent systems handle novel problems that don't fit established patterns?
- What is the reliability ceiling for self-healing loops — do they converge or oscillate?
- How does persistent memory across agent sessions work in practice, and what are the risks of stale context?

## Sources

- [[Chat to Create AI Agents Ready to Work for You]]
