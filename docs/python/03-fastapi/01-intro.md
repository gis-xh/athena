---
tags: [Python]
difficulty: 入门
status: published
---

# FastAPI 入门

> 本页带你从零搭建第一个 FastAPI 应用：路由、路径参数、请求体与自动文档，并解释为什么 FastAPI 适合作为 Python 后端的首选框架之一。

## 学习目标

- 能独立创建并运行一个 FastAPI 项目；
- 理解路由、路径参数、查询参数与请求体的用法；
- 知道 FastAPI 自动生成的交互式文档在哪里。

## 前置要求

- Python 3.8+；
- 了解函数与类型注解（FastAPI 大量依赖类型注解）。

## 1 FastAPI 是什么

FastAPI 是一个现代 Python Web 框架：

- 基于 Starlette 与 Pydantic；
- 自动生成 OpenAPI 文档（`/docs`）；
- 自带数据校验与类型提示，开发体验好；
- 异步支持，性能接近 Node.js/Go。

安装：

```bash
pip install fastapi "uvicorn[standard]"
```

## 2 第一个应用

创建 `main.py`：

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello World"}

@app.get("/items/{item_id}")
def read_item(item_id: int, q: str | None = None):
    return {"item_id": item_id, "q": q}
```

启动：

```bash
uvicorn main:app --reload
```

然后访问：

- `http://127.0.0.1:8000/`：首页 JSON；
- `http://127.0.0.1:8000/docs`：Swagger 交互式文档；
- `http://127.0.0.1:8000/items/42?q=hello`：带参数的路由。

## 3 核心概念

### 3.1 路径参数与类型校验

`item_id: int` 声明了参数类型，访问 `/items/abc` 会得到 422 校验错误而不是程序崩溃。

### 3.2 查询参数

`q: str | None = None` 表示可选查询参数，不传时为 `None`。

### 3.3 请求体

```python
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    price: float
    is_offer: bool | None = None

@app.post("/items/")
def create_item(item: Item):
    return {"name": item.name, "price": item.price}
```

Pydantic 会自动校验字段类型并返回友好的错误信息。

## 4 常见坑

1. 路由顺序：`/items/{item_id}` 与 `/items/me` 同时存在时，固定路径要写在前面；
2. 忘记 `--reload`：修改代码不生效；
3. 返回非 JSON 可序列化对象：报错，统一返回字典或 Pydantic 模型；
4. 中文乱码：响应头没有 `charset`，FastAPI 默认返回 UTF-8，一般不会乱码；乱码通常出在数据库或终端显示层。

## 5 动手练习

1. 添加一个 `/sum?a=1&b=2` 路由，返回两个数的和；
2. 定义一个 `Todo` 模型（标题、完成状态），实现 `POST /todos` 与 `GET /todos`（先用内存列表存储）；
3. 写一个健康检查路由 `/health`，返回 `{"status": "ok"}`；
4. 尝试用 `curl` 发送一次 POST 请求，观察 422 与 200 的响应差异。

## 参考

- FastAPI 官方文档：https://fastapi.tiangolo.com/
- 本仓库 Django 教程：[Django 入门](../01-django/index.md)
