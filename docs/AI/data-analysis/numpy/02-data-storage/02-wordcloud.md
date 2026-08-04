---
tags: [AI, 数据分析]
difficulty: 入门
status: published
---

# 0202 WordCloud 词云统计

## 1 词云图

### 1.1 WordCloud 简介

&emsp;&emsp;词云图，也叫文字云是对文本中出现频率较高的 “关键词” 予以视觉化的展现，词云图过滤掉大量的低频低质的文本信息，使得浏览者只要一眼扫过文本就可领略文本的主旨。

### 1.2 WordCloud 包

&emsp;&emsp;WordCloud 是一款 python 环境下的词云图工具包，同时支持 `python2` 和 `python3`，能通过代码的形式把关键词数据转换成直观且有趣的图文模式。

## 2 安装

&emsp;&emsp;使用以下任意命令即可进行 WordCloud 包的安装。

> pip 默认安装方式

```sh
pip install wordcloud
```

> conda 安装

```sh
conda install -c conda-forge wordcloud
```

## 3 编程实现

> 引入 wordcloud 包

```python
from wordcloud import wordcloud
```

> 定义词云函数

```python
def myWordCloud(filename):
    with open(filename, encoding="utf-8") as file:

```

上一节：[0201 NumPy 数据存取](./01-storage.md)

下一节：[0203 NumPy 常用函数](./03-functions.md)