---
comments: false
tags: [AI, 大语言模型]
difficulty: 入门
status: published
---

# 大语言模型 (LLM)

大型语言模型原理、应用及 Agent 架构学习笔记。

## 目标

1. 熟悉主流 LLM 的运行流程
2. 找到配置友好且功能全面的模型方案
3. 掌握知识提取技术（LangChain 等）
4. 构建基于专业领域知识的问答系统

## 目录

- [01 预训练](./01-pretrain.md)
- [02 Transformer](./02-transformer.md)
- [03 GPT & BERT](./03-gpt-bert.md)
- [04 RWKV](./04-rwkv.md)
- [05 GLM](./05-glm.md)
- [06 知识图谱](./06-knowledge-graph.md)
- [07 LangChain](./07-langchain.md)
- [08 MCP](./08-mcp.md)
- [09 向量数据库](./09-vectorstore.md)
- [10 训练方法](./10-training-approach.md)
- [AI Agents](../agents/index.md)
- [Deeplearning.AI](../courses/deeplearning-ai/index.md)

## 核心概念

**Embedding**: 将离散的符号（单词、句子、文档）表示为连续向量的方法，使模型能够捕捉语义关系、处理多模态任务（图像、代码生成）。

**LLM (Large Language Model)**: 基于海量文本训练的大型神经网络，能处理文本生成、问答、对话、摘要等任务。通过预训练+微调或零样本方式实现下游应用。