---
title: "Garry Tan"
type: entity
entity_type: person
source_count: 3
tags: [yc, y-combinator, harness-engineering, agent-architecture, 20x-companies]
---

## Overview

Garry Tan is the president of [[Y Combinator]]. He is a practitioner and evangelist of AI-first engineering, having developed the "thin harness, fat skills" architectural pattern for agent systems and popularized the "20x companies" framing for startups that beat larger incumbents through internal automation.

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
- **"20x companies"** — his framing for tiny teams that beat 20x-larger incumbents through pervasive internal automation across every function (engineering, support, sales, ops, design, QA); an evolution of Parker Conrad's "compound startup" applied to internal automation rather than product breadth
- Three reproducible patterns for 20x companies: **AI teammate** (an agent as a full-time virtual employee), **unified AI source of truth** (one interface pulling all context for humans), **custom per-employee agents** (document manual work, then automate it)

## Mentions

- [[Thin Harness, Fat Skills]] — primary article laying out his architectural philosophy and YC implementation
- [[Resolvers: The Routing Table for Intelligence]] — deep dive on resolvers: context rot, dark capabilities, trigger evals, check-resolvable, agents-as-organization framing
- [[The New Way To Build A Startup]] — YC Main Function video where he extends the thesis from agent architecture to company architecture, introducing the 20x companies framing with Giga ML, Legion Health, and Phase Shift as case studies

## Related

- [[Y Combinator]] — the accelerator he runs; source of his portfolio-wide view on AI-first company building
- [[AI-First Engineering]] — the broader movement his architecture operationalizes
- [[AI Agents]] — his agent design principles (latent vs. deterministic, resolvers, diarization)
- [[OpenAI]] — coined "harness engineering," which Tan extends with prescriptive architecture
