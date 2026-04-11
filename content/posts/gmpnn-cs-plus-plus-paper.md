---
title: "GMPNN-CS++: A Novel Dual-Contrasting Framework for Drug-Drug Interaction Prediction"
date: 2024-12-01
author: "Forrest Chai"
description: "The proposed GMPNN-CS++ model introduces a dual-contrasting sampling approach and self-attention mechanism for DDI prediction, achieving 97% overall accuracy."
categories: "gmpnn"
tags: ["Research"]
keywords: ["GMPNN-CS++", "drug-drug interaction", "gated message passing neural network", "contrastive learning", "self-attention"]
thesis_featured: true
featured_rank: 1
---

Drug-drug interactions (DDIs) occur when multiple drugs react with each other when taken together. They can lead to unintended side effects that may be harmful to patients. Developing an efficient and accurate computational model for DDI predictions is highly important to assist healthcare professionals in making better prescription decisions.

## Model Architecture

The proposed GMPNN-CS++ model employs the **Self-attention mechanism** and a **residual memory network** after GMPNN-CS's message-passing module to enhance the extracted representation of cross-substructure pairs within two interacting drug molecules.

## Novel Dual-Contrasting Approach

Previous neural network models for DDI prediction mainly consider two types of samples: positive samples and negative non-interaction samples. I introduce a novel dual-contrasting sampling approach in GMPNN-CS++ to include a third type — **negative mislabeled-interaction samples** — representing an overlooked common negative scenario.

A dual-contrasting loss function is designed to make the neural network distinguish between positive samples and both types of negative samples, thereby widening the model's applicability.

## Results

Through dual-contrasting, GMPNN-CS++ demonstrates an ability to capture additional features and improves performance in predicting both positive and negative DDI cases, achieving an **overall accuracy of 97%**, compared to the baseline GMPNN-CS.

## Key Contributions

- Novel dual-contrasting framework with three types of samples
- Self-attention module for enhanced cross-substructure representation
- Residual memory network for improved feature extraction
- 97% overall accuracy in DDI prediction

## Recognition

This research led to selection as a **Regeneron Science Talent Search Scholar '25**, the nation's oldest and most prestigious science and math competition for high school seniors ($2,000 scholar award and $2,000 school award).

<a href="/research/gmpnn-cs-plus-plus.pdf" class="pdf-download" target="_blank">Download Full Paper (PDF)</a>
