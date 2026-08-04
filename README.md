# Athena

## 介绍

&emsp;&emsp;这是 gis-xh 的**全栈技术学习笔记**，基于 MkDocs + Material 主题构建。内容涵盖 AI/机器学习、Python 后端、前端开发、Linux、DevOps 部署、IoT、3D 建模等多个技术领域。

## 内容架构

| 分类         | 路径              | 说明                                        |
| ------------ | ----------------- | ------------------------------------------- |
| 环境与工程化 | `docs/setup/`     | 基础应用、开发工具、容器环境、Git 工作流     |
| AI/算法      | `docs/AI/`        | 深度学习、大语言模型、数据分析、AI 应用     |
| Python       | `docs/python/`    | Django、FastAPI、GUI、数据分析              |
| 前端         | `docs/frontend/`  | Vue、React、Cesium、Web3D                   |
| Linux 与部署 | `docs/deploy/`    | Linux、Docker、阿里云、CentOS 部署          |
| 桌面应用     | `docs/desktop/`   | PyQt、Electron、Rust + Tauri                |
| 3D           | `docs/3D/`        | Blender 建模                                |
| GIS/遥感     | `docs/gis/`       | GEE、遥感影像处理与 GIS 开发                |
| 学术研究     | `docs/research/`  | 论文研读、文献分析与科研工具                |
| 物联网       | `docs/iot/`       | OrangePi、边缘计算                          |

## 技术栈

- **框架**: MkDocs + MkDocs Material
- **语言**: Python、JavaScript、Shell
- **部署**: GitHub Pages

## 本地运行

```bash
# 推荐使用 uv（依赖版本以 uv.lock 为准）
uv sync
uv run mkdocs serve

# 没有 uv 时，使用 pip 安装锁定版本的依赖
pip install -r requirements.txt
mkdocs serve

# 构建静态站点
mkdocs build
```
