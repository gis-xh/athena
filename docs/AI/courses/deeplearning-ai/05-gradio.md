---
tags: [AI, 大语言模型, 课程]
difficulty: 进阶
status: published
---

# 用 Gradio 快速构建 AI 应用界面

> 本页是 DeepLearning.AI《Building Generative AI Applications with Gradio》的结构化笔记：用最少代码把模型封装成可交互的网页应用。

## 学习目标

- 能用 `gr.Interface` 在 5 行代码内发布一个模型 Demo；
- 理解输入组件、输出组件与 `fn` 回调的关系；
- 会使用 Blocks 搭建多组件、多 Tab 的自定义界面。

## 前置要求

- Python 基础；
- 有一个可调用的模型函数（如 Hugging Face 上的文本生成模型）。

## 1 Gradio 是什么

Gradio 是 Hugging Face 推出的 Python 库，用于快速构建机器学习模型的 Web 界面：

- 无需前端知识；
- 自动生成界面、输入输出组件与分享链接；
- 是给同事、客户演示模型的最快方式。

安装：

```bash
pip install gradio
```

## 2 最快上手：Interface

```python
import gradio as gr

def greet(name):
    return f"你好，{name}！"

demo = gr.Interface(fn=greet, inputs="text", outputs="text")
demo.launch()
```

运行后浏览器打开 `http://127.0.0.1:7860` 即可交互。

## 3 多输入输出

```python
import gradio as gr

def analyze(text, language):
    length = len(text)
    return {"字数": length, "语言": language}

demo = gr.Interface(
    fn=analyze,
    inputs=[gr.Textbox(label="输入文本"), gr.Dropdown(["中文", "English"], label="语言")],
    outputs=[gr.Number(label="字数"), gr.Textbox(label="语言")],
)
demo.launch()
```

输入输出都可以是多个，Gradio 会自动按顺序匹配。

## 4 用 Blocks 做自定义布局

需要更复杂界面时使用 Blocks：

```python
import gradio as gr

with gr.Blocks() as demo:
    gr.Markdown("# 翻译小助手")
    text = gr.Textbox(label="原文")
    button = gr.Button("翻译")
    output = gr.Textbox(label="译文")
    button.click(fn=lambda t: f"[译文] {t}", inputs=text, outputs=output)

demo.launch()
```

## 5 常见坑

1. `launch()` 默认绑定 127.0.0.1，只允许本机访问；需要公网演示时加 `share=True`（临时链接）；
2. 大模型推理较慢时，界面上没有进度反馈，可用 `gr.Progress` 或异步函数；
3. 输入输出类型不匹配（如把文本传给图像组件）会直接报错；
4. 中文路径或文件名可能导致组件资源加载失败，尽量使用英文路径。

## 6 动手练习

1. 把上一节的 FastAPI 例子改成 Gradio 界面，比较两种交付方式；
2. 用 Hugging Face 的 `pipeline("sentiment-analysis")` 做一个情感分析 Demo；
3. 给 `greet` 函数增加一个“语言选择”下拉框，实现中英文输出切换；
4. 用 Blocks 做一个包含输入、按钮、输出的“翻译助手”页面，并用 `gr.Examples` 添加示例输入。

## 参考

- DeepLearning.AI 原版课程：https://learn.deeplearning.ai/huggingface-gradio/
- Gradio 官方文档：https://www.gradio.app/
- B 站搬运视频：《使用 Gradio 构建生成式人工智能应用程序》
