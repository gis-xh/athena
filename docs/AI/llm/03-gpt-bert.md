---
tags: [AI, 大语言模型]
difficulty: 入门
status: published
---

# GPT 与 BERT：预训练语言模型的两条路线

> 本页梳理 GPT 与 BERT 的核心思想：同样是“预训练 + 微调”，它们选择了不同的训练目标，也因此适合不同的任务。

## 学习目标

- 理解“预训练 + 微调”范式的含义；
- 能解释 GPT 的自回归目标与 BERT 的掩码语言模型目标；
- 知道什么任务适合 GPT、什么任务适合 BERT。

## 前置要求

- 已了解 Transformer 的基本结构（见[Transformer 笔记](./02-transformer.md)）；
- 了解词向量与自注意力的概念。

## 1 预训练 + 微调

先在海量无标注文本上训练一个通用语言模型（预训练），再在少量标注数据上针对具体任务调整（微调）。

这样做的好处是：**通用语言知识来自廉价的海量文本，任务知识只需要少量人工标注**。

## 2 GPT：自回归路线

GPT（Generative Pre-trained Transformer）使用 Transformer 的**解码器**结构，训练目标是**预测下一个词**：

$$
P(x_t | x_1, \dots, x_{t-1})
$$

这种“只能看左边”的注意力叫**掩码自注意力**。

特点：

- 天然适合**生成**任务：续写、对话、摘要；
- 只用单向上下文，但生成时不需要额外改造；
- ChatGPT 系列、GPT-4 都沿用这条路线。

## 3 BERT：自编码路线

BERT（Bidirectional Encoder Representations from Transformers）使用 Transformer 的**编码器**，训练目标是**掩码语言模型**：随机遮住 15% 的词，让模型根据左右两侧上下文猜出来。

$$
P(x_t | x_1, \dots, x_{t-1}, x_{t+1}, \dots, x_n)
$$

特点：

- 能同时利用左右上下文，理解能力更强；
- 适合**理解类**任务：文本分类、命名实体识别、问答、语义相似度；
- 不擅长自由生成，因为训练时没有学习“从左到右写句子”。

## 4 两条路线对比

| 维度 | GPT | BERT |
| --- | --- | --- |
| 基础结构 | Transformer 解码器 | Transformer 编码器 |
| 训练目标 | 预测下一个词（自回归） | 预测被掩码的词（自编码） |
| 上下文 | 单向 | 双向 |
| 擅长任务 | 文本生成 | 文本理解 |
| 典型模型 | GPT-2/3/4、ChatGLM | BERT、RoBERTa |

## 5 预训练任务之外：输入输出设计

微调时通常把任务改造成模型熟悉的格式：

- GPT 系：把分类任务写成“提示词 + 期望续写”；
- BERT 系：在输入前加 `[CLS]` 标记，用它的输出向量接一个分类头。

## 6 动手练习

1. 用 `transformers` 库加载一个 BERT 模型，对一句中文做文本分类（如情感分析）；
2. 用同一个库加载一个 GPT 模型做续写，比较两者在生成任务上的表现差异；
3. 思考：为什么 BERT 不适合做聊天机器人？如果要改造，需要加什么训练目标？

## 参考

- 《Attention Is All You Need》（Transformer 原始论文）；
- BERT 论文《BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding》；
- GPT 系列论文（OpenAI）。
