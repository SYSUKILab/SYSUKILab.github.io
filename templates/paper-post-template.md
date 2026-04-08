<!--
使用方式：
1. 复制本文件到 /<paper-name>/index.md
2. 将配图放在同级目录，例如 workflow.png、results.png
3. 把所有 [占位内容] 替换成真实信息
4. 删除不需要的按钮、章节和注释
-->

---
layout: "simple"
title: "[论文标题]"
description: "[一句话摘要：解决什么问题、提出了什么方法、为什么重要。]"
date: 2026-04-07
tags: ["标签1", "标签2", "标签3"]
categories: ["research", "publication"]
draft: true
showDate: true
showSummary: true
---

[用 2-3 句话概括这篇论文。建议写清楚研究问题、核心方法和主要价值。]

{{< alert "triangle-exclamation" >}}
**核心问题**：[这篇论文要解决的关键瓶颈、研究空白或实际挑战是什么？]
{{< /alert >}}

<!-- 没有的按钮可以直接删掉 -->
{{< button href="[paper-link]" target="_blank" >}}
📄 论文
{{< /button >}}
{{< button href="[code-link]" target="_blank" >}}
📁 代码
{{< /button >}}
{{< button href="[dataset-or-demo-link]" target="_blank" >}}
🤗 数据集 / 演示
{{< /button >}}

## 研究背景

[简要介绍研究背景，并说明这个问题为什么重要。这里通常写 1-2 段即可。]

## 主要工作

- **[贡献点 1]**：[一句话描述。]
- **[贡献点 2]**：[一句话描述。]
- **[贡献点 3]**：[一句话描述。]

## 方法与系统

### 1. 整体框架

[用一小段介绍整体方法、系统流程或技术路线。]

{{< figure src="workflow.png" alt="[整体框架图]" caption="图 1. [用一句话说明这张核心框架图展示了什么。]" >}}

### 2. [模块 / 阶段 1]

[介绍第一个关键模块、阶段或技术点。]

### 3. [模块 / 阶段 2]

[介绍第二个关键模块、阶段或技术点。]

<!-- 如果论文更偏应用/系统，可以把上面两个小节改成 Data Pipeline / Interface / Deployment 等 -->

## 数据与实验设置

{{< alert "circle-info" >}}
**数据 / 模型 / 设置**：[用 1-2 句话概括最关键的实验设定。]
{{< /alert >}}

- **数据集**：[数据集名称与规模。]
- **基座模型 / 主干网络**：[模型或架构。]
- **对比方法**：[主要 baseline。]
- **评测设置**：[如数据划分、指标、temperature、硬件环境等。]

## 代表性结果

- **[结果 1]**：[最重要的实验结论。]
- **[结果 2]**：[有意思或出乎意料的观察。]
- **[结果 3]**：[实际启发、分析结论或消融发现。]

<!-- 没有结果图就删掉 -->
{{< figure src="results.png" alt="[主要结果图]" caption="图 2. [用一句话说明这张结果图或表格截图展示了什么。]" >}}

{{< alert "lightbulb" >}}
**核心结论**：[用一句话总结这篇论文最值得带走的结论。]
{{< /alert >}}

<!-- 系统/工具类论文可选：保留；纯算法论文可删掉 -->
## 使用方式

1. [步骤 1]
2. [步骤 2]
3. [步骤 3]

## 引用信息

**参考文献（[会议 / 期刊], [年份]）：**

[作者 1]，[作者 2]，[作者 3]。[年份]。[论文标题]。[会议 / 期刊 / Proceedings]。[DOI 或 URL]

<details>
<summary>BibTeX</summary>

```bibtex
[在这里粘贴 BibTeX]
```
</details>

