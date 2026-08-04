---
tags: [AI, 大语言模型]
difficulty: 进阶
status: published
---

# 基于现有语言模型进行微调

> 本页介绍把通用大模型改造成“懂你的领域数据”的常见方法：从数据准备、全量微调到参数高效微调（LoRA、P-Tuning），并给出一个微信聊天记录微调的完整案例思路。

## 学习目标

- 理解微调与提示工程的区别；
- 掌握全量微调、LoRA、P-Tuning 三种方式的适用场景；
- 能完成“导出数据 → 预处理 → 构造训练样本 → 训练 → 评估”的完整流程。

## 前置要求

- 熟悉 Python 与基本的 JSON 处理；
- 了解 Transformer 结构与词元（token）概念；
- 有一块可用 GPU（或使用云平台）训练小模型。

## 1 微调是什么

预训练模型已经学会通用语言能力，微调是在**少量领域数据**上继续训练，让模型学会特定风格或知识：

- 学会模仿你的说话风格；
- 学会回答特定领域问题；
- 学会执行特定任务（如从聊天记录中提取信息）。

提示工程不改变模型参数，微调会改变参数。前者成本低、见效快；后者效果更稳定、可复用。

## 2 三种主流微调方式

### 2.1 全量微调（Full Fine-tuning）

所有参数都参与训练。

- 优点：效果上限高；
- 缺点：显存占用大、训练时间长、每个任务都要保存一份完整模型。

### 2.2 LoRA（低秩适配）

冻结原模型，只在每层旁路添加两个小矩阵（低秩分解）参与训练。

- 可训练参数通常只有 1% 左右；
- 训练完只保存一个小适配器文件；
- 可以同时维护多个任务适配器，随时切换。

### 2.3 P-Tuning / Prompt-Tuning

不修改模型权重（或只修改少量参数），而是学习一组**可训练的提示向量**插入输入序列。

- 适合数据量很少的场景；
- 对模型本身零改动，部署最简单；
- 效果依赖模型规模，小模型上不稳定。

## 3 数据准备：微信聊天记录案例

### 3.1 导出数据

使用 Wechat Chat History Exporter 等工具把聊天记录导出为文本或 JSON。

### 3.2 数据预处理

```python
import json

def clean_messages(raw):
    # 去掉时间戳、撤回提示、表情等噪音
    cleaned = []
    for msg in raw:
        text = msg.get("content", "").strip()
        if text and not text.startswith("["):
            cleaned.append({"role": "user" if msg["is_sender"] == 0 else "assistant",
                            "content": text})
    return cleaned

with open("export.json", encoding="utf-8") as f:
    data = json.load(f)
print(clean_messages(data)[:5])
```

### 3.3 构造训练样本

把连续的“你问我答”切分成对话对：

```python
def build_samples(messages, window=6):
    samples = []
    for i in range(len(messages) - 1):
        context = messages[max(0, i - window): i + 1]
        samples.append({"context": context[:-1], "target": context[-1]})
    return samples
```

注意隐私：正式训练前应脱敏（姓名、电话、地址替换为占位符）。

## 4 训练与评估

### 4.1 训练

推荐顺序：先用 LoRA 在小模型（如 1-7B 量级）上跑通，再逐步增大数据与模型。

```bash
# 示例：使用 transformers + peft
python train_lora.py --model chatglm3-6b --data train.jsonl --epochs 3
```

### 4.2 评估

- 准备一份训练时没见过的留出集；
- 分别评估：内容相关性、风格相似度、事实准确性；
- 与提示工程基线对比，确认微调确实带来了提升。

## 5 常见坑

1. 数据量太少（几千条以下）：先试提示工程或 P-Tuning；
2. 不脱敏直接训练：隐私风险；
3. 用模型没见过的语言/符号：词表外内容效果差；
4. 只看了训练损失不看验证集：过拟合无法发现；
5. 全量微调显存不足：换成 LoRA 或减少序列长度。

## 6 动手练习

1. 用你本人的聊天记录导出一份 200 条左右的对话数据，完成清洗与样本构造；
2. 用 LoRA 微调一个 1B 左右的模型，训练 1-2 个 epoch，对比微调前后的回复风格；
3. 设计一个 10 条问题的评估集，分别用“提示工程版”和“微调版”回答并打分。

## 参考

- LoRA 论文《LoRA: Low-Rank Adaptation of Large Language Models》；
- P-Tuning v2 论文（清华）；
- Hugging Face PEFT 文档：https://huggingface.co/docs/peft
