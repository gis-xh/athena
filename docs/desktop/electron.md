---
tags: [桌面应用]
difficulty: 进阶
status: published
---

# Electron

[Electron应用打包教程：Electron-Packager, Electron-Builder, Electron-Forge和Electron-Vue-CSDN博客](https://blog.csdn.net/weixin_40629244/article/details/116429201)

截至目前最新版本 `26.15.2`

```sh
npm add electron-builder --save-dev
```

移除依赖，重新安装依赖

```sh
Remove-Item -Recurse -Force node_modules -ErrorAction SilentlyContinue
Remove-Item -Force package-lock.json -ErrorAction SilentlyContinue
npm install
```

```sh
npm run dist
```

若打包时一直出现无法打包的情况，可以手动下载。electron-builder 会缓存 Electron 到本地目录。

## 手动下载 Electron

1. **下载链接**（用浏览器打开）：

   ```
   https://npmmirror.com/mirrors/electron/42.4.0/electron-v42.4.0-win32-x64.zip
   ```

2. **将下载的 zip 文件放到**：

   ```
   C:\Users\<你的用户名>\AppData\Local\electron-builder\Cache\electron\electron-v42.4.0-win32-x64.zip
   ```

3. **重新打包**：
   ```bash
   npm run dist
   ```

沙盒化的预加载脚本（Sandboxed Preload Script）仍不支持使用 `import`，即 ESM

```sh
$env:ELECTRON_MIRROR="https://npmmirror.com/mirrors/electron/"
$env:ELECTRON_BUILDER_BINARIES_MIRROR="https://npmmirror.com/mirrors/electron-builder-binaries/"

# 清理旧文件并重新打包
Remove-Item -Recurse -Force dist\win-unpacked* -ErrorAction SilentlyContinue
Remove-Item -Recurse -Force dist\*.exe -ErrorAction SilentlyContinue
npm run dist
```

[[原创\]向Typora学习electron安全攻防-逆向工程-看雪安全社区｜专业技术交流与安全研究论坛](https://bbs.kanxue.com/thread-272604.htm)

[GitHub - toyobayashi/electron-asar-encrypt-demo: Hide JavaScript code in an Electron application. · GitHub](https://github.com/toyobayashi/electron-asar-encrypt-demo)