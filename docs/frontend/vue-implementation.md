# Vue前端实现-20260311



## 相关参考

1. [Tailwind CSS 中文站| Install Tailwind CSS with Vite](https://www.tailwindcss.cn/docs/guides/vite)
3. [Tailwind CSS 官方站| Installation Using Vite](https://tailwindcss.com/docs/installation/using-vite)
4. [Github | tailwindcss 无法初始化 Discussion](https://github.com/tailwindlabs/tailwindcss/discussions/15820)
5. [Github | Cesium 官方 Vite 模板](https://github.com/CesiumGS/cesium-vite-example)



## 一、环境配置

### （一）编辑器配置

1、编辑器：VS Code / Trae

2、安装相关插件扩展

（1）vscode-icons：文件系统图标优化

（2）Code Spell Checker：拼写检查工具

（3）Vue：Vue 语言支持

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

```sh
npm create vite@latest frontend -- --template vue
```

2、切换到项目目录

```sh
cd frontend
```

3、安装 Tailwind CSS

（1）目的：将 Tailwind CSS 安装为 PostCSS 插件，将其与 webpack、Rollup、Vite 和 Parcel 等构建工具无缝集成。

（2）注意：由于 Tailwind CSS v4 出现了破坏性变更，不再使用 `init` 初始化，且不支持旧版浏览器，考虑到后续兼容性，选用 v3 版本。

```sh
npm install -D tailwindcss@3 postcss autoprefixer
```

4、初始化样式配置文件

（1）自动创建 `tailwind.config.js` 与 `postcss.config.js` 文件。

```sh
npx tailwindcss init -p
```

5、配置项目CSS关联

- 详细配置参考：https://v3.tailwindcss.com/docs/guides/vite#vue

### （三）配置必备依赖

1、安装 Vite 核心配件

- 用于 `vite.config.js` 内部配置
- 逐个安装，`-D` 表示仅开发环境下使用

```sh
npm install -D @vitejs/plugin-vue-jsx vite-plugin-html vite-plugin-static-copy
```

2、安装 Vue Router

（1）目的：Vue 官方路由，页面切换或模块切换。

（2）官网：https://router.vuejs.org/zh/

```sh
npm install vue-router@4
```

3、安装 Pinia

（1）目的：用于 Vue 的专属状态管理库，允许跨组件或页面共享状态。

（2）官网：https://pinia.vuejs.org/zh/

```sh
npm install pinia
```

4、安装 Axios

（1）目的：用于接口请求。

（2）官网：https://axios-http.com/zh/

```sh
npm install axios
```

5、安装 Element-Plus 及其常用的图标 Icon 集合

（1）目的：当前主流 UI 组件库

（2）官网：https://element-plus.org/zh-CN/

```sh
npm install element-plus
```

```sh
npm install @element-plus/icons-vue
```

6、安装 ECharts

（1）目的：当前主流图表组件库

（2）官网：https://echarts.apache.org/zh/download.html

（3）注意：由于很多依赖包依赖的是 ECharts v5 版本，考虑到后续兼容性，选用 v5 版本。

```sh
npm install echarts@5
```

### （四）构建其他库

1、安装 Cesium

```sh
npm install cesium
```

2、deepdark-ui：大屏 UI 组件库

（1）注意：deepdark-ui 依赖于 ECharts v5 版本。

```sh
npm install deepdark-ui
```

## 二、基本配置

### （一）`vite.config.js` 配置



### （二）`main.js` 配置

为了将上面安装的库引入到代码中使用，需要在 `main.js` 中的设置如下。

```js
import { createApp } from "vue";
import { createPinia } from "pinia";
import router from "./router";
import ElementPlus from "element-plus";
import * as ElementPlusIconsVue from "@element-plus/icons-vue";
import zhCn from "element-plus/es/locale/lang/zh-cn";
import "element-plus/dist/index.css";
import "./style.css";
import App from "./App.vue";

// 导入Cesium相关资源
import * as Cesium from "cesium";
import "cesium/Source/Widgets/widgets.css";
// 导入deepdark-ui样式
import * as DeepdarkUi from "deepdark-ui";
import "deepdark-ui/deepdark-ui.css";

// 设置Cesium Ion Token
Cesium.Ion.defaultAccessToken = "XXXXXX";

// 引入Pinia
const pinia = createPinia();
// 创建Vue应用实例
const app = createApp(App);

// 注册Element Plus 所有Icon图标
for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
  app.component(key, component);
}

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

// 挂载应用
app.mount("#app");
```

