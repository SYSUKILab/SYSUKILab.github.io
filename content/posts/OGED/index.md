---
title: "Beyond Known Event Types: Open-Domain Event Detection via Ontology-Guided Grounding–Discovery Resolution"
description: "We propose OGED, an ontology-guided framework for open-domain event detection with an evolving event type set. OGED organizes event types into a hierarchical ontology and keeps known-type grounding and new-type discovery jointly active during inference, achieving strong performance on both known-type detection and unknown-type discovery on a unified benchmark with 564 event types."
date: 2026-08-21
tags: ["Event Detection", "Information Extraction", "Large Language Models", "Ontology"]
categories: ["Findings", "EMNLP 2026"]
draft: false
showDate: true
showSummary: true
layout: "simple"
---

Real-world event detection systems rarely work with a complete list of event types — new kinds of events keep emerging. When an event type is missing from the predefined set, existing zero-shot detectors either miss the event entirely or force it into a plausible but wrong known type, silently hiding the need for discovery. We propose OGED, an ontology-guided framework that treats known-type assignment and new-type discovery as competing explanations and resolves them jointly before output. On a unified benchmark with 564 event types, OGED substantially improves both known-type detection and unknown-type discovery over strong baselines.
<!--more-->

{{< alert "triangle-exclamation" >}}
**Core Problem**: Existing zero-shot event detection methods assume a fixed, predefined type inventory. When the true event type is absent from this set, they either miss the event entirely or over-ground it to a plausible but wrong known type — both failures of deciding whether a mention should be assigned to an existing type or should induce a new one.
{{< /alert >}}

{{< button href="https://github.com/chenzhouli/OGED" target="_blank" >}}
📁 Code
{{< /button >}}

## Background

Open-domain event detection extracts event triggers and types from unstructured documents, supporting applications such as knowledge graph construction and information retrieval. Real systems operate with an *evolving* type set: in cyber threat analysis, for instance, resources like MITRE ATT&CK organize known adversary behaviors, yet incident reports keep introducing behaviors outside the current ontology. Existing paradigms sit at two extremes: zero-shot detection constrains outputs to a closed type set and misses or force-maps out-of-set events, while fully open-domain generation produces noisy, redundant free-form types misaligned with downstream labels. A practical framework should treat the seed type set as a semantic anchor while allowing principled expansion.

## Contributions

- **A practical task setting**: open-domain event detection with an *incomplete and evolving* type set, where the system must decide whether each mention grounds to an existing type or induces a new one.
- **The OGED framework**: an ontology-guided framework that keeps known-type grounding and new-type discovery jointly active within a unified inference process.
- **Comprehensive evaluation**: on a unified benchmark with 564 event types, OGED improves both known-type detection and unknown-type discovery over strong zero-shot baselines.

## Method

### 1. Overview

OGED organizes the seed event type set into a hierarchical ontology tree, where internal nodes are coarse semantic domains and leaf nodes are fine-grained event types with textual definitions. Given a document, it iteratively selects a promising semantic cluster, plans between detection and discovery, extracts and verifies candidates, updates the global state, and finally resolves conflicts among competing candidates.

{{< figure src="feature.png" alt="Overall framework of OGED" caption="Figure 1. OGED performs grounding–discovery resolution in five phases: (a) cluster-based semantic scheduling, (b) hybrid reasoning planning, (c) detection with retrieval-gated verification, (d) global state update, and (e) conflict selection." >}}

### 2. Cluster-based Semantic Scheduler and Hybrid Reasoning Planner

OGED retrieves the top-p relevant leaf types to build a document-specific search tree whose parent nodes form candidate semantic clusters. Each cluster carries a **semantic gap** score $G_k(c) = P_0(c)\cdot(1 - V_{k-1}(c))$ — high when the cluster is relevant to the document but insufficiently explained by current predictions. The scheduler picks the next cluster by balancing this gap with an exploration bonus, and the planner compares the gap against a threshold $\tau$ to decide whether the visit performs known-type **detection** or new-type **discovery**.

### 3. Retrieval-Gated Verification and Conflict Selection

When the detector proposes a new event type, a retrieval-gated verifier checks its embedding similarity against seed definitions: proposals distinctly far from existing types are accepted, while near-duplicates are re-examined by a judge LLM that either maps the mention back to an existing type or accepts the new one. Finally, an arbitration LLM resolves conflicts among candidates with overlapping trigger spans, allowing a newly induced type to replace a plausible but incorrect known-type label when supported by document evidence.

## Experimental Setup

{{< alert "circle-info" >}}
**Data / Models / Configuration**: To simulate an evolving environment, 30% of event types are masked as unknown and the seed ontology is built from the remaining 70%. For each document, the top 15 relevant event types are retrieved to build the dynamic search tree, and all results are averaged over three runs.
{{< /alert >}}

- **Datasets**: MAVEN, CASIE, FewEvent, MLEE, WikiEvents (unified by the SEOE benchmark, 564 event types)
- **Base Models**: Qwen3-30B-A3B-Instruct-2507 (main), Qwen3-8B (scale generalization), with Qwen3-Reranker-8B and Qwen3-Embedding-4B
- **Baselines**: GuidelineEE, Multi-event Staged, DiCoRe, ChatIE, Multi-event Direct (MD), MD-Open, GoLLIE-7B
- **Evaluation Metrics**: F1 for Trigger Identification (TI), Trigger Classification (TC), and Event Identification (EI), on both known and unknown events

## Results

- **Unknown event discovery**: Closed-set baselines score zero F1 on unknown events, while OGED improves average Unknown TI by +13.34 and Unknown EI by +31.63 F1 over the open-generation baseline MD-Open, effectively discovering new events with semantically appropriate types.

{{< figure src="main_result.png" alt="Main results on the open-domain event detection benchmark" caption="Figure 2. Main results across five datasets: OGED substantially outperforms all baselines on unknown-event discovery while achieving the best average Known TI and TC." >}}

- **Known event detection remains competitive**: OGED achieves the best average Known TI (40.54) and TC (20.78), surpassing the strongest closed-set baseline ChatIE — discovery capability does not come at the cost of known-type detection.
- **Larger gains under greater incompleteness**: As the masking ratio increases from 30% to 70%, OGED's overall TC advantage over ChatIE grows from +5.55 to +19.49 F1.

{{< figure src="case_study.png" alt="Case studies on CASIE" caption="Figure 3. Case studies on CASIE: for unseen event types, OGED induces appropriate new labels (In-App Phishing, Fake News Site Setup), while the baseline either misses the event or misroutes it to a wrong known type." >}}

{{< alert "lightbulb" >}}
**Key Takeaway**: When the event type inventory is incomplete and evolving, grounding and discovery should be kept jointly active rather than handled in separate stages. Organized by a hierarchical ontology and resolved through verification and conflict selection, a single framework can preserve accurate known-event detection while reliably discovering unseen event types.
{{< /alert >}}

## Citation

**Reference（Findings of EMNLP 2026）：**

Yuhan Hu, Yupeng Qi, Yongxi Luo, Chen Su, Shan Huang, Ziyu Lyu. 2026. Beyond Known Event Types: Open-Domain Event Detection via Ontology-Guided Grounding–Discovery Resolution. In Findings of the Association for Computational Linguistics: EMNLP 2026.

<details>
<summary>BibTeX</summary>

```bibtex
@inproceedings{hu2026beyond,
  title={Beyond Known Event Types: Open-Domain Event Detection via Ontology-Guided Grounding--Discovery Resolution},
  author={Hu, Yuhan and Qi, Yupeng and Luo, Yongxi and Su, Chen and Huang, Shan and Lyu, Ziyu},
  booktitle={Findings of the Association for Computational Linguistics: EMNLP 2026},
  year={2026}
}
```
</details>
