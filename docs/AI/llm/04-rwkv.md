---
tags: [AI, 大语言模型]
difficulty: 入门
status: published
---

# RWKV：用 RNN 的方式做 LLM

> 本页介绍 RWKV 的核心思路：它把 Transformer 的注意力改造成线性递推形式，从而兼具 RNN 的低推理开销与 Transformer 的建模能力。

## 学习目标

- 理解 Transformer 推理开销大的原因；
- 理解 RWKV 如何用“状态 + 递推”替代注意力矩阵；
- 知道 RWKV 的适用场景与限制。

## 前置要求

- 熟悉 Transformer 的自注意力（见[Transformer 笔记](./02-transformer.md)）；
- 了解 RNN 的“按时间步递推”基本思想。

## 1 背景：Transformer 的问题

自注意力需要计算并存储 $O(T^2)$ 的注意力矩阵（$T$ 为序列长度），并且每个新 token 都要重新处理之前的全部内容。

后果：

- 长文本显存占用大；
- 推理速度随上下文增长而下降；
- 在资源受限的设备上部署困难。

## 2 RWKV 的核心思想

RWKV 由四个字母组成：

- R：Receptance（接收门），类似 RNN 的遗忘/接收机制；
- W：Weight（位置权重），决定历史信息按什么比例衰减；
- K：Key，当前 token 的“查询键”；
- V：Value，当前 token 携带的信息。

它把注意力的计算改写成**线性递推**：维护一个固定大小的状态，每读入一个新 token 更新一次状态，推理复杂度从 $O(T^2)$ 降到 $O(T)$，显存占用也变成常量。

关键区别：Transformer 的注意力是“当前 token 与所有历史 token 直接交互”，RWKV 是“历史信息先压缩成状态，再与当前 token 交互”。

## 3 RWKV 的特点

优点：

- 推理速度快、显存占用低；
- 支持超长上下文；
- 模型结构简单，容易在 CPU/嵌入式设备上运行；
- 开源社区活跃，有很多量化部署方案。

局限：

- 状态压缩会丢失部分历史细节，复杂长程依赖能力弱于完整注意力；
- 生态和工具链不如 Transformer 成熟；
- 顶尖效果仍由大规模 Transformer 模型保持。

## 4 与 Transformer 的对比

| 维度 | Transformer | RWKV |
| --- | --- | --- |
| 注意力 | 全量自注意力 $O(T^2)$ | 线性递推 $O(T)$ |
| 推理显存 | 随上下文增长 | 固定 |
| 长文本 | 优秀 | 良好（有信息压缩损失） |
| 部署友好度 | 一般 | 高 |
| 生态 | 最成熟 | 发展中 |

## 5 动手练习

1. 阅读 RWKV 的 README，用官方脚本下载一个小模型并在本地跑一次文本生成；
2. 用相同 prompt 分别让一个 GPT 风格模型和 RWKV 生成 200 字，比较两者的连贯性；
3. 思考：如果让你在树莓派上部署一个聊天机器人，你会选 Transformer 还是 RWKV？为什么？

## 参考

- RWKV 项目主页：https://www.rwkv.com
- RWKV GitHub：https://github.com/BlinkDL/RWKV-LM
- 知乎相关介绍：[RWKV 中文介绍](https://www.zhihu.com/question/602564718/answer/3042600470)
