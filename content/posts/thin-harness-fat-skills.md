---
title: "Thin Harness, Fat Skills: What gbrain Taught Me About Building AI Products"
date: 2026-04-14
author: "Forrest Chai"
description: "The next generation of AI products will not be won by thicker harnesses or smarter prompts. It will be won by thin harnesses and fat skills."
categories: "crowdlisten"
tags: ["Product", "AI Agents", "Harness Engineering", "Skills"]
keywords: ["thin harness", "fat skills", "gbrain", "AI product architecture", "agent skills", "harness engineering", "Garry Tan"]
thesis_featured: true
featured_rank: 1
---

Most people still explain AI product progress with the wrong variable.

They point to the model. Better reasoning. Better coding. Better multimodal understanding. More intelligence.

But after reading Garry Tan's *Thin Harness, Fat Skills* and then looking through the gbrain repo, I came away with a different conclusion. The real divide is not between products with smarter models and products with weaker ones. It is between systems that know what belongs in the harness and systems that keep stuffing more and more capability into the wrapper around the model. Garry's argument is sharp: the harness should stay thin; the skills should be fat. The harness runs the loop, manages context, reads and writes files, and enforces safety. The skills encode judgment, process, and domain knowledge. That is where the value lives.

That framing matters because a lot of AI products are still being built backwards. When something does not work, the instinct is to add another tool, another layer of orchestration, another giant system prompt, another MCP round-trip, another abstraction. The result is often a bloated harness and thin skills: heavy infrastructure, weak judgment. Garry calls that the anti-pattern directly. The point is not to make the wrapper fancier. The point is to keep the wrapper minimal enough that the model gets the right context, at the right time, without drowning in noise.

What gbrain made clear to me is that Thin Harness, Fat Skills is not just a slogan. It is a product architecture.

The repo's README describes GBrain as a system where meetings, emails, tweets, calendar events, calls, and ideas flow into a searchable knowledge base that the agent reads before every response and writes to after every conversation. It explicitly frames this as a compounding loop: signal arrives, the agent checks the brain first, responds with context, writes new information back, and syncs the result for the next query. In other words, the product is not just an assistant. It is a memory system that gets more useful every cycle.

That is where the "thin harness" part becomes important. Gbrain's README is explicit that the skill logic does not live in the binary; the skills are "fat markdown files that tell AI agents HOW to use gbrain." The engine layer handles search, storage, chunking, embeddings, indexing, and interface parity across CLI and MCP. The harness is intentionally kept closer to infrastructure than intelligence. The intelligence is pushed upward into markdown procedures. That is almost a direct implementation of Garry's thesis: keep execution in deterministic systems, and push judgment into skills.

The enrich skill is probably the clearest example. Its philosophy section says a brain page should read like an intelligence dossier, not a LinkedIn scrape, because "facts are table stakes" and "texture is the value": what a person believes, what they are building, what makes them tick, and where they are headed. That is a perfect expression of what "fat skills" actually means. A fat skill does not just tell the model to summarize a profile. It encodes a richer standard of judgment. It tells the system what kind of understanding is worth producing.

This also explains why markdown matters so much in both Garry's piece and gbrain. Garry argues that a skill file is not just a prompt; it is closer to a method call, a reusable procedure that takes different parameters in different worlds. Gbrain extends the same logic: "markdown is code," the recipe is the installer, and the skills are the durable place where behavior lives. That is a very different philosophy from treating prompts as disposable strings buried inside the application layer. It treats procedures, heuristics, and domain knowledge as first-class product assets.

The deeper lesson, though, is not merely that skills should exist. It is that thin harnesses make fat skills possible.

A fat harness tends to absorb everything. Tool sprawl. Context bloat. Too many interfaces. Too much orchestration logic where the model is supposed to think. As the wrapper gets thicker, the actual reusable intelligence in the system often gets weaker. The product starts to depend on fragile plumbing rather than durable judgment. By contrast, a thin harness creates a cleaner division of labor. The deterministic substrate handles what should be reliable: storage, retrieval, indexing, sync, versioning, transport. The skills handle what should remain flexible and high-level: enrichment, synthesis, prioritization, research flows, and interpretation. Garry states this principle directly: push intelligence up into skills, push execution down into deterministic tooling, and keep the harness thin.

Gbrain reinforces that separation in its knowledge model too. The README describes pages with compiled truth on top and an append-only timeline on the bottom. "The compiled truth is the answer. The timeline is the proof." I like this a lot because it shows that a thin harness is not the same thing as a dumb system. The substrate can be very strong. It can preserve provenance, versions, raw data, and structured history. But it does not try to replace the skill's job. The harness stores and retrieves. The skill interprets. The harness preserves evidence. The skill turns evidence into judgment.

That distinction has changed how I think about AI product management.

A lot of AI product conversations still revolve around front-end questions: Which model? What latency? Which tool protocol? How many integrations? Those are real questions, but they are often secondary. The more foundational question is: where does product intelligence live? If it lives inside an ever-thickening harness, the system becomes hard to maintain, hard to generalize, and hard to improve. If it lives in fat skills on top of a thin, deterministic substrate, then every model improvement can flow upward into a reusable layer of judgment without destabilizing the system underneath. That is exactly why Garry describes skills as permanent upgrades. Build them once, and the system compounds.

I think that is the biggest thing I learned from putting Garry's essay next to gbrain.

The future of AI products is not just better agents. It is better architecture for where intelligence belongs.

Not more wrapper. Better wrapper.
Not fatter harnesses. Fatter skills.
Not prompts scattered everywhere. Reusable judgment, codified.
Not a smarter chat box. A system that compounds.

That is why Thin Harness, Fat Skills is more than a catchy line. It is a design principle for AI products that want to get better over time rather than just look impressive in one session. Garry says the secret is not the model but the thing wrapping the model. Gbrain shows what that looks like when taken seriously: a thin infrastructure layer, a compounding memory loop, and fat markdown skills that teach the model how to think in a domain instead of merely what to say.

If you keep one sentence, make it this:

**The best AI products will be built with thin harnesses and fat skills.**
