---
title: "HiMCM 2023: Dandelion Spread PDE Model — Finalist Award"
date: 2023-11-01
author: "Forrest Chai"
description: "Finalist Award (Top ~7% globally) at HiMCM 2023. Constructed a system of four coupled PDEs modeling dandelion population dynamics with Fisher logistic growth, advection-diffusion, and Brownian dispersal."
categories: "math-modeling"
tags: ["Research"]
keywords: ["HiMCM", "PDE", "dandelion spread", "Fisher model", "advection-diffusion", "FEniCS"]
thesis_featured: true
featured_rank: 3
---

**Finalist Award — Top ~7% globally** at the 2023 High School Mathematical Contest in Modeling.

## The Model

We constructed a **Dandelion Spread PDE Model (DSM)** — a system of four coupled partial differential equations that model population densities of:

1. Settled dandelion seeds
2. Dandelion plants
3. Puffballs
4. Drifting seeds

We corrected the **Fisher model** by multiplying a logistic term to obtain a logistic population growth that depicts the effect of intraspecific competition on the dandelion population.

For the PDE of drifting seeds, we used the **advection-diffusion equation** by adding **Brownian Random Dispersal**. This allows us to effectively predict the spread of dandelions in various kinds of winds.

## Computation & Analysis

We used **FEniCS** to obtain the final predictions of the DSM model, followed by a **sensitivity analysis**. Finally, we used the **Analytic Hierarchy Process** to measure the dandelions' invasiveness, with total biomass calculated using the DSM model.

<a href="/research/himcm-13718.pdf" class="pdf-download" target="_blank">Download Full Paper (PDF)</a>
