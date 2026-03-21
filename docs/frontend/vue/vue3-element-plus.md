# Vue3 setup 语法糖下的 Element-Plus 使用

## 1  i8n 国际化

官网：[国际化 | Element Plus](https://element-plus.gitee.io/zh-CN/guide/i18n.html)

Element Plus 组件 **默认** <font style="color:red;">使用英语</font>，如果要使用中文，需要做以下处理。

> main.js 中全局配置

```js
import ElementPlus from 'element-plus'
import zhCn from 'element-plus/es/locale/lang/zh-cn'
app.use(ElementPlus, {
  locale: zhCn,
})
```



## 2 Table 表格

### 2.1 插槽实现多级表头

```vue
<el-table border :data="waterData" :cell-style="cellStyle">
    <el-table-column v-for="(item, index) in waterOptions" :key="index" :prop="item.prop" :label="item.label" />
    <el-table-column v-for="(item, index) in wqParamOptions" :key="index" :prop="item.param_key" :label="item.param_name">
        <template #default v-if="item.prop === 'cl'">
			<el-table-column prop="cl" label="mg/L" />
        </template>
        <template #default v-else-if="item.prop === 'ph'">
			<el-table-column prop="ph" label="--" />
        </template>
        <template #default v-else-if="item.prop === 'tem'">
			<el-table-column prop="tem" label="℃" />
        </template>
        <template #default v-else-if="item.prop === 'opr'">
			<el-table-column prop="opr" label="Ntu" />
        </template>
        <template #default v-else-if="item.prop === 'tur'">
			<el-table-column prop="tur" label="us/cm" />
        </template>
    </el-table-column>
</el-table>
```

### 2.2 动态数据样式

参考文章：

1. [ElementPlus怎么修改表格行和单元格样式](https://www.yisu.com/zixun/695193.html)
2. [CSDN - element-ui动态更改el-table某个单元格字体颜色](https://blog.csdn.net/agua001/article/details/107960393)
3. [CSDN - 自定义element-ui的table字体颜色，及背景色](https://blog.csdn.net/Gordennizaicunzai/article/details/114752938)

> script setup 配置

```js
// 数据异常显示
const cellStyle = ({ row, column, rowIndex, columnIndex }) => {
  if (
    (column.label === "cl" && row.cl >= 4) ||
    (column.label === "cl" && row.cl < 0)
  ) {
    return { color: "#ff0000" };
  } else if (
    (column.label === "--" && row.ph >= 14) ||
    (column.label === "--" && row.ph <= 6)
  ) {
    return { color: "#ff0000" };
  } else if (
    (column.label === "℃" && row.tem >= 50) ||
    (column.label === "℃" && row.tem < 0)
  ) {
    return { color: "#ff0000" };
  } else if (
    (column.label === "Ntu" && row.opr >= 6) ||
    (column.label === "Ntu" && row.opr < 0)
  ) {
    return { color: "#ff0000" };
  } else if (
    (column.label === "us/cm" && row.tur >= 10) ||
    (column.label === "us/cm" && row.tur < 0)
  ) {
    return { color: "#ff0000" };
  }
};
```





## 3 Pagination 分页器

官网：[Pagination 分页 | Element Plus](https://element-plus.gitee.io/zh-CN/component/pagination.html)

参考文章：

1. [CSDN - 使用vue的分页实现自定义分页、多个slot的处理方案](https://blog.csdn.net/qq_42431872/article/details/106097979)
2. [vue（九）：elementUI分页插件的插槽slot的用法](https://blog.csdn.net/Fiona_lms/article/details/81368918)

由于 vue 的标签 el-pagination 当中的 layout 只能存在一个自定义 slot，所以当需要自定义多个 slot 时，就考虑用多个 el-pagination 拼接到一起。