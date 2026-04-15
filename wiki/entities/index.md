---
title: "Index"
type: entity
entity_type: organization
source_count: 1
tags: [data-analytics, bi-platform, ai-agents, conversational-bi]
---

## Overview

Index is a BI platform founded by Xavier Pladevall and Eduardo Portet that enables non-technical users (PMs, CEOs) to connect data warehouses and APIs into a BigQuery-based data access layer for streamlined analysis. It has over 1,000 customers.

## Key Ideas

- Built a unified data access layer connecting sources like Salesforce, Stripe, PostHog, and relational databases through BigQuery
- Adding a conversational AI layer using a supervisor agent architecture powered by [[Mastra]]
- Transitioned from LangChain to Mastra for TypeScript-native type safety and better developer experience
- The data access layer's schema is what makes their agents especially powerful — it creates a consistent "write SQL, get dataset, execute" workflow

## Mentions

- [[How Index Built an AI-First Data Analytics Platform with Mastra]] — primary case study of their agent-powered analytics platform and adoption of Mastra

## Related

- [[Mastra]] — agent framework powering their system
- [[AI Agents]] — their supervisor agent architecture
- [[AI-First Engineering]] — transitioning existing product to AI-native capabilities
