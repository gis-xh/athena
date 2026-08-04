---
tags: [学术]
difficulty: 进阶
status: published
---

# LaTeX 文档撰写

参考：

1. [LaTeX：从入门到日常使用](https://dylandong.top/posts/e480/)
2. [【LaTeX】新手教程：从入门到日常使用](https://zhuanlan.zhihu.com/p/456055339)
3. [【LaTeX】自用简洁模板（一）：中英作业](https://zhuanlan.zhihu.com/p/379312980)

模板网站：

- <https://www.latextemplates.com/cat/articles>
- [Overleaf](https://www.overleaf.com/)

## 设置基本参数

### 文档类型

```tex
\documentclass[12pt, a4paper, oneside]{ctexart}
```

- 字号：小四；纸张大小：A4；单面打印
- `ctexart`：适用于中文为主的文档，内容自动首行缩进，底层调用 ctex 宏包

### 加载基本宏包

```tex
\usepackage{amsmath, amsthm, amssymb, graphicx}
\usepackage[bookmarks=true, colorlinks, citecolor=blue, linkcolor=black]{hyperref}
```

- 数学公式包、插入图片包
- 引用超链接

### 页面设置

```tex
\usepackage{geometry}
\geometry{left=3.0cm,right=3.0cm,top=3.0cm,bottom=2.5cm}
```

- 设置页边距

### 页眉页脚

```tex
\pagenumbering{roman}
```

### 版面设置

```tex
\linespread{1.25} %1.25 倍行间距
```

## 基础语法

### 插入空行

```tex
\vspace{2cm} %插入一个高度为2厘米的空行
\vspace*{3cm} %插入一个高度为3厘米的空行
```

- `\vspace*` 命令可以在页面顶部或底部插入空行，而 `\vspace` 命令则不会。

### 字号

LaTeX 中的行距与字号直接相关，基本行距是文字大小的 1.2 倍。可以使用 `\fontsize` 命令设置字号和行距，再使用 `\selectfont` 命令应用设置。

```tex
\fontsize{16pt}{19.2pt}\selectfont %设置字体大小为16pt，行距为19.2pt
```

[px, pt, em 换算表 | 菜鸟教程](https://www.runoob.com/w3cnote/px-pt-em-convert-table.html)

```tex
\fontsize{16pt}{18pt}\selectfont %设置字体大小为3号，行距为18pt
\normalsize %恢复正常字体大小
```

### 空格

在 LaTeX 中显示空格，可以使用 `\` 命令表示一个空格，或使用 `\hspace` 命令指定空格宽度。

```tex
{学\hspace{1em}号： 2022710468}\par
```