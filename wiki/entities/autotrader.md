---
title: "Autotrader"
type: entity
entity_type: organization
source_count: 1
tags: [platform-engineering, ai-agents, ai-first-engineering, agent-orchestration]
---

## Overview

Autotrader is a UK-based automotive marketplace whose platform engineering team, led in part by [[Karl Stoney]], has built `atai` / `skippr` — an internal autonomous AI agent platform built around GitHub `copilot-cli`, a Kubernetes-based worker pool, and a job-queue orchestrator that ingests work from GitHub, Jira, and direct API calls.

## Key Ideas

- **In-house agent platform as a single product.** Autotrader merged a previously separate `gemini-cli` PR review bot and an opinionated developer CLI (`atai`) into one architecture: `atai-api` (orchestration, policy, queueing, scheduling, state, UI) plus `atai` (containerised worker runtime executing `copilot-cli`). Engineers see this as one product called "Skippr".
- **Multiple ingestion paths, one job shape.** Skippr handles GitHub webhooks (`pull_request.opened`, `review_requested`, `issues.opened`, `issue.assigned`, comment `@mentions`, `push`), Jira (`@Skippr` mentions, assignments), and direct API enqueue with `runAt` / `runEvery` scheduling. All become normalised jobs with identity, dedupe keys, scheduling, status, and result.
- **`push` ingestion as a pipeline gate.** Engineers who don't use PRs can have Skippr review pushed code; if critical issues are detected, Skippr raises a GitHub issue and pauses the pipeline to block promotion to further environments — a concrete agent-as-checkpoint pattern.
- **Per-repo policy is tiny.** A 9-line `shippr.yaml` (`codeAssist.pullRequests`, `codeAssist.issues` with `enabled` and `on:` event lists) turns on automatic agent involvement; zero config is needed for manual `@skippr` mention flows.
- **Direct-API enqueue is the wedge for company-wide AI workflows.** Once any internal service can `POST` a job, Autotrader has wired Skippr into monitoring (errors → triage issue or fix PR), deploys (post-deploy exploratory testing), incidents (preliminary investigation into Slack), and scheduled tasks ("daily 8am service-health summary in Slack"). The platform team doesn't need to know each use case in advance.
- **The Data Platform and Delivery Platform are load-bearing.** Autotrader's prior investment in standardised build / test / deploy and first-class APIs that return deterministic answers about services is the foundation that makes their agents reliable. AI doesn't have to guess; the answer is always available via API, dashboard, or CLI with consistent behaviour.
- **Adoption-as-metric.** The platform is opt-in for engineers, with no top-down mandate; continued daily use is the only measure of value the team accepts. Stoney explicitly rejects "hours saved" and "10x productivity" calculations as vanity.
- **Self-fixing runtime.** Skippr was observed raising a PR against itself when it discovered its container lacked `dotnet`, blocking it from running tests on a target repo — a small but concrete instance of an agent extending its own capabilities.

## Mentions

- [[From PR Review Bot + ATAI CLI to an Autonomous Work Queue]] — Stoney's account of the merged `atai` / `skippr` platform: the queue-based architecture, the GitHub / Jira / direct-API ingestion patterns, `shippr.yaml`, the `copilot-cli` choice over `gemini-cli`, and the platform-engineering philosophy that underpins it.

## Related

- [[Karl Stoney]] — author and platform engineer behind `atai` / `skippr`
- [[AI-First Engineering]] — Autotrader's pattern is a platform-engineering instantiation of harness engineering
- [[AI Agents]] — concrete production case study of orchestrated agent execution at organisation scale
