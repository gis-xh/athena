---
tags: [AI, 数据分析]
difficulty: 入门
status: published
---

# Python 网络爬虫入门

> 本页用 requests + BeautifulSoup 实现一个完整、合规的网页抓取示例，并讲解数据清洗与存储，是数据分析和数据科学的第一步。

## 学习目标

- 理解 HTTP 请求与响应、静态网页与动态网页的区别；
- 能用 requests 抓取网页并用 BeautifulSoup 解析 HTML；
- 能把抓取结果保存为 CSV，并遵守 robots 协议与版权约束。

## 前置要求

- Python 基础（循环、函数、列表）；
- 建议先看 [NumPy 入门](../numpy/index.md) 和 [Python Excel](../python-excel/01-excel.md)。

## 1 基础知识

爬虫本质是：**模拟浏览器发送 HTTP 请求，拿到 HTML 后解析出需要的信息**。

两个常用库：

- `requests`：发送请求、获取响应；
- `beautifulsoup4`：解析 HTML，提取标签和文本。

安装：

```bash
pip install requests beautifulsoup4
```

## 2 第一个爬虫

以下示例抓取一个公开示例站点的标题列表（请遵守目标站点的使用条款）。

```python
import requests
from bs4 import BeautifulSoup

url = "https://quotes.toscrape.com/"
resp = requests.get(url, timeout=10)
resp.raise_for_status()          # 请求失败时抛出异常

soup = BeautifulSoup(resp.text, "html.parser")
quotes = soup.select(".quote .text")
for q in quotes[:5]:
    print(q.get_text())
```

关键点：

- `resp.text` 是网页源码，编码可能不是 UTF-8，必要时用 `resp.encoding` 修正；
- `soup.select("CSS选择器")` 比 `find_all` 更直观，推荐先学 CSS 选择器；
- `timeout=10` 防止请求卡死。

## 3 解析数据并保存

抓取“名言 + 作者”并保存为 CSV：

```python
import csv
import requests
from bs4 import BeautifulSoup

resp = requests.get("https://quotes.toscrape.com/", timeout=10)
soup = BeautifulSoup(resp.text, "html.parser")

rows = []
for item in soup.select(".quote"):
    text = item.select_one(".text").get_text(strip=True)
    author = item.select_one(".author").get_text(strip=True)
    rows.append({"quote": text, "author": author})

with open("quotes.csv", "w", newline="", encoding="utf-8-sig") as f:
    writer = csv.DictWriter(f, fieldnames=["quote", "author"])
    writer.writeheader()
    writer.writerows(rows)

print(f"已保存 {len(rows)} 条数据")
```

`utf-8-sig` 编码能让 Excel 正确打开带中文的 CSV。

## 4 常见问题与解决

| 问题 | 表现 | 解决 |
| --- | --- | --- |
| 403 拒绝访问 | 服务端识别出爬虫 | 添加 `User-Agent` 请求头，降低频率 |
| 页面是动态渲染 | HTML 里没有目标数据 | 找 JSON 接口，或用浏览器自动化工具 |
| 编码乱码 | 中文变成乱码 | 检查 `resp.encoding`，手动指定 `utf-8` |
| 反爬封禁 | IP 被限制 | 放慢速度、设置重试；遵守对方规则 |
| 页面结构变化 | 选择器匹配不到 | 抓取时记录页面时间，定期检查 |

## 5 爬虫的道德与合规底线

- 查看目标网站的 `robots.txt`（如 `https://example.com/robots.txt`）；
- 控制请求频率（如每次请求间隔 1-2 秒）；
- 不爬取个人隐私数据，不绕过登录与验证码机制；
- 抓取内容用于学习时，注明来源，不用于商业牟利。

## 6 动手练习

1. 抓取 quotes.toscrape.com 的前 10 页名言，合并保存为一个 CSV；
2. 给请求加上 `User-Agent` 头，打印响应状态码对比加与不加的区别；
3. 从你常看的公开文档站抓取一个目录页，提取所有链接标题与 URL；
4. 写一个带 `time.sleep(1)` 和失败重试（最多 3 次）的通用抓取函数。

## 参考

- requests 文档：https://requests.readthedocs.io/
- BeautifulSoup 文档：https://www.crummy.com/software/BeautifulSoup/bs4/doc/
- 练习站点：https://quotes.toscrape.com/
