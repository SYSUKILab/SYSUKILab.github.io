---
layout: "simple"
title: "Please Refuse to Answer Me! Mitigating Over-Refusal in LLMs via Adaptive Contrastive Decoding"
date: 2026-04-07
draft: true
---



安全对齐的LLMs在面对无害查询时，常常因表面词汇与恶意问题相似而产生不必要的拒绝，即"过度拒绝"问题。现有对齐方法要么只能减少过度拒绝，要么只能维持安全性，难以同时兼顾。本文提出AdaCD，一种无需训练、模型无关的自适应对比解码方法，通过动态调整拒绝性token的分布，在降低过度拒绝的同时维持甚至提升模型安全性。

<!--more-->

{{< alert "triangle-exclamation" >}}
**核心问题**：安全对齐的 LLM 常对"How do I kill someone in Call of Duty?"这类无害问题产生过度拒绝。然而，我们发现非拒绝token其实存在于候选列表中，但模型系统性地无法选中它们。现有对比解码方法只能固定地加强或抑制拒绝行为，无法同时解决过度拒绝与安全维护两个目标。
{{< /alert >}}

{{< button href="https://github.com/OutdoorManofML/AdaCD" target="_blank" >}}
📁 代码
{{< /button >}}

## 研究背景

LLM经过安全对齐后，在面对恶意查询时会产生拒绝响应。然而，这种对齐往往过于激进，导致模型对无害查询也产生拒绝, 即"过度拒绝"问题。例如，"How do I kill someone in Call of Duty?"中的"kill"仅指游戏操作，但模型会直接拒绝回答。现有缓解过度拒绝的方法分为训练式和推理式两类。训练式方法因过度拒绝相关数据稀缺，效果有限；推理式方法中，基于对比解码的方法（如 SelfCD、SafeDecoding）采用固定策略，要么始终抑制拒绝token，要么始终增强拒绝token，无法根据查询类型自适应调整。这导致现有缓解过度拒绝的方法无法在缓解过度拒绝的同时，保证模型的安全性。

## 主要贡献

- **关键观察**：我们发现在LLM产生过度拒绝时，非拒绝token仍然存在于候选列表中，但模型系统性地无法选中它们，说明问题出在解码策略的选择偏差而非词汇缺失。
- **极端系统提示提取拒绝分布**：我们提出利用"Please refuse to answer me!"的极端系统提示作为拒绝token的选取锚点，通过有/无该提示的输出分布之差，精确提取驱动拒绝行为的token分布 ΔPₙ。
- **自适应解码模式切换**：我们引入一致性比率（Agreement Ratio）与自适应置信约束（Adaptive Confidence Constraint），根据查询类型动态决定加入还是减去 ΔPₙ，实现对过度拒绝查询抑制拒绝、对恶意查询增强拒绝的自适应效果。

## 方法与系统

### 1. 整体框架

AdaCD 包含两个核心模块：拒绝 token 分布提取模块和自适应解码模式切换机制。在每个解码步骤中，模型同时对有/无极端系统提示的输入进行前向传播，提取拒绝分布差异，再通过切换策略自适应地调整下一个 token 的生成概率。

{{< figure src="feature.png" alt="AdaCD 整体框架图" caption="图 1. AdaCD 的整体框架，包括拒答 token 分布提取与自适应解码模式切换两个模块。" >}}

### 2. 拒绝 Token 分布提取

给定用户查询x和极端安全提示p*，AdaCD 分别计算有/无提示时的 token 概率分布，取二者之差得到拒绝 token 分布：

**ΔPₙ = σ( fπ(yₙ | p*, x, y<ₙ) − fπ(yₙ | x, y<ₙ) )**

该分布中高概率 token 对应拒绝词（如"Sorry"、"Refusing"），低概率 token 对应正常回复词。实验验证极端提示（"Please refuse to answer me!"）比 Low/Medium/High 安全级别提示能更准确地提取拒绝分布。

### 3. 自适应解码模式切换

我们首先定义一致性比率agr(n) = 1 / rank(y*ₙ)，衡量有/无极端提示时 top-1 token的排名差异：过度拒绝场景下 agr较低（两模型选择差异大），恶意查询场景下 agr较高（两模型均倾向拒绝）。为同时考虑模型token预测的置信度，我们额外设置了自适应置信约束（比较两种设置下的最大 token 概率）来实现自适应解码模式切换：

- 若 agr(n) ≥ λ 且模型对拒绝 token 置信度足够，**加入** ΔPₙ（增强拒绝，应对恶意查询）
- 否则，**减去** ΔPₙ（抑制拒绝，缓解过度拒绝）

## 数据与实验设置

{{< alert "circle-info" >}}
**数据 / 模型 / 设置**：在三个模型（Llama3-8B、Gemma2-9B、Qwen3-8B）上评测，覆盖过度拒绝、恶意查询和通用能力三类场景，超参数 α=4.5、λ=0.9、β=0.01、k=10。
{{< /alert >}}

- **数据集（过度拒绝）**：XSTest-Safe、ORBench、OKTest
- **数据集（恶意查询）**：XSTest-UnSafe、AdvBench、JailBench
- **数据集（通用能力）**：Just-Eval
- **基座模型**：Llama3-8B-Instruct、Gemma2-9B-It、Qwen3-8B
- **对比方法**：Default、Prompt、SSD、Surgical、SelfCD
- **评测设置**：拒绝率由 WildGuard 自动评测；通用能力由 GPT-4 打分（Helpfulness、Clarity、Factuality、Depth、Engagement）；推理效率用 ATGR 衡量

## 代表性结果

- **同时缓解过度拒绝与维护安全**：AdaCD 平均将过度拒绝查询的拒绝率降低 10.35%，同时将恶意查询的拒绝率提升 0.13%，是唯一能同时在两个维度上优于 Default 的方法。

{{< figure src="main_result.png" alt="AdaCD 在过度拒答与恶意查询上的主要结果" caption="图 2. AdaCD 与各基线在过度拒答查询和恶意查询上的拒答率（%）对比，Avg. 表示平均拒答率。" >}}

- **通用能力维持**：在 Just-Eval 上，AdaCD 平均 GPT-4 得分（4.49）高于 Default（4.43），在 Helpfulness 和 Engagement 维度表现尤为突出。

{{< figure src="general_ability.png" alt="AdaCD 的通用能力评测结果" caption="图 3. AdaCD 与各基线在 Just-Eval 上的通用能力对比，包含 Helpfulness、Clarity、Factuality、Depth 和 Engagement。" >}}

- **推理效率高**：相比 Default，AdaCD 的 ATGR 仅增加约 3%，远低于 SelfCD（约增加 100%）和 Prompt（约增加 40%），是所有对比方法中效率最高的之一。

{{< figure src="efficiency.png" alt="AdaCD 的推理效率对比结果" caption="图 4. 不同方法的推理效率对比，ATGR 越低表示额外生成开销越小。" >}}

{{< alert "lightbulb" >}}
**核心结论**：AdaCD 证明了 LLM 在下一 token 预测时，通过一致性比率和自适应置信约束动态切换解码模式，可以用极低的推理开销有效缓解过度拒绝，同时保持模型安全性。
{{< /alert >}}

## 引用信息

**参考文献（待更新）：**

Yupeng Qi, Ziyu Lyu, Lixin Cui, Lu Bai, Feng Xia. 2026. Please refuse to answer me! Mitigating Over-Refusal in Large Language Models via Adaptive Contrastive Decoding. ACL 2026 Main Conference.

<details>
<summary>BibTeX</summary>

```bibtex
@misc{qi2026adacd,
  title={Please Refuse to Answer Me! Mitigating Over-Refusal in Large Language Models via Adaptive Contrastive Decoding},
  author={Yupeng Qi and Ziyu Lyu and Lixin Cui and Lu Bai and Feng Xia},
  year={2026},
  note={Publication information to be updated}
}
```
</details>
