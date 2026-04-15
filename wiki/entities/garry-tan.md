---
title: "Garry Tan"
type: entity
entity_type: person
source_count: 1
tags: [yc, y-combinator, harness-engineering, agent-architecture]
---

## Overview

Garry Tan is the president of Y Combinator. He is a practitioner and evangelist of AI-first engineering, having developed the "thin harness, fat skills" architectural pattern for building agent systems.

## Key Ideas

- **Thin harness, fat skills** — a three-layer architecture: fat markdown skill files on top (90% of value), thin CLI harness in the middle (~200 lines), deterministic application on the bottom
- Skill files work like method calls — same process, different parameters, radically different capabilities
- Five key definitions for agent architecture: skill files, the harness, resolvers, latent vs. deterministic, and diarization
- "If I have to ask you for something twice, you failed" — every repeated task should be codified into a skill file
- Read Anthropic's leaked Claude Code source (512,000 lines) and found it confirmed the harness engineering principles he teaches at YC
- Building real systems at YC: founder enrichment, event matching, and self-improving feedback loops for Startup School (6,000 founders, July 2026)

## Mentions

- [[Thin Harness, Fat Skills]] — primary article laying out his architectural philosophy and YC implementation

## Related

- [[AI-First Engineering]] — the broader movement his architecture operationalizes
- [[AI Agents]] — his agent design principles (latent vs. deterministic, resolvers, diarization)
- [[OpenAI]] — coined "harness engineering," which Tan extends with prescriptive architecture
