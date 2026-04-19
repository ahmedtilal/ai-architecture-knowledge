---
title: "From PR Review Bot + ATAI CLI to an Autonomous Work Queue"
type: source
source_type: article
date_ingested: 2026-04-19
original_path: "raw/articles/From PR Review Bot + ATAI CLI to an Autonomous Work Queue.md"
tags: [ai-agents, ai-first-engineering, agent-orchestration, platform-engineering, autotrader, copilot-cli, harness-engineering]
---

## Summary

[[Karl Stoney]], a platform engineer at [[Autotrader]], describes how two previously separate efforts — a `gemini-cli`-powered PR review bot and an opinionated developer CLI called `atai` — were merged into a single autonomous agent platform. The merged architecture splits cleanly into two systems: `atai-api` (orchestration, queueing, policy, scheduling, state) and `atai` (a containerised worker runtime that executes agent tasks). Jobs are the first-class primitive on a FIFO queue; a swarm of Kubernetes-based workers pop jobs, execute them with `copilot-cli`, and stream events back via the `copilot-sdk` (replacing the prior approach of scraping CLI logs from disk).

The platform accepts work from many ingestion sources, normalising each into the same job shape: GitHub webhooks (`pull_request.opened`, `review_requested`, `issues.opened`, `issue.assigned`, `@mention` comments, `push`), Jira mentions/assignments, and direct API enqueues with optional `runAt`/`runEvery` scheduling. Per-repo behaviour is configured through a tiny `shippr.yaml` (≈9 lines turns on auto-review for PRs and issues). Engineers interact with the public face "Skippr" — assigning it as a reviewer, mentioning it in threads, or letting it auto-pick up `issue.opened` events.

Direct API enqueue is the most generative ingestion path: any internal service (monitoring, deploy, incident) can trigger an agent workflow. Examples include monitoring-detected errors that produce a triage issue or PR for the fix, post-deploy exploratory testing, scheduled daily service-health summaries delivered to Slack, and incident pre-investigation to reduce MTTR. Stoney attributes the speed of this build-out not to AI itself but to Autotrader's investment in their underlying Data Platform and Delivery Platform — first-class APIs that return deterministic answers about services, so the agent doesn't need to guess.

The closing argument deliberately rejects "10x productivity" framings. Stoney argues hours-saved is the wrong metric (engineers still validate AI work, faster shipping introduces new bugs) and that AI should be understood the way CI/CD or cloud was understood — infrastructure that removes monotony so humans can focus on judgment. The only metric that matters to him is **adoption**: the platform is opt-in, not mandated, and the fact that engineers use it daily is the proof.

## Key Takeaways

- **Agent orchestration is a queue problem.** Once you have multiple triggers (GitHub, Jira, API, schedules) and multiple execution timings (now vs. cron vs. `runAt`), "one webhook = one script" collapses. Jobs become the primitive; orchestration (`atai-api`) and execution (`atai` worker) separate cleanly.
- **First-class SDKs beat CLI scraping.** GitHub's [`copilot-sdk`](https://github.com/github/copilot-sdk) wraps `copilot-cli` with proper event hooks (`session.on('assistant.message', …)`), eliminating the brittle "run script, scrape logs" pattern they had with `gemini-cli`.
- **The `copilot-cli` vs. `gemini-cli` choice mattered.** A direct quote: "`gemini-cli` wasn't a patch on `copilot-cli` for software engineering workflows, particularly headless (container) ones." This is a concrete production data point on agent-CLI maturity for headless execution.
- **Job metadata enables real operations.** Jobs carry identity (who triggered), dedupe keys (no two in-flight runs for the same PR), scheduling, status, and final result + usage — which gives operational controls (pause/unpause workers, stale-job failure, observability) the original webhook script could not.
- **The `push` ingestion pattern is novel.** Engineers who don't use PRs can have `atai` review their pushed code; if critical issues are found, it raises a GitHub issue and **pauses the pipeline** to block promotion to further environments.
- **`shippr.yaml` is intentionally tiny.** Nine lines of YAML enable auto-review for PRs and issues; zero config is required for manual `@skippr` mentions. Friction-free defaults are part of the adoption strategy.
- **Direct API enqueue is the wedge for company-wide AI workflows.** Once any service can `POST` a job, monitoring-triggered triage, post-deploy exploratory testing, scheduled service-health digests, and incident pre-investigation become straightforward — and the platform team doesn't need to know about each use case.
- **Scheduled agent runs are a real product surface.** "Every day at 8am check all the services I own and post a summary in Slack" is a user-facing primitive on top of `runEvery`.
- **Underlying platform investment is load-bearing.** Stoney is explicit: AI works because the Data Platform and Delivery Platform have first-class APIs returning deterministic answers. Garbage in, garbage out applies to agents.
- **Self-fixing agents are observed in the wild.** When Skippr couldn't run tests because its container lacked `dotnet`, it raised a PR against itself to fix its own runtime environment.
- **Adoption is the only metric that matters.** Stoney explicitly rejects "hours saved" calculations (e.g., "500 PR reviews/week × 30 min = 250 hours") as vanity. The platform is opt-in, and continued daily use is the proof of value.

## Relevance

This source extends [[AI-First Engineering]] in a direction the existing wiki has only touched implicitly: the **platform-engineering perspective on agent orchestration**. Where [[Peter Pang]]'s [[CREAO]] story is a ground-up rebuild of one company's process and [[OpenAI]]'s guide ([[Building an AI-Native Engineering Team – Codex]]) is a phase-by-phase SDLC framework, Stoney is describing the **infrastructure layer underneath both**: a job queue, a worker pool, normalised ingestion, per-repo policy, and observability. This is "harness engineering" in the most literal, ops-flavoured sense.

It also concretises [[Garry Tan]]'s thesis from [[Thin Harness, Fat Skills]]. Stoney's `atai-api` is a thin orchestration harness; the intelligence sits in the `copilot-cli` execution context plus per-repo `shippr.yaml` policy. The architecture is also evidence for [[Resolvers: The Routing Table for Intelligence]] in a different register — Stoney's normalised-job pattern is itself a kind of resolver: every event type from every source (GitHub, Jira, monitoring, deploy) is mapped into the same job shape with the same metadata, so the worker doesn't need event-specific plumbing.

The `push`-as-pipeline-gate behaviour and the "raise a PR against itself when blocked" anecdote both add concrete failure-mode handling to the [[AI Agents]] theme. The Open Question on the [[AI Agents]] page about "where are the human checkpoints most critical?" gets a partial answer: at the point where critical issues are detected post-push, the agent escalates to a human-readable issue and halts pipeline progression, rather than merging or silently failing.

Stoney's "adoption is the only metric" framing is also a counter-voice in the wiki: existing sources ([[CREAO]], [[The New Way To Build A Startup]]) lean on productivity-multiplier framings (10x, 20x, 99% AI-written code), and this is the first source to explicitly reject that framing.

## Links

- Themes: [[AI Agents]], [[AI-First Engineering]]
- Entities: [[Karl Stoney]], [[Autotrader]]
- Related concepts that may warrant their own pages later: GitHub Copilot CLI (`copilot-cli`), `copilot-sdk`, MCP toolsets in `atai`'s runtime
