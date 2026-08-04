---
tags: [AI, Agents]
difficulty: 入门
status: published
---

# OpenClaw 及其插件安装配置说明 - 20260321 更新

## 一、不同系统的注意事项

### 1、Windows环境安装

- 全程使用管理员权限的 PowerShell 控制台
- 安装全程保持无魔法环境

### 2、WSL Ubuntu环境下安装

（1）Windows中的环境变量有概率会污染WSL内部环境，影响内部软件运行，最好隔离内外环境变量。

（2）在WSL Ubuntu中禁用Windows下的环境变量的方法：https://blog.csdn.net/weixin_43698781/article/details/124792708

## 二、安装软件

### 1、NVM

（1）用途：用于安装、管理和控制不同的 Node 版本的最佳实现。

（2）官方地址：

- [NVM 下载for Windows、Linux 和 MacOS - NVM中文网](https://www.nvmnode.com/zh/guide/download.html)
- [NVM 安装指南 - NVM中文网](https://www.nvmnode.com/zh/guide/installation.html)

### 2、Node.js

（1）用途：用于启动 OpenClaw 工具。

（2）安装：OpenClaw 最好使用 Node.js 22 进行安装。

- 设置镜像

```sh
# 设置 Node.js 镜像
nvm node_mirror https://npmmirror.com/mirrors/node/
# 设置 npm 镜像
nvm npm_mirror https://npmmirror.com/mirrors/npm/
```

- 查看当前支持的 Node 版本

```sh
nvm list available
```

- 安装 Node.js 22 最新 LTS 版本（截止至 2026.03.01）

```sh
nvm install 22.22.0
```

- 配置全局默认 Node.js 版本

```sh
nvm use 22.22.0
```

（3）其他 Node 配置

- 在`nvm` 根目录下新建 `node_cache` 和 `node_global` 两个文件夹
- 配置 `cache` 缓存路径和 `prefix` 全局安装路径

```sh
npm config set cache "D:\nvm\node_cache"
npm config set prefix "D:\nvm\node_global"
```

### 3、OpenClaw

（1）用途：AI生产工具，Windows桌面版仍然有很多bug，最好使用控制台版本。

（2）官方地址：[[OpenClaw | 能干活的 AI 助手](https://openclaws.io/zh/)](https://openclaws.io/zh/install)

- Windows 安装（使用 PowerShell）并已安装 Node.js >= 22

```sh
iwr -useb https://openclaw.ai/install.ps1 | iex
```

- Linux 一键安装

```sh
curl -sSL https://openclaw.ai/install.sh | bash
```

## 三、配置说明

### 1、购买并配置模型

（1）MiniMax Coding Plan：https://platform.minimaxi.com/docs/coding-plan

（2）阿里云 Coding Plan：https://help.aliyun.com/zh/model-studio/coding-plan

### 2、Skills配置

（1）Skills安装命令集合：https://skills.sh/

## 四、日常使用

1、入门学习-OpenClaw中文站：https://www.openclawdcn.com/

2、网络环境：安装时可以使用国内npm镜像，不需要使用魔法。日常使用国产模型时，最好关闭魔法工具再运行。

3、常用快捷键：。。。

# SealOS部署OpenClaw

环境变量中修改模型供应商：

```sh
OPENCLAW_PROVIDER_KIND=minimax-portal
```

添加挂载卷`/home/node/.npm`的大小，至少为 1G

## QQ机器人：[QQ开放平台｜机器人列表](https://q.qq.com/qqbot/openclaw/index.html)

添加 NPM 镜像源

```
npm config set registry https://registry.npmmirror.com
```

安装 OpenClaw 开源社区 QQBot 插件

```sh
openclaw plugins install @tencent-connect/openclaw-qqbot@latest
```

配置绑定当前QQ机器人

```sh
openclaw channels add --channel qqbot --token "AppID:AppSecretkey"
```

重启本地OpenClaw服务

```sh
openclaw gateway restart
```

ffmpeg：语音/视频格式转换，可用于 QQBot 识别语音与视频

```sh
sudo apt install ffmpeg
```

# OrangePi Zero3部署OpenClaw

```sh
openclaw configure
```