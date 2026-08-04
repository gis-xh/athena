---
tags: [工程化, Python, AI]
difficulty: 进阶
status: published
---

# WOS 论文检索自动化分析软件开发日志

> 一个把“WOS 文献检索 → 数据清洗 → 图表生成 → 词云 → Word 报告”全流程自动化的 Python 项目。本页记录项目的开发环境、每日进度与当前成果，供后续接手与扩展参考。

## 项目目标

围绕 Web of Science 论文检索数据，实现从表格清洗、关键词分析到图表与 Word 报告生成的全自动流水线，并通过 AI 辅助完成同义词合并、翻译与词频统计。

## 技术栈（AI + Python 办公开发环境）

| 任务 | 格式 | 相关工具 |
| --- | --- | --- |
| Word 文档 | doc / docx | Python + Pandoc |
| Excel 表格 | xls / xlsx / csv | Pandas |
| 图表 | jpg / png | Matplotlib |
| 大模型连接 | - | openai 包（对接 DeepSeek API） |

## 开发过程

### Day 1：环境与项目启动

1. 安装并配置 Git、Miniconda；
2. 创建 `essay-auto-analysis` 所需的 Python 环境；
3. 使用 Git 获取代码并启动项目。

### Day 2：规则与数据分析

1. 尝试为 AI 制定通用性规则；
2. 进一步分析清洗后表格的各个列，生成合适的图表。

### Day 3：工程化与模型接入

1. 整理代码工程化结构：`.vscode/`、`.git/`、`.gitignore`、`README.md`、`main.py`；
2. 梳理最终文档组织结构；
3. 使用 openai 包连接 DeepSeek API。

### Day 4：词频与词云

1. 调用 AI 对关键词进行同义词合并，重新统计词频并完成中英翻译；
2. 重新生成词云图并更新 Word 文档；
3. 完善工程化：`.env` 配置、代码注释规范（Parameters + Returns）。

### Day 5：论文下载尝试

尝试使用智能体操作浏览器，根据论文 DOI 链接在局域网环境下下载论文原文。

## 当前进度

1. 使用 Python + Gradio 实现软件界面与完整论文分析业务逻辑；
2. 使用 PyInstaller 完成软件的 exe 打包。

## 下一步建议

- 将 DOI 自动下载模块接入正式流程；
- 增加异常处理、运行日志与进度提示；
- 扩展更多数据源（如知网、GEE）与报告模板。

## 参考

- [论文数据分析流程](../../research/paper-analysis-workflow.md)；
- [PyInstaller 打包](../../python/pyinstaller-setup.md)；
- [Gradio 入门](../../AI/courses/deeplearning-ai/05-gradio.md)。
