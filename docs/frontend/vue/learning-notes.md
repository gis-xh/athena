  

是否使用分号结尾，是否使用单引号，最后一段话是否以逗号结尾。

```
{
    "semi": false,
    "singleQuote": true,
    "trailingComma": "none"
}
```

在 `.eslintrc.js` 中的 `rules` 添加如下代码

```js
'indent': 0,
'space-before-function-paren': 0
```

> 安装 commitizen

```
cnpm i -g commitizen@4.2.4
```

> 安装 cz-customizable

```
cnpm i cz-customizable@6.3.0 --save-dev --legacy-peer-deps
```

## 4 使用 husky 强制 git 代码提交规范

强制性的 commit 规范，husky 是强制性的使用规范。

> 安装 commitlint

```
cnpm i --save-dev @commitlint/config-conventional@12.1.4 @commitlint/cli@12.1.4
```

> 安装 husky

```
cnpm i husky@7.0.1 --save-dev
```

> husky 初始化

```
npx husky install
```


## 安装 ElementPlus 库

### 1 安装

> yarn 安装

```
yarn add element-plus@1.3.0-beta.5
```

> cnpm 安装

```
cnpm i element-plus@1.3.0-beta.5
```

### 2 按需导入

```
cnpm install -D unplugin-vue-components unplugin-auto-import
```

5.在package.json中新增指令
```
"prepare": "husky install"
```

6.并执行

```
cnpm run prepare
```

7.新增husky配置文件 并往里面写入

```
npx husky add .husky/commit-msg
```

8.在配置中添加

```
npx --no-install commitlint --edit
```


## 安装 svg-sprite-loader

> cnpm 安装

```
cnpm i --save-dev svg-sprite-loader@6.0.9
```

> yarn 安装

```
yarn add --save-dev svg-sprite-loader@6.0.9
```


```
yarn add vue-i18n@next
```

> 安装全屏插件

```
yarn add screenfull@5.1.0
```

> 添加引导页插件

```
yarn add driver.js
```

> 添加时间解析插件

```
npm install dayjs --save
```