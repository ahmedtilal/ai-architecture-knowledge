---
title: "AI-First Engineering"
type: theme
source_count: 2
tags: [ai-first-engineering, harness-engineering, engineering-management, software-development]
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

## Contradictions

_None identified._

## Open Questions

- How does AI-first engineering scale beyond small teams (25 people)? Does the Architect/Operator model hold at 100+ engineers?
- What happens when the Architect becomes a bottleneck — is there a way to distribute that role?
- How do you handle the senior engineer transition without losing institutional knowledge?
- Is "vibe coding" (prompting until something works) ever appropriate, or always an anti-pattern in production systems?

## Sources

- [[Chat to Create AI Agents Ready to Work for You]]
- [[How Index Built an AI-First Data Analytics Platform with Mastra]]
