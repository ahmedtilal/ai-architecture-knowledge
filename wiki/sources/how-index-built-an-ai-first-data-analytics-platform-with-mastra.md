---
title: "How Index Built an AI-First Data Analytics Platform with Mastra"
type: source
source_type: article
date_ingested: 2026-04-15
original_path: "raw/articles/How Index Built an AI-First Data Analytics Platform with Mastra.md"
tags: [ai-agents, data-analytics, mastra, typescript, conversational-bi]
---

## Summary

[[Index]] is a BI platform with over 1,000 customers that enables non-technical users to connect data warehouses and APIs into a BigQuery-based data access layer for streamlined analysis. Founded by Xavier Pladevall and Eduardo Portet — childhood friends from the Dominican Republic who studied CS in the US — Index has invested heavily in UX with drag-and-drop dashboards and real-time shared cursors.

Their next step was adding a conversational AI layer: a data analyst agent that lets users query their data in natural language. The pipeline works as: user describes what they want, the agent generates SQL, the backend executes it, and the frontend renders charts or tables. They initially used LangChain but found its Python-first ecosystem frustrating for their TypeScript stack — poor type safety and inconsistent type enforcement in functions.

They switched to [[Mastra]], a TypeScript-native agent framework. The immediate "aha moment" was the built-in playground (`yarn mastra dev`) that provided visual feedback for rapid iteration. They implemented a supervisor agent architecture — a central router coordinating specialized nested workflows: data analyst agents for SQL generation, visualization workflows for chart creation, and API integration tools for third-party data sources.

Eduardo Portet praised Mastra's opinionated design for reducing cognitive load and its type safety for boosting development velocity. The playground allowed them to move from concept to working prototype quickly, which was critical for a small team building an ambitious product.

## Key Takeaways

- A supervisor agent pattern (central router + specialized nested agents/workflows) is effective for complex data analytics tasks
- TypeScript-first frameworks with strong type safety provide significant DX advantages over Python-first frameworks with TypeScript bindings
- A unified data access layer (BigQuery-based, connecting Salesforce, Stripe, PostHog, relational DBs) creates a consistent workflow for agents: write SQL, get dataset, execute on dataset
- Rich schema ingestion makes agents more powerful — the data access layer's schema is a key differentiator
- Built-in playgrounds and local dev servers dramatically accelerate agent development for small teams
- Opinionated frameworks reduce cognitive load and let developers focus on product rather than infrastructure decisions

## Relevance

This article provides a concrete case study of adding an agent layer to an existing product — contrasting with [[CREAO]]'s approach of building an entire company around AI agents from the ground up. It demonstrates the practical value of TypeScript-native tooling for agent development and introduces the supervisor agent pattern as an architecture for coordinating specialized agents. Connects to [[AI Agents]] as a production deployment pattern and to [[AI-First Engineering]] as an example of a product company transitioning to AI-native capabilities.

## Links

- [[Index]] — the company building the platform
- [[Mastra]] — the agent framework powering their system
- [[AI Agents]] — supervisor agent architecture pattern
- [[AI-First Engineering]] — transitioning an existing product to AI-native
