---
layout: "simple"
title: "Please Refuse to Answer Me! Mitigating Over-Refusal in LLMs via Adaptive Contrastive Decoding"
description: "Safety-aligned LLMs frequently over-refuse harmless queries. AdaCD mitigates over-refusal via adaptive contrastive decoding, without any fine-tuning, while simultaneously mitigating the over-refusal and preserving model safety."
date: 2026-04-07
tags: ["Over-Refusal", "Contrastive Decoding", "LLM Safety"]
categories: ["Main Conference", "ACL 2026"]
draft: false
showDate: true
showSummary: true
---

Safety-aligned LLMs frequently generate refusal responses to harmless queries due to superficial lexical similarity with malicious ones — a phenomenon known as *over-refusal*. Existing approaches either reduce over-refusals or preserve safety, but rarely achieve both simultaneously. We propose AdaCD, a training-free and model-agnostic adaptive contrastive decoding method that dynamically adjusts the refusal token distribution to mitigate over-refusal while maintaining or even enhancing model safety.
<!--more-->

{{< alert "triangle-exclamation" >}}
**Core Problem**: Safety-aligned LLMs frequently over-refuse benign queries such as *"How do I kill someone in Call of Duty?"* We find that non-refusal tokens are in fact present in the candidate distribution, yet the model systematically fails to select them. Existing contrastive decoding methods apply a fixed strategy — either unconditionally suppressing or reinforcing refusal behavior — and are therefore unable to simultaneously address both over-refusal and safety preservation.
{{< /alert >}}

{{< button href="https://github.com/OutdoorManofML/AdaCD" target="_blank" >}}
📁 Code
{{< /button >}}

## Background

After safety alignment, LLMs are trained to refuse responses to malicious queries. However, this alignment is often overly aggressive, causing models to also refuse harmless queries — a problem known as *over-refusal*. For example, the query *"How do I kill someone in Call of Duty?"* is refused because "kill" superficially resembles harmful intent, despite referring to in-game actions. Existing mitigation methods fall into two categories: training-based and inference-based. Training-based methods are limited by the scarcity of over-refusal-specific data. Among inference-based methods, contrastive decoding approaches such as SelfCD and SafeDecoding apply a fixed strategy — either always suppressing or always reinforcing refusal tokens — and cannot adaptively adjust based on query type. As a result, no existing method can simultaneously mitigate over-refusal and guarantee model safety.

## Contributions

- **Key Observation**: We show that when LLMs over-refuse, non-refusal tokens remain present in the next-token candidate distribution, yet the model systematically fails to select them. This indicates that the problem stems from a selection bias in the decoding strategy rather than an absence of appropriate vocabulary.
- **Refusal Token Distribution Extraction via Extreme System Prompt**: We propose using the extreme system prompt *"Please refuse to answer me!"* as an anchor for extracting the refusal token distribution. By computing the difference between output distributions with and without this prompt, we precisely isolate the token distribution ΔPₙ that drives refusal behavior.
- **Adaptive Decoding Mode Switch**: We introduce the Agreement Ratio and an Adaptive Confidence Constraint to dynamically determine whether to add or subtract ΔPₙ based on query type, enabling context-sensitive control: suppressing refusal for over-refusal queries and reinforcing it for malicious ones.

## Method

### 1. Overview

AdaCD consists of two core components: a refusal token distribution extraction module and an adaptive decoding mode switch mechanism. At each decoding step, the model performs parallel forward passes with and without the extreme system prompt, extracts the difference in refusal token distributions, and then adaptively adjusts the next-token generation probability via the switching strategy.

{{< figure src="feature.png" alt="Overall Framework" caption="Figure 1. (a) Refusal Token Distribution Extraction: the refusal token distribution is extracted by contrasting outputs of the prompted and unprompted LLM under the extreme system prompt; (b) Adaptive Decoding Mode Switch: the agreement ratio and adaptive confidence constraint are used to dynamically adjust refusal token selection." >}}

### 2. Refusal Token Distribution Extraction

Given a user query x and the extreme safety prompt p*, AdaCD computes the token probability distributions with and without p* and takes their difference to obtain the refusal token distribution:

**ΔPₙ = σ( fπ(yₙ | p*, x, y<ₙ) − fπ(yₙ | x, y<ₙ) )**

Tokens with high probability in this distribution correspond to refusal-oriented outputs (e.g., "Sorry", "Refusing"), while low-probability tokens correspond to compliant responses. Experiments confirm that the extreme prompt ("Please refuse to answer me!") extracts refusal distributions more accurately than Low/Medium/High safety-level prompts.

### 3. Adaptive Decoding Mode Switch

We first define the Agreement Ratio agr(n) = 1 / rank(y*ₙ), which measures the rank of the top-1 token from the safety-prompted model within the unprompted token distribution. A low agr(n) indicates a large discrepancy between the two distributions — characteristic of over-refusal scenarios — while a high agr(n) indicates convergent refusal tendencies — characteristic of malicious queries. To further account for the model's confidence in token prediction, we additionally introduce an Adaptive Confidence Constraint (comparing the maximum token probabilities under both settings) to govern the decoding mode switch:

- If agr(n) ≥ λ and the model has sufficient confidence in refusal tokens → **Add** ΔPₙ (reinforce refusal; handle malicious queries)
- Otherwise → **Subtract** ΔPₙ (suppress refusal; mitigate over-refusal)

## Experimental Setup

{{< alert "circle-info" >}}
**Data / Models / Configuration**: Experiments are conducted on three models (Llama3-8B, Gemma2-9B, Qwen3-8B) across three evaluation scenarios covering over-refusal, malicious queries, and general usability. Hyperparameters are set to α=4.5, λ=0.9, β=0.01, and k=10.
{{< /alert >}}

- **Over-Refusal Datasets**: XSTest-Safe, ORBench, OKTest
- **Malicious Query Datasets**: XSTest-UnSafe, AdvBench, JailBench
- **General Usability Dataset**: Just-Eval
- **Base Models**: Llama3-8B-Instruct, Gemma2-9B-It, Qwen3-8B
- **Baselines**: Default, Prompt, SSD, Surgical, SelfCD
- **Evaluation**: Refusal ratio assessed by WildGuard; general usability scored by GPT-4 (Helpfulness, Clarity, Factuality, Depth, Engagement); inference efficiency measured by ATGR

## Results

- **Simultaneous Mitigation of Over-Refusal and Preservation of Safety**: AdaCD reduces the average refusal ratio on over-refusal queries by 10.35%, while simultaneously increasing the average refusal ratio on malicious queries by 0.13%. It is the only method that outperforms the Default baseline on both dimensions concurrently.

{{< figure src="main_result.png" alt="Main Results" caption="Figure 2. Refusal ratio (%) comparison across over-refusal and malicious queries. 'Avg.' denotes the mean refusal ratio across datasets." >}}

- **Preservation of General Usability**: On Just-Eval, AdaCD achieves a mean GPT-4 score of 4.49, surpassing the Default (4.43), with particularly notable gains on the Helpfulness and Engagement dimensions.

{{< figure src="general_ability.png" alt="General Usability Results" caption="Figure 3. GPT-4-based evaluation scores across five usability dimensions. AdaCD achieves the highest average score among all compared methods." >}}

- **Inference Efficiency**: Compared to Default, AdaCD incurs only a ~3% increase in average token generation time, substantially lower than SelfCD (~100% increase) and Prompt (~40% increase), making it one of the most computationally efficient methods among all evaluated baselines.

{{< figure src="efficiency.png" alt="Efficiency Comparison" caption="Figure 4. Inference efficiency comparison across methods." >}}

{{< alert "lightbulb" >}}
**Key Takeaway**: AdaCD demonstrates that, by dynamically switching decoding modes based on the agreement ratio and adaptive confidence constraint during next-token prediction, it is possible to effectively resolve the long-standing tension between over-refusal mitigation and safety preservation at minimal computational cost.
{{< /alert >}}

## Citation

**Reference (ACL 2026 Main Conference):**

Yupeng Qi, Ziyu Lyu, Lixin Cui, Lu Bai, Feng Xia. 2026. Please refuse to answer me! Mitigating Over-Refusal in Large Language Models via Adaptive Contrastive Decoding. ACL 2026 Main Conference.

<details>
<summary>BibTeX</summary>

</details>
