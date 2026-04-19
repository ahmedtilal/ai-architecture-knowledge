---
title: "Karl Stoney"
type: entity
entity_type: person
source_count: 1
tags: [platform-engineering, ai-agents, autotrader, ai-first-engineering]
---

## Overview

Karl Stoney is a platform engineer at [[Autotrader]] who has written publicly about building an internal autonomous AI agent platform around GitHub `copilot-cli`, with a strong opinionated stance on AI infrastructure as platform engineering rather than productivity theatre.

## Key Ideas

- **Agent platforms are queue platforms.** Multiple ingestion sources (GitHub, Jira, internal services) and multiple execution timings (now / `runAt` / `runEvery`) make ad hoc webhook scripts collapse; jobs become the first-class primitive on a FIFO queue with dedupe, identity, scheduling, status, and result/usage as standard fields. ([[From PR Review Bot + ATAI CLI to an Autonomous Work Queue]])
- **Separate orchestration from execution.** His architecture splits cleanly into `atai-api` (orchestration, policy, queue, state, UI) and `atai` (containerised worker runtime running `copilot-cli`). Each has a single responsibility and can scale independently on Kubernetes.
- **`copilot-cli` beats `gemini-cli` for headless engineering work.** Stoney is explicit that GitHub's CLI plus the `copilot-sdk` event API is materially better than `gemini-cli` for container-based agent execution; CLI scraping is a smell.
- **Tiny per-repo policy beats central config.** A 9-line `shippr.yaml` enables auto-review for PRs and issues; zero config is required for manual `@skippr` mention flows. Friction-free defaults are the adoption strategy.
- **Direct API enqueue is the wedge.** Once any internal service can `POST` a job, monitoring-triggered triage, post-deploy exploratory testing, scheduled service-health digests, and incident pre-investigation all become straightforward without the platform team needing to pre-bake each use case.
- **Underlying platforms make AI work.** Stoney attributes their speed to prior investment in Autotrader's Data Platform and Delivery Platform — first-class APIs that return deterministic answers about services. "AI doesn't have to guess anything." Garbage in, garbage out applies to agents.
- **Adoption is the only honest metric.** He explicitly rejects "hours saved" and "10x productivity" framings as vanity metrics. The platform is opt-in; continued daily use by engineers is the proof of value. The frame to use is CI/CD or cloud — infrastructure that removes monotony, not labour-replacement.

## Mentions

- [[From PR Review Bot + ATAI CLI to an Autonomous Work Queue]] — describes the merger of two prior efforts (a `gemini-cli` PR review bot and the opinionated `atai` developer CLI) into a single autonomous agent platform, including queue architecture, ingestion patterns (GitHub webhooks, Jira, direct API), `shippr.yaml` policy, and the philosophy of measuring success by adoption rather than hours saved.

## Related

- [[Autotrader]] — Stoney's employer; the platform context that makes his architecture viable
- [[AI-First Engineering]] — his work is a platform-engineering instantiation of harness engineering
- [[AI Agents]] — his queue-and-worker pattern is a concrete agent orchestration architecture
- [[Garry Tan]] — Tan's "thin harness, fat skills" thesis is reflected in Stoney's thin `atai-api` over a `copilot-cli` execution context
