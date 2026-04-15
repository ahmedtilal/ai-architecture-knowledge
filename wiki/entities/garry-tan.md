---
title: "Garry Tan"
type: entity
entity_type: person
source_count: 2
tags: [yc, y-combinator, harness-engineering, agent-architecture]
---

## Overview

Garry Tan is the president of Y Combinator. He is a practitioner and evangelist of AI-first engineering, having developed the "thin harness, fat skills" architectural pattern for building agent systems.

## Key Ideas

- **Thin harness, fat skills** — a three-layer architecture: fat markdown skill files on top (90% of value), thin CLI harness in the middle (~200 lines), deterministic application on the bottom
- Skill files work like method calls — same process, different parameters, radically different capabilities
- Five key definitions for agent architecture: skill files, the harness, resolvers, latent vs. deterministic, and diarization
- **Resolvers as management** — agent systems are organizations: skills are employees, the resolver is the org chart, filing rules are internal process, check-resolvable is audit/compliance, trigger evals are performance reviews
- **Context rot** — resolvers decay within ~90 days; proposes self-healing resolvers that learn from observed task dispatch traffic (RL loop)
- `check-resolvable` meta-skill — weekly linter that walks AGENTS.md → skill → code to find unreachable capabilities (found 15% dark capabilities in his system)
- "If I have to ask you for something twice, you failed" — every repeated task should be codified into a skill file
- Read Anthropic's leaked Claude Code source (512,000 lines) and found it confirmed the harness engineering principles he teaches at YC
- Runs a personal agent system: 40+ skills, 25,000 files, 200 inputs/day
- Open-sourced **GBrain** (knowledge layer with resolver), **GStack** (fat skills, 72K+ GitHub stars), and **OpenClaw/Hermes Agent** (thin harness conductor)

## Mentions

- [[Thin Harness, Fat Skills]] — primary article laying out his architectural philosophy and YC implementation
- [[Resolvers: The Routing Table for Intelligence]] — deep dive on resolvers: context rot, dark capabilities, trigger evals, check-resolvable, agents-as-organization framing

## Related

- [[AI-First Engineering]] — the broader movement his architecture operationalizes
- [[AI Agents]] — his agent design principles (latent vs. deterministic, resolvers, diarization)
- [[OpenAI]] — coined "harness engineering," which Tan extends with prescriptive architecture
