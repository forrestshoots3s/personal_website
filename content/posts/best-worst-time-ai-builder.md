---
title: "The Best and Worst Time to Be an AI Builder: Lessons I Already Learned Before Starting to Build My First Independent Product"
date: 2026-04-11
author: "Forrest Chai"
description: "Before building my first product, I spent a week trying to find a defensible position in AI-assisted patent work. I didn't find one. But I learned something more valuable about what it actually takes to compete."
categories: "indie"
tags: ["Product", "AI Builder", "Indie"]
keywords: ["AI builder", "product development", "patent agents", "indie product", "market research", "vertical specialization"]
thesis_featured: true
featured_rank: 1
---

There is a common narrative in the current AI moment: building has never been cheaper. With tools like Claude Code, a single developer can ship a working demo in a day. The barrier to entry has collapsed. Anyone with a laptop and an idea can produce something that looks like a product by sundown.

This is true. It is also the source of the problem. When everyone can build, everyone does. The same structural shift that makes it possible for a college freshman to prototype an AI agent in a weekend also means that every adjacent idea is being prototyped simultaneously by dozens of other people, many of whom have more capital, more domain expertise, and more runway. The best time to be an AI builder is also the worst.

This essay is about what I learned before writing a single line of product code — by spending a full week researching one vertical, patent agent products, and discovering that every niche I could think of was already occupied.

## 1. Two Ways an Undergraduate Can Compete

When I started thinking seriously about building an AI product, I identified two plausible strategies for someone without institutional advantages.

The first is **extreme vertical specialization**. General-purpose AI assistants like ChatGPT are broad but shallow across any given domain. A product that goes deep on one narrow workflow — say, drafting patent claims, or classifying tariff codes, or generating FDA pre-submission documents — can outperform a generalist by embedding domain-specific logic, retrieval, and verification into the system itself. The model is the same; the context engineering and harness design around it are what create differentiation.

The second is **superior interaction design**. Most AI products today still feel like chat interfaces bolted onto an API. If you can build something where the user experience itself is the moat — where the way a user interacts with the agent makes the work meaningfully faster, more transparent, or more controllable — that is a real advantage. This is less about model capability and more about product craft: the interface, the feedback loops, the way intermediate results are surfaced, the way the user stays in control.

I believed the first strategy was more immediately actionable. So I picked a vertical and went deep.

## 2. Why I Chose Patent Agents

Patent work seemed like an ideal vertical for AI. It is document-heavy, expensive, highly regulated, and dominated by legacy software. The workflows are well-defined: invention disclosure, prior art search, claim drafting, prosecution, maintenance, licensing, and infringement monitoring. Each stage involves structured reasoning over large corpora of text — exactly the kind of work language models are suited for.

I also had a proximity advantage. Northwestern's Innovation and New Ventures Office (INVO) manages the university's patent portfolio, and I had direct access to the people involved through The Garage, Northwestern's entrepreneurship hub. If I could build something useful for university technology transfer offices (TTOs), I could validate it locally before scaling.

So I started researching.

## 3. What I Found: Every Niche Was Taken

I began by mapping the full patent lifecycle and identifying where an AI agent could add the most value. Then I researched existing companies at each stage.

**Claim drafting and prosecution support.** This is the most obvious entry point — help patent attorneys write better claims faster. But this space is already crowded. Solve Intelligence, IP Copilot, and several other startups are well-funded and focused on exactly this. Y Combinator alone has backed multiple companies in this space, each with a slightly different angle.

**Prior art search and patent landscape analysis.** Another natural fit for AI. But Patlytics, Cypris, and others already offer AI-powered patent search with significant traction.

**Post-grant monitoring and infringement detection.** Surely this was less saturated? It was not. Multiple companies are building infringement monitoring products, several with enterprise clients already in place.

**Invention discovery** — proactively scanning research outputs to surface patentable innovations. This felt genuinely novel. The idea was to watch GitHub repositories, preprint servers, and lab publications to flag potentially patentable work before researchers even think to file a disclosure. But even here, I found competitors. FirstIgnite, for instance, had been doing adjacent work in research commercialization — and had recently been acquired by Inteum, the same company that provides Northwestern's inventor portal.

That acquisition was the final signal. I had identified the one niche I thought was underserved — university TTO automation, specifically the pre-submission invention disclosure phase — only to discover that the incumbent IP management platform Northwestern already uses had just acquired the closest competitor in that exact space. The market structure had already closed around the opportunity I thought I was the first to see.

## 4. The Structural Problem: Low Barriers, High Density

The lesson is not that patent agents are a bad idea. Several of the companies I researched are doing well. The lesson is about market structure under conditions of radically reduced building costs.

When it takes six months and a team of five to build a prototype, the number of people who will actually attempt any given idea is small. When it takes one person and one weekend, the number explodes. Every idea you can generate, someone else has also generated. Every niche you can identify, someone with deeper domain expertise has already identified and started building in.

This is the core tension of the current moment. AI has democratized building, but it has also democratized competition. The result is that finding a defensible position requires not just a good idea, but a structurally protected idea — one that depends on access, relationships, data, or domain knowledge that cannot be replicated by another builder with the same tools.

For an undergraduate, this is a hard bar to clear. I don't have ten years of patent law experience. I don't have proprietary datasets. I don't have enterprise sales relationships. What I do have is speed, technical fluency, and proximity to certain communities. But those advantages are shared by every other technically capable student with the same ambition.

## 5. What This Means for How I Search

After abandoning patent agents, I started researching other verticals — cross-border trade compliance, insurance document processing, financial KYC workflows. The pattern repeated: each vertical had clear AI opportunities, and each was already being addressed by funded startups.

But the research itself taught me something about methodology. Every time I investigated a vertical without fully understanding the business logic, I wasted time on ideas that were obviously flawed to anyone with domain experience. The only way to find a genuinely underserved niche is to understand the workflow deeply enough to see the gaps that outsiders miss. And that depth of understanding usually comes from one of two sources: years of professional experience in the domain, or sustained research engagement with the problem.

This is why so many successful AI startup founders come from the industries they're disrupting. Their ideas aren't better in the abstract — they're better because they emerged from direct contact with the friction. A former patent examiner sees different opportunities than a computer science student reading about patent law for the first time.

For me, this points toward a practical conclusion. Rather than continuing to survey verticals from the outside, I should be looking for opportunities to go deep — whether through applied research, part-time work, or close collaboration with domain experts. The niche ideas I'm looking for are unlikely to come from market research alone. They're more likely to come from being embedded in a problem long enough to see what everyone else has overlooked.

## 6. The Two-Strategy Framework, Revisited

I started this process believing that extreme vertical specialization was the more promising path. I still think it's correct in principle, but I now understand the prerequisite better: you need domain depth before you can specialize effectively. Without it, you're guessing at niches rather than discovering them.

The second strategy — competing on interaction design and user experience — may actually be more immediately accessible to someone with my profile. Building a product where the interface itself is the differentiator doesn't require ten years of domain expertise. It requires taste, technical skill, and a willingness to obsess over how humans and AI systems collaborate in practice. That is something I can start doing now, across any vertical.

In reality, the strongest position probably combines both: deep enough domain knowledge to identify a real gap, and strong enough product design to make the solution feel inevitable. But the order matters. Right now, I'm building the product instinct. The domain depth will come from research, from collaboration, and from time.

## 7. Conclusion

I spent a week trying to find a defensible niche in patent agent products. I mapped the full lifecycle, researched every competitor I could find, and stress-tested every idea I generated. None survived contact with the market.

That week was not wasted. It taught me that in the current AI landscape, the hard part is not building — it is finding something worth building that no one else has already started. It taught me that niche discovery requires domain immersion, not just market scanning. And it taught me that the democratization of AI development cuts in both directions: it empowers individual builders, but it also floods every visible opportunity with competition.

For anyone in a similar position — technically capable, ambitious, but lacking deep domain expertise — I think the honest advice is this: don't start by looking for a product to build. Start by finding a domain to understand. The product ideas will follow, and they'll be better for having emerged from genuine insight rather than surface-level research.

The best time to be an AI builder is right now. But only if you know where to dig.
