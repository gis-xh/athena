---
comments: false
tags: [部署]
difficulty: 入门
status: published
---

# 项目部署（DevOps）

> 让代码真正跑在服务器上：Linux 基础、Windows/Ubuntu/CentOS/阿里云部署、Docker 与常见踩坑记录。

## 适合谁 / 前置要求

- 已完成[环境配置](../setup/index.md)并至少熟悉一个 Web 框架；
- 建议先读 [Linux 学习](../deploy/linux/index.md) 再进入部署实战。

## 学习路径

1. [Linux 基础](linux/index.md)（入门）：目录结构、远程访问、用户与网络管理；
2. [Windows 部署](windows-deployment.md)（进阶）、[Ubuntu 部署](ubuntu-deployment.md)（进阶）、[CentOS 部署](centos-deployment.md)（进阶）；
3. [阿里云部署](aliyun-deployment.md)（进阶）与[踩坑记录](aliyun-deployment-issues.md)（进阶）；
4. [Docker GeoServer](docker-geoserver.md)（进阶）、[网络配置](network-setup.md)（入门）、[DevOps 学习](devops-learning.md)（进阶）。

## 能力目标

学完后你能独立把 Web 应用部署到云服务器（含域名、HTTPS、进程守护），并能根据踩坑记录快速定位常见部署问题。

## 配套练习 / 项目

- 在一台云服务器上完整部署一个 Django/FastAPI 应用；
- 用 supervisor 守护进程并配置开机自启；
- 用 Docker 启动 GeoServer 并挂载本地数据目录。

## 参考资源

- 容器环境配置见[环境配置](../setup/deploy/index.md)；
- 部署问题优先查[阿里云踩坑记录](aliyun-deployment-issues.md)。
