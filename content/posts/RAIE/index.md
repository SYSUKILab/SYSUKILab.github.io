---
layout: "simple"
title: "RAIE: Region-Aware Incremental Preference Editing with LoRA for LLM-based Recommendation"
description: "A region-aware incremental editing framework that enables localized and stable adaptation of LLM-based recommenders under user preference drift."
date: 2026-04-07
tags: ["LLM-based Recommender Systems", "Knowledge Editing", "Preference Drift"]
categories: ["research", "publication"]
---

{{< katex >}}

We study the problem of **user preference drift** in LLM-based recommendation and propose **RAIE**, a region-aware incremental editing framework. Instead of global updates or instance-level edits, RAIE introduces **preference regions** as structured units for localized adaptation. This design enables efficient continual learning while preserving stable preferences.

<!--more-->

{{< alert "triangle-exclamation" >}}
**Core Problem**: Existing adaptation strategies either update the entire model (causing interference and forgetting) or operate at the instance level (failing to capture distributional shifts). There is a lack of **middle-level granularity** for stable and precise preference adaptation.
{{< /alert >}}

{{< button href="https://arxiv.org/abs/2603.00638" target="_blank" >}}
📄 Paper
{{< /button >}}
{{< button href="https://github.com/fengaogao/RAIE" target="_blank" >}}
📁 Code
{{< /button >}}

---

## Main Contributions

- **Region-based Preference Modeling**: Introduces *preference regions* as semantically coherent clusters of user behaviors.  
- **Region-aware Incremental Editing**: Designs three editing operations (Update / Expand / Add) to model different drift patterns.  
- **Region-specific LoRA Adaptation**: Assigns each region a dedicated LoRA adapter for localized parameter updates.  

---

## Method

### 1. Overview

RAIE decomposes preference adaptation into three stages:  
(1) knowledge region construction, (2) region-aware editing, and (3) routing-based inference.

{{< figure src="feature.png" alt="RAIE framework" caption="Figure 1. The overall architecture of our proposed RAIE. It consists of three core modules: (a) Knowledge Region Construction in the setup phase (S); (b) Region-aware Editing and LoRA Adaptation in the incremental learning finetune phase (F); and (c) Region-aware Routing in the inference test phase (T)." >}}

---

### 2. Knowledge Region Construction

User interaction sequences are segmented into subsequences and encoded by a frozen LLM. These representations are clustered via **spherical k-means** to form preference regions:

- each region has a center \( c_k \) and radius \( R_k \)  
- each region corresponds to a specific interest pattern  

---

### 3. Region-Aware Editing and Adaptation

For each incoming subsequence, RAIE:

1. computes similarity to existing regions  
2. selects an editing operation based on confidence  

- **Update**: refine region center (small drift)  
- **Expand**: enlarge region boundary (moderate drift)  
- **Add**: create a new region (emerging preference)  

Each region is associated with a **dedicated LoRA adapter**, trained only on its regional data.

---

### 4. Inference Routing

At inference time:

- map sequence → region  
- activate the corresponding LoRA adapter  

This enables **dynamic, context-aware adaptation** without modifying the backbone model.

---

## Experimental Setup

{{< alert "circle-info" >}}
**Setup**: We evaluate RAIE under a time-sliced continual learning protocol (Set-up → Finetune → Test), measuring both retention and adaptability.
{{< /alert >}}

- **Datasets**: MovieLens-10M, Yelp  
- **Backbones**: BERT4Rec, SASRec, TiSASRec, OpenP5  
- **Baselines**: LoRA, Replay, LwF, LSAT, MoLE, E-BPR  
- **Metrics**: Recall@10, NDCG@10  

---

## Representative Findings

- **Superior Adaptation**: RAIE achieves the best performance on future data (Test split).  
- **Strong Retention**: Maintains competitive performance on historical data (Set-up split).  
- **Balanced Learning**: Effectively resolves the stability–plasticity trade-off.  
- **Interpretable Adaptation**: Visualization shows that RAIE preserves global region structure while performing localized adjustments, leading to controlled and interpretable preference updates.

{{< figure src="result.png" alt="Main results" caption="Figure 2. RAIE consistently outperforms baselines across datasets and backbones." >}}
{{< figure src="visualization.png" alt="Region visualization" caption="Figure 3. Visualization of preference regions before and after editing, showing localized geometric adjustments with preserved global structure." >}}
{{< alert "lightbulb" >}}
**Key Insight**: Preference drift should be handled via **structured, localized editing**, rather than global parameter updates.
{{< /alert >}}

---

## Citation

**WWW 2026**

Jin Zeng, Yupeng Qi, Hui Li, Chengming Li, Ziyu Lyu, Lixin Cui, Lu Bai. 2026. RAIE: Region-Aware Incremental Preference Editing with LoRA for LLM-based Recommendation. arXiv preprint arXiv:2603.00638.

<details>
<summary>BibTeX</summary>

```bibtex
@article{zeng2026raie,
  title={RAIE: Region-Aware Incremental Preference Editing with LoRA for LLM-based Recommendation},
  author={Zeng, Jin and Qi, Yupeng and Li, Hui and Li, Chengming and Lyu, Ziyu and Cui, Lixin and Bai, Lu},
  journal={arXiv preprint arXiv:2603.00638},
  year={2026}
}
```

</details>
