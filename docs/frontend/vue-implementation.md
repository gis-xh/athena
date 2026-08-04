---
tags: [前端]
difficulty: 进阶
status: published
---

# Vue TypeScript 前端项目初始化-20260729

## 相关参考

1. [Tailwind CSS 中文站| Install Tailwind CSS with Vite](https://www.tailwindcss.cn/docs/guides/vite)
2. [Tailwind CSS 官方站| Installation Using Vite](https://tailwindcss.com/docs/installation/using-vite)
3. [Github | tailwindcss 无法初始化 Discussion](https://github.com/tailwindlabs/tailwindcss/discussions/15820)
4. [Github | Cesium 官方 Vite 模板](https://github.com/CesiumGS/cesium-vite-example)

## 一、环境配置

### （一）编辑器配置

1、编辑器：VS Code / Trae

2、安装相关插件扩展

（1）vscode-icons：文件系统图标优化

（2）Code Spell Checker：拼写检查工具

（3）Vue：Vue 框架支持

（4）Tailwind CSS IntelliSense：Tailwind CSS样式可视化

（5）Prettier：代码格式化

（6）ESLint：代码检查

- 全局安装 ESLint

```sh
npm install -g eslint
```

### （二）项目初始化

初始化一个使用Vite、Vue、Tailwind CSS的前端项目底板。

1、创建 vite-vue-javascript 项目

（1）目的：使用 vite v7 版本直接创建 vue 模板项目

（2）官网：https://v7.vite.dev/guide/

（3）注意：由于 vite 最新版本 v8 出现了破坏性变更，对静态路径的读取发生改变，考虑到后续兼容性，选用 v7 版本。

- Vite 官方构建模板说明：https://github.com/vitejs/vite/tree/main/packages/create-vite#readme

```sh
npm create vite@7 frontend -- --template vue-ts
```

2、切换到项目目录

```sh
cd frontend
```

3、安装 @types/node

（1）目的：为 Node.js 提供 TypeScript 类型定义的包

（2）注意：需要同步安装与当前 node 对应的版本，当前以 node 22 版本为例

```sh
npm install @types/node@22
```

### （三）样式初始化

详细配置参考：https://v3.tailwindcss.com/docs/guides/vite#vue

1、安装 Tailwind CSS

（1）目的：将 Tailwind CSS 安装为 PostCSS 插件，将其与 webpack、Rollup、Vite 和 Parcel 等构建工具无缝集成。

（2）官网：https://v3.tailwindcss.com/docs/installation

（3）注意：由于 Tailwind CSS v4 出现了破坏性变更，不再使用 `init` 初始化，且不支持旧版浏览器，考虑到旧版兼容性，选用 v3 版本。

```sh
npm install -D tailwindcss@3 postcss autoprefixer
```

2、初始化样式配置文件

（1）自动创建 `tailwind.config.js` 与 `postcss.config.js` 文件。

```sh
npx tailwindcss init -p
```

3、配置项目CSS关联

（1）将 `tailwind.config.js` 内容修改为如下内容：

```js
/** @type {import('tailwindcss').Config} */
export default {
  content: ["./index.html", "./src/**/*.{vue,js,ts,jsx,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

（2）在 `./src/style.css` 中顶部添加如下内容：

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### （四）配置必备依赖

1、安装 Vite 核心配件

（1）目的：用于 `vite.config.ts` 内部配置

（2）逐个安装，`-D` 表示仅开发环境下使用

（3）注意：vite-plugin-static-copy v4 版本与 v3 版本存在较大差异，考虑到旧版本兼容性，选用 v3 版本。

```sh
npm install -D @vitejs/plugin-vue-jsx vite-plugin-static-copy@3.4.0 vite-plugin-compression
```

- `vitejs/plugin-vue-jsx`：用于对 vue 的 jsx 与 tsx 语言支持
- `vite-plugin-static-copy`：用于复制静态依赖
- `vite-plugin-compression`：用于开启打包时的代码压缩

2、安装 Vue Router

（1）目的：Vue 官方路由，页面切换或模块切换。

（2）官网：https://router.vuejs.org/zh/

```sh
npm install vue-router@4
```

（3）实际使用需要在 `src/` 目录下手动创建 `router/index.ts`

3、安装 Vue 状态管理库

安装 Vuex（可选）

（1）目的：Vue 官方状态管理模式库。

（2）官网：https://vuex.vuejs.org/zh/

安装 Pinia（推荐）

（1）目的：用于 Vue 的专属状态管理库，允许跨组件或页面共享状态。

（2）官网：https://pinia.vuejs.org/zh/

```sh
npm install pinia
```

4、安装 Axios

（1）目的：用于控制接口请求。

（2）官网：https://axios.rest/zh/

```sh
npm install axios
```

5、安装 Element-Plus 及其插件

（1）目的：当前主流 UI 组件库。

（2）官网：https://element-plus.org/zh-CN/

```sh
# 安装 Element-Plus 及其常用的图标 Icon 集合
npm install element-plus @element-plus/icons-vue

# 用于 element ui 的中国省市区级联数据
npm install element-china-area-data

# 用于 element ui 的元素大小检测高性能插件
npm install element-resize-detector
```

6、安装 ECharts

（1）目的：当前主流图表组件库。

（2）官网：[下载 - Apache ECharts](https://echarts.apache.org/zh/download.html)

（3）注意：由于目前很多第三方依赖包依赖的仍是 ECharts v5 版本，考虑到后续兼容性，选用 v5 版本。

```sh
npm install echarts@5
```

7、安装 exceljs

（1）目的：用于导出 xlsx 格式的 Excel 表格，支持设置表格内容样式。

（2）官网：[GitHub - exceljs/exceljs: Excel Workbook Manager · GitHub](https://github.com/exceljs/exceljs)

```sh
npm install exceljs
```

8、安装 vue-office

（1）目的：用于docx、xlsx、pdf文件预览

（2）官网：https://501351981.github.io/vue-office/examples/docs/

```sh
# docx文档预览组件
npm install @vue-office/docx vue-demi

# excel文档预览组件
npm install @vue-office/excel vue-demi

# pdf文档预览组件
npm install @vue-office/pdf vue-demi
```

### （五）构建其他库（可选）

1、安装 Cesium 及其插件

（1）目的：三维场景可视化库。

（2）官网：https://cesium.com/platform/cesiumjs/

```sh
npm install cesium

# 配合 cesium 使用的罗盘，导航仪（放大/缩小）和距离刻度组件
npm install cesium-navigation-es6
```

2、deepdark-ui：大屏 UI 组件库

（1）注意：deepdark-ui 依赖于 ECharts v5 版本。

```sh
npm install deepdark-ui
```

3、安装 DataV - Vue3

（1）目的：DataV Vue3+TS+Vite版，用于大屏展示的 UI 组件库。

（2）官网：https://datav-vue3.netlify.app/

```sh
npm install @kjgl77/datav-vue3
```

4、安装 Day.js

（1）目的：轻量级处理时间和日期的 JavaScript 库。

（2）官网：https://day.js.org/zh-CN/

```sh
npm install dayjs
```

## 二、基本配置

### （一）`vite.config.mts` 配置

将默认生成的`vite.config.ts`文件名改为`vite.config.mts`，修改后不会再被 `package.json` 中的 `"type"` 和 Node 版本影响。

```ts
import { fileURLToPath, URL } from "node:url";
import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";
import vueJsx from "@vitejs/plugin-vue-jsx";
import { viteStaticCopy } from "vite-plugin-static-copy";

const cesiumSource = "node_modules/cesium/Build/Cesium";
const cesiumBaseUrl = "cesium";

// https://vite.dev/config/
export default defineConfig({
  base: "./", // 项目根目录
  define: {
    // 定义 CESIUM_BASE_URL
    CESIUM_BASE_URL: JSON.stringify(cesiumBaseUrl),
  },
  resolve: {
    // 配置别名
    alias: {
      "@": fileURLToPath(new URL("./src", import.meta.url)),
      assets: fileURLToPath(new URL("./src/assets", import.meta.url)),
    },
    // vite需要手动添加对后缀名省略的支持, webpack不需要
    extensions: [".js", ".ts", ".jsx", ".tsx", ".vue", ".css", ".scss"],
  },
  optimizeDeps: {
    include: [
      "cesium",
      "echarts",
      "element-plus",
      "vue",
      "vue-router",
      "pinia",
      "@element-plus/icons-vue",
    ],
  },

  plugins: [
    vue(),
    vueJsx(),
    // CESIUM 静态资源复制
    viteStaticCopy({
      targets: [
        { src: `${cesiumSource}/ThirdParty`, dest: cesiumBaseUrl },
        { src: `${cesiumSource}/Workers`, dest: cesiumBaseUrl },
        { src: `${cesiumSource}/Assets`, dest: cesiumBaseUrl },
        { src: `${cesiumSource}/Widgets`, dest: cesiumBaseUrl },
      ],
    }),
  ],
  // 开发服务器配置
  server: {
    host: "0.0.0.0", // 允许外部访问
    port: 5713, // 端口号
    // 代理转发配置
    proxy: {
      // 后端代理
      "/api": {
        target: "http://127.0.0.1:8000/api/",
        changeOrigin: true,
        secure: false,
        ws: true,
        // 将请求路径中开头的 `/api` 替换为空字符串，即移除 `/api` 前缀
        rewrite: (path) => path.replace(/^\/api/, ""),
      },
    },
  },
});
```

### （二）`main.ts` 配置

为了将上面安装的库引入到代码中使用，需要在 `main.ts` 中的设置如下。

```ts
import { createApp } from "vue";
import { createPinia } from "pinia";
import router from "./router";
import ElementPlus from "element-plus";
import zhCn from "element-plus/es/locale/lang/zh-cn";
import * as ElementPlusIconsVue from "@element-plus/icons-vue";
import { ElTableColumn } from "element-plus";
import "element-plus/dist/index.css";
import "./style.css";
import App from "./App.vue";

// 导入Cesium相关资源
import * as Cesium from "cesium";
import "cesium/Source/Widgets/widgets.css";
// 导入deepdark-ui样式
import * as DeepdarkUi from "deepdark-ui";
import "deepdark-ui/deepdark-ui.css";

// 配置Cesium Ion Token
Cesium.Ion.defaultAccessToken =
  "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiIzZGJkM2ExNC1jMTYyLTQ2YjQtODBmMi04NzE4NGQ4MmI1NTQiLCJpZCI6MTAyNzI1LCJpYXQiOjE2NzczOTk3ODF9.OhHmzcF2vVFbfChAoNjWGTylgsRfVa8q9wYgS3nXQXU";

// 创建Vue应用实例
const app = createApp(App);

// 注册Element Plus 所有Icon图标
for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
  app.component(key, component);
}

// 引入Pinia
const pinia = createPinia();
// 使用Pinia
app.use(pinia);
// 使用Vue Router
app.use(router);
// 使用Element Plus, 并设置语言为中文, 表单组件默认尺寸大小为small, 弹窗组件层级z-index为3000
app.use(ElementPlus, {
  locale: zhCn,
  size: "small",
  zIndex: 3000,
});
// 全局配置表格列超出部分显示提示
ElTableColumn.props.showOverflowTooltip = {
  type: Function,
  default: () => true,
};

// 挂载应用
app.mount("#app");
```

### （三）Vite改造

vite无法使用require，需要替换

正则表达式检索替换：

- `require\((["'][^"']+["'])\)`替换为`new URL($1, import.meta.url)`
- `require\((["'][^"']+["'])\)`替换为`new URL($1, import.meta.url)`

在package.json中添加一行，强制使用ESM语法

```
  "type": "module",
```