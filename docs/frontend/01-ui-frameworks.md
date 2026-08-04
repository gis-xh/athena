---
tags: [前端]
difficulty: 入门
status: published
---

# UI 样式

## 一、组件库与CSS框架

### （一）组件库

#### 1 大屏组件库

（1）[DataV-Vue2](http://datav.jiaminghi.com/)

（2）[DataV-Vue3](https://datav-vue3.netlify.app/)

（3）[deepdark-ui](https://www.npmjs.com/package/deepdark-ui)

- 说明文档：https://deepdarkui.com/index/comp/base/banner

#### 2 常用 UI 组件库

（1）[Element Plus](https://element-plus.org/zh-CN/)

- 2.10.0版本后，不再支持node18

（2）[Layui](https://layui.dev/)

### （二）CSS框架

1、[Bootstrap](https://www.getbootstrap.cn/)

2、[tailwindcss](https://www.tailwindcss.cn/)

## 二、CSS样式

### 毛玻璃

添加后背景有透明效果，blur值越小越透明

```css
backdrop-filter: blur(1px);
```

### 渐变色

`to left`：表示向左从0%到100%的渐变色

```css
background: linear-gradient(
  to left,
  rgba(0, 100, 200, 0.8) 0%,
  rgba(0, 150, 255, 0.6) 20%,
  rgba(0, 200, 255, 0.4) 40%,
  rgba(0, 212, 255, 0.2) 60%,
  rgba(0, 220, 255, 0.1) 80%,
  transparent 100%
);
```