---
title: "REST API + Skill Documents vs MCP: Two Strategies for Connecting AI Agents to Backend Capabilities"
date: 2026-04-07
author: "Forrest Chai"
description: "REST APIs are not inherently weak contracts. MCP is not inherently superior. The real comparison requires distinguishing three layers of interface quality — prose documentation, machine-readable specification, and protocol-native tool discovery."
categories: "crowdlisten"
tags: ["Product", "AI Agents", "MCP", "API Design"]
keywords: ["REST API", "MCP", "Model Context Protocol", "OpenAPI", "skill documents", "AI agents", "agent architecture"]
thesis_featured: true
featured_rank: 2
---

As AI agents move from demo toys to production infrastructure, the question of how they connect to backend services has become a serious architectural decision. There are currently two dominant strategies: exposing capabilities through REST APIs paired with structured skill documents, and exposing capabilities through MCP (Model Context Protocol) servers. The popular framing — that REST is legacy and MCP is the future — oversimplifies the tradeoff. REST APIs, particularly when backed by machine-readable specifications like OpenAPI, offer strong contracts, broad compatibility, and mature tooling. MCP offers agent-native tool discovery, session-aware invocation, and push-based capability updates designed specifically for LLM tool use. Neither subsumes the other. This essay examines the two strategies across three dimensions — contract strength, orchestration control, and adaptability to code changes — and argues that the right choice depends on who your consumers are, what granularity they need, and how much backend volatility you expect them to absorb.

## 1. Introduction

When a backend service exposes capabilities for AI agents to consume, the interface design matters more than it appears. The choice is not merely technical plumbing; it shapes how much control the agent has over its workflow, how quickly the system adapts when backend code changes, and how much integration burden falls on each consumer.

Two strategies have emerged. The first is familiar: expose REST API endpoints and provide structured documentation — often called "skill files" or SKILL.md — that teaches the agent what endpoints exist, what parameters they accept, and when to use them. The second is newer: wrap the same capabilities in an MCP server, where each capability becomes a typed, discoverable tool that agents invoke through a standardized protocol rather than raw HTTP.

Much of the current discourse frames this as a replacement story: MCP replaces REST, just as REST once replaced SOAP. That framing is too simple. REST APIs with strong specifications are not weak contracts waiting to be superseded. MCP is not automatically superior to a well-designed API. The real comparison requires distinguishing three layers of interface quality — prose documentation, machine-readable specification, and protocol-native tool discovery — and understanding what each provides (OpenAPI Initiative, 2024; Model Context Protocol Specification, 2025).

## 2. What REST API + Skill Documents Actually Means

In this model, the backend is a standard HTTP service. It exposes endpoints — `POST /api/agents/analyze`, `GET /api/agents/capabilities`, and so on — that any HTTP client can call. The skill document is a structured markdown file that describes the available capabilities, recommended workflows, input/output expectations, and strategic guidance for when to use what.

A typical SKILL.md might read:

> For competitive analysis, use `search_content` to find discussions mentioning competitors, then `analyze_content` to extract sentiment per competitor, then `cluster_opinions` to group competitive comparisons by theme. Output should follow the Share of Voice template.

This is genuinely useful. It gives an LLM agent workflow intelligence — not just what endpoints exist, but how to think about the problem and in what order to approach it. A well-written skill document is closer to a senior colleague's briefing than to API reference documentation.

But it is important to be precise about what kind of contract this provides. A prose skill document is a **cognitive contract**: it constrains how the agent should reason about the task. It is not a **technical contract**: it does not enforce parameter types, validate request shapes, or guarantee response structures. If the backend renames a field from `platforms` to `sources`, the skill document does not catch the mismatch. The agent calls the old parameter name; the request fails or silently drops the field (Xano, 2026).

## 3. The OpenAPI Layer That Most Comparisons Ignore

Here is where the popular "REST is weak, MCP is strong" argument breaks down. REST does not inherently lack formal contracts. The OpenAPI Specification — the dominant standard for HTTP API description — is a machine-readable, structured interface definition that exists precisely to solve the problems attributed to prose documentation.

The OpenAPI Initiative defines it clearly: the specification allows "both humans and computers to discover and understand the capabilities of a service without requiring access to source code, additional documentation, or inspection of network traffic" (OpenAPI Specification v3.1, 2024). This is not documentation in the informal sense. It is a declarative contract that specifies paths, methods, parameters, request bodies, response schemas, authentication methods, status codes, and data types in a standardized format.

From a contract-strength perspective, OpenAPI provides:

- **Machine readability.** The spec is YAML or JSON, parseable by any tool without natural-language interpretation.
- **Schema validation.** Field types, required parameters, enum values, and formats (like `email` or `uuid`) can be validated before the request is sent.
- **Code generation.** Client SDKs, mock servers, and test suites can be generated directly from the spec.
- **Change detection.** Schema diffs clearly surface breaking changes — a renamed field, a new required parameter, a removed endpoint.

According to Postman's 2025 State of the API Report, 82% of organizations have adopted some level of API-first development, and machine-readable API descriptions have moved from best practice to mandatory in many platforms (Programming Helper Tech, 2026).

So the accurate comparison is not "REST + prose docs vs. MCP + typed schema." It is three levels:

1. **REST + prose skill document**: human-readable workflow guidance, no machine validation.
2. **REST + OpenAPI specification**: machine-readable contract, schema-validated, broadly tooled.
3. **MCP + protocol-native tool discovery**: typed tool schema, dynamic discovery, push-based updates, agent-native invocation.

The first is genuinely fragile. The second and third are both strong contracts — but they operate at different abstraction layers and serve different consumers (OpenAPI Documentation, 2024; BuildFastWithAI, 2026).

## 4. MCP: What It Actually Provides

The Model Context Protocol, introduced by Anthropic in November 2024 and donated to the Agentic AI Foundation under the Linux Foundation in December 2025, is an open standard for connecting LLM applications to external tools, data sources, and services. As of 2026, it has been adopted by Claude Code, Cursor, Windsurf, Gemini CLI, Codex, and other major agent runtimes (Model Context Protocol Blog, 2025; Wikipedia, 2026).

The protocol defines several primitives, but the one most relevant to this comparison is **tools**. An MCP server exposes tools — named, described, schema-typed callable functions — that connected agents can discover and invoke. The specification defines the mechanism precisely:

- **`tools/list`**: the client requests the server's available tools. The server returns an array of tool definitions, each with a name, description, and `inputSchema` (a JSON Schema describing the expected parameters).
- **`notifications/tools/list_changed`**: when the server's tool list changes — a tool is added, removed, or modified — servers that declared the `listChanged` capability send a push notification to connected clients.
- **Tool invocation**: the client calls a tool by name with arguments conforming to the `inputSchema`. The server validates, executes, and returns structured results.

This is meaningfully different from "REST API with a spec." Not because the schema is stronger — both MCP's `inputSchema` and OpenAPI's request body schema are JSON Schema under the hood. The difference is the **abstraction layer** and **discovery model** (MCP Specification, 2025).

In a REST API, even with OpenAPI, the consumer thinks in terms of HTTP operations: paths, methods, headers, status codes, request bodies. An agent using a REST API must construct URLs, set headers, manage authentication per request, handle pagination, and interpret status codes. The OpenAPI spec describes these operations precisely, but the agent must still operate at the HTTP level.

In MCP, the consumer thinks in terms of **tools**: named functions with typed inputs. The agent does not construct URLs or manage HTTP headers. It calls `run_analysis({ query: "...", platforms: ["reddit"] })` and receives structured results. The transport layer — whether stdio, HTTP, or SSE — is handled by the MCP client library. The agent never sees it.

This distinction matters because LLM agents reason about tool use, not HTTP operations. The function-calling pattern used by modern models (Claude, GPT-4, Gemini) is: "here are the tools available, here are their schemas, pick one and provide arguments." MCP maps directly onto this pattern. REST API + OpenAPI can be mapped onto it, but requires an additional translation layer — which is precisely what systems like AutoGen, CrewAI, and LangChain build when they read OpenAPI specs to let agents call APIs autonomously (Xano, 2026; AWS, 2026).

## 5. Orchestration: Who Controls the Workflow?

The deeper architectural question is not just how the agent connects to capabilities, but **how much control the agent has over the workflow**.

### Coarse-Grained REST: Backend Orchestrates

When a REST API exposes a high-level endpoint like `POST /api/agents/analyze`, the backend controls the entire pipeline. The agent submits a query; the backend runs search, synthesis, clustering, and follow-up generation internally; the agent receives the final result. The skill document describes when to call this endpoint and how to interpret the response, but the agent has no ability to intervene mid-pipeline.

This is appropriate when the backend's pipeline is well-tuned and the consumer does not need step-level control. It is the simplest integration pattern: one HTTP call, one result. But it means the agent cannot skip irrelevant platforms, cannot branch based on intermediate results, and cannot inject its own reasoning between pipeline stages.

### Fine-Grained REST: Agent Orchestrates via HTTP

Nothing prevents a REST API from exposing fine-grained endpoints — `POST /search`, `POST /analyze`, `POST /cluster` — and letting the agent compose them. With an OpenAPI spec describing each endpoint, the agent can plan multi-step workflows. Some frameworks already do this: Microsoft Copilot Studio reads OpenAPI specs to let agents chain REST calls autonomously (Microsoft, 2026).

The challenge is practical, not theoretical. Each REST call requires the agent to manage authentication, construct URLs, handle errors, and maintain state across calls. The cognitive overhead is higher than calling named tools with typed schemas. It works, but it is not what the interface was designed for.

### MCP: Agent Orchestrates via Tools

MCP's tool model is designed exactly for the agent-orchestration pattern. The agent sees a flat list of typed tools, picks the ones it needs, and composes them in whatever order its reasoning suggests. There is no URL construction, no header management, no status code interpretation. The agent's reasoning loop stays at the semantic level: "I need to search Reddit, then analyze the results, then cluster opinions."

This matters most when the agent is sophisticated enough to benefit from step-level control. A simple automation script does not need to reason about intermediate results. But a Claude Code or Cursor agent working on a complex research task benefits significantly from being able to search one platform, assess the signal quality, decide whether to search another, and only then synthesize.

The orchestration comparison is therefore not binary but contextual:

| Consumer type | Best fit |
|---|---|
| Simple automation, webhook, CI/CD | Coarse-grained REST endpoint |
| Third-party developer building a specific integration | Fine-grained REST + OpenAPI |
| LLM agent doing multi-step reasoning | MCP tools |

## 6. Adaptability: What Happens When Code Changes?

This is where the architectural difference becomes most consequential for engineering teams.

### Level 1: REST + Prose Skill Document

When the backend changes, the skill document must be manually updated. There is no enforcement mechanism. If the document drifts from the implementation, agents follow stale instructions and fail at runtime — or worse, succeed silently with wrong parameters.

Consider: the backend adds an optional `language` parameter to the analysis endpoint. The skill document is not updated. Every agent that reads the skill document continues to work, but none of them can filter by language. The capability exists but is invisible. There is no mechanism to surface the change.

Now consider a breaking change: the `platforms` parameter is renamed to `sources`. Agents using the old skill document send `platforms`; the backend ignores the unrecognized field or returns a validation error. The skill document update helps future agents but cannot reach agents that already cached the old document.

### Level 2: REST + OpenAPI

OpenAPI significantly improves this. The spec is the contract; changes to the spec are diffable and detectable. If `platforms` is renamed to `sources`, the schema diff is explicit. Client SDK regeneration catches the break at compile time. Automated contract tests can detect the mismatch in CI.

However, OpenAPI is pull-based. The client must re-fetch the spec to learn about changes. There is no built-in push notification. Agents using cached or embedded specs will not know about new capabilities or breaking changes until they explicitly refresh. For long-running agent sessions, this means the agent operates with a stale view of the API until restart.

### Level 3: MCP

MCP adds two things OpenAPI does not: **push-based discovery** and **an adapter layer**.

Push-based discovery: when the server's tool list changes, the `notifications/tools/list_changed` event notifies connected agents in real-time. An agent in a long-running session learns about new tools, modified schemas, or removed capabilities without restarting or re-fetching. This is a meaningful improvement over OpenAPI's pull model for the specific case of long-lived agent sessions (MCP Specification, 2025).

The adapter layer is an architectural choice, not an inherent MCP feature, but it is the common deployment pattern. When an MCP server (like a harness) sits between agents and the backend, it absorbs backend changes. A field rename in the backend becomes a mapping change in the harness; agents calling the MCP tool are unaffected. A new backend endpoint becomes a new MCP tool; connected agents discover it automatically. A full backend rewrite — FastAPI to Go, monolith to microservices — changes only the harness internals.

But this benefit deserves an honest caveat. The "update once, propagate everywhere" claim holds only when all agents connect to the same MCP server instance, trust it, and refresh their tool lists promptly. In practice, there may be multiple harness versions deployed, different client caching behaviors, and varied permission configurations. MCP reduces integration duplication, but it does not automatically eliminate compatibility management. It turns the N×M problem into N+M, not into 1 (WorkOS, 2026; Tallyfy, 2026).

### Honest Comparison Matrix

| Change type | REST + Prose | REST + OpenAPI | MCP |
|---|---|---|---|
| Add optional parameter | Update doc; agents may never notice | Update spec; agents re-fetch to see it | Update inputSchema; `list_changed` pushes to connected agents |
| Breaking field rename | Agents break silently | Schema diff catches it; SDK regen needed | Harness maps old→new internally, or update schema + push |
| Add new capability | New doc section; existing agents unaware | New operation in spec; agents must re-fetch | New tool in `tools/list`; `list_changed` notifies immediately |
| Remove capability | Delete from doc; agents get 404 | Remove from spec; SDK regen catches it | Remove tool; `list_changed` notifies; clean error if called |
| Backend rewrite | All HTTP integrations break | All client SDKs need regeneration | Only harness internals change; agents see same tools |

## 7. The Role of Skill Documents Across Both Models

A subtlety that gets lost in the REST-vs-MCP framing: skill documents serve a purpose that neither OpenAPI nor MCP replaces.

OpenAPI answers: **what can I call, and what shape must the request be?**

MCP answers: **what tools are available right now, and how do I invoke them?**

A skill document answers: **given this kind of problem, what is the recommended strategy?**

These are three different questions. The first two are technical contracts. The third is cognitive guidance. A competitive analysis SKILL.md does not just list tools; it describes a five-step analytical workflow — share of voice analysis, sentiment comparison, switching intent detection, white space identification, feature gap analysis — with output templates and strategic reasoning.

In the REST + SKILL.md model, the skill document is load-bearing. It is the only thing teaching the agent how to approach the problem, which endpoints to call, and in what order. If the skill document is wrong or stale, the agent has no fallback.

In the MCP model, the skill document is enrichment. Even without it, the agent can discover tools, read their descriptions, and compose a reasonable workflow. But with a skill document injected into context, the agent gets expert-level strategic guidance on top of the tool surface. The skill document shifts from "essential contract" to "optional but valuable context."

This is why maintaining skill documents remains worthwhile regardless of which integration strategy you choose. They provide a layer of intelligence that neither API specs nor protocol schemas can encode: the judgment of when to use what, and why.

## 8. When to Choose What

The choice is not binary. In most production systems, both strategies coexist — often with MCP wrapping REST internally. The question is which to prioritize as the **primary consumer interface**.

**Prioritize REST API + OpenAPI when:**

- Your primary consumers are third-party developers building specific integrations in their own codebases.
- Consumers are non-LLM systems: webhooks, CI/CD pipelines, scheduled jobs, internal services.
- You need maximum compatibility — any language, any platform, any HTTP client.
- Your API surface is relatively stable and consumers can regenerate SDKs when it changes.
- The workflow is well-defined and coarse-grained: one call, one result.

**Prioritize MCP when:**

- Your primary consumers are LLM agents (Claude Code, Cursor, Gemini CLI, Windsurf).
- Consumers benefit from fine-grained tool composition and multi-step reasoning.
- Your backend changes frequently and you want an adapter layer to absorb volatility.
- You want push-based capability discovery for long-running agent sessions.
- Token efficiency matters — progressive tool disclosure (activating skill packs on demand) can reduce tool-listing tokens by 85–98% compared to exposing everything statically.

**Maintain skill documents regardless:**

- They provide workflow intelligence that neither OpenAPI nor MCP schemas encode.
- In the REST model, they are the primary integration guide.
- In the MCP model, they are optional context enrichment that improves agent performance on complex tasks.

## 9. Conclusion

The framing of "REST API vs MCP" is imprecise in the same way "prompt engineering vs context engineering" was imprecise a year ago. REST APIs are not inherently weak contracts — with OpenAPI, they provide schema-validated, machine-readable, broadly tooled interface descriptions. MCP is not inherently superior — its advantage is not stronger contracts but agent-native discovery and invocation, designed for the specific pattern of LLM tool use.

The more accurate framing is: REST + OpenAPI is an excellent contract for **programmatic consumers** who know what they want to call. MCP is an excellent contract for **reasoning consumers** who need to discover, evaluate, and compose capabilities dynamically. Skill documents provide **strategic guidance** that neither technical contract encodes.

For teams building AI-agent-facing services today, the practical recommendation is: build the REST API as your capability layer, describe it with OpenAPI as your technical contract, wrap it in MCP as your agent-facing interface, and maintain skill documents as your cognitive layer. These are not competing strategies. They are layers of the same stack, each solving a different problem at a different altitude.

---

**References**

Anthropic. "Introducing the Model Context Protocol." 2024. [Link](https://www.anthropic.com/news/model-context-protocol)

Model Context Protocol. "Specification — 2025-11-25." 2025. [Link](https://modelcontextprotocol.io/specification/2025-11-25)

Model Context Protocol. "Tools — Server Specification." 2025. [Link](https://modelcontextprotocol.io/specification/2025-06-18/server/tools)

Model Context Protocol Blog. "One Year of MCP: November 2025 Spec Release." 2025. [Link](https://blog.modelcontextprotocol.io/posts/2025-11-25-first-mcp-anniversary/)

OpenAPI Initiative. "OpenAPI Specification v3.1.0." 2024. [Link](https://spec.openapis.org/oas/v3.1.0)

OpenAPI Initiative. "Introduction — OpenAPI Documentation." 2024. [Link](https://learn.openapis.org/introduction.html)

Swagger. "OpenAPI Specification — Version 3.1.0." 2024. [Link](https://swagger.io/specification/)

WorkOS. "MCP vs. REST: What's the right way to connect AI agents to your API?" 2026. [Link](https://workos.com/blog/mcp-vs-rest)

Tallyfy. "MCP, AI agents, and REST APIs compared." 2026. [Link](https://tallyfy.com/mcp-agents-rest-apis/)

Xano. "OpenAPI Specification Guide (2026): AI Agents, MCP, & API Design." 2026. [Link](https://www.xano.com/blog/openapi-specification-the-definitive-guide/)

Atlan. "MCP vs API: When to Use Each for AI Agent Integration in 2026." 2026. [Link](https://atlan.com/know/when-to-use-mcp-vs-api/)

Programming Helper Tech. "API-First Development 2026: REST, OpenAPI, and the Developer Experience Layer." 2026. [Link](https://www.programming-helper.com/tech/api-first-development-2026-rest-openapi-developer-experience-python)

BuildFastWithAI. "API Design and Documentation with AI: OpenAPI, Schema Validation, and Contract-First Development." 2026. [Link](https://buildfastwith.ai/ai-api-design-documentation)

Microsoft. "Extend your agent with tools from a REST API." 2026. [Link](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agent-extend-action-rest-api)

AWS. "Enabling AI agents to interact with existing applications." 2026. [Link](https://aws.amazon.com/blogs/migration-and-modernization/enabling-ai-agents-to-interact-with-existing-applications/)
