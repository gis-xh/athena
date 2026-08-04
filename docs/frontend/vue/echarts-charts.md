# ECharts 图表汇总

- 内容基于 Vue 3.2 `<script setup>` 语法糖

## 一、基础模板

```vue
<template>
  <div ref="chartRef" style="height: 300px;"></div>
</template>

<script setup>
import * as echarts from "echarts";
import { ref, onMounted } from "vue";
// 1.为DOM元素创建一个ref
const chartRef = ref(null);
// 2.定义echarts实例
const myEcharts = ref(null);
// 3.准备数据
const data = [];
// 4.定义配置
const options = {};
// 5.初始化echarts实例方法
const initEcharts = () => {
  // 获取DOM元素
  const el = chartRef.value;
  // 创建echarts实例
  myEcharts.value = echarts.init(el);
  // 设置图表的选项
  myEcharts.value.setOption(options);
  // 图表大小自适应窗口大小变化
  window.onresize = () => {
    myEcharts.resize();
  };
};
// 6.在onMounted钩子函数上初始化echarts实例
onMounted(() => {
  initEcharts();
});
</script>

<style scoped lang="scss"></style>
```

## 二、饼状图

### 1 南丁格尔玫瑰图

官网示例：<https://echarts.apache.org/examples/zh/editor.html?c=pie-roseType-simple>

```vue
<template>
  <!-- 玫瑰图 -->
  <div id="rosechart" ref="chartRef"></div>
</template>

<script setup>
...
// 3.准备数据
const data = [
  { value: 40, name: "种类 1" },
  { value: 38, name: "种类 2" },
  { value: 32, name: "种类 3" },
  { value: 30, name: "种类 4" },
  { value: 28, name: "种类 5" },
  { value: 26, name: "种类 6" },
];
// 4.定义配置
const options = {
  legend: {
    // top: "bottom",
    bottom: 10,
  },
  toolbox: {
    show: true,
    feature: {
      mark: { show: true },
      dataView: { show: true, readOnly: false },
      restore: { show: true },
      saveAsImage: { show: true },
    },
  },
  series: [
    {
      name: "Nightingale Chart",
      type: "pie",
      radius: [20, 100],
      center: ["50%", "50%"],
      roseType: "area",
      itemStyle: {
        borderRadius: 8,
      },
      data,
    },
  ],
};
...
</script>

<style scoped lang="scss">
#rosechart {
  box-sizing: border-box;
  height: 100%;
  width: 80%;
  margin: 0 auto;
}
</style>
```

## 三、仪表盘

（待补充）

## 四、前后端交互

如果想要读取后端的数据进行显示，可以使用 JavaScript 的 Fetch API 来发送 HTTP 请求。Fetch API 是一个内置的 JavaScript 接口，可以用来处理 HTTP 请求，如 GET（获取数据）、POST（发送数据）、PUT（更新数据）和 DELETE（删除数据）等。

例如，从假的 REST API（<https://jsonplaceholder.typicode.com/posts>）获取文章数据并显示在饼图中：

```vue
<template>
  <div ref="chartRef" style="height: 300px;"></div>
</template>

<script setup>
import * as echarts from "echarts";
import { onMounted, ref } from "vue";

// create a ref for the DOM element
const chartRef = ref(null);
// create a ref for the echarts instance
const chartInstance = ref(null);

// create a ref for the posts data
const posts = ref([]);

// fetch the posts data on mounted hook
onMounted(async () => {
  // get the DOM element
  const el = chartRef.value;
  // create the echarts instance
  chartInstance.value = echarts.init(el);
  // fetch the posts data from the fake REST API
  const response = await fetch("https://jsonplaceholder.typicode.com/posts");
  // convert the response to JSON format
  const data = await response.json();
  // assign the data to the posts ref
  posts.value = data;
  // set the options for the chart
  chartInstance.value.setOption(options);
});

// define options
const options = {
  title: {
    text: "Posts by User ID",
    left: "center",
  },
  tooltip: {
    trigger: "item",
  },
  legend: {
    orient: "vertical",
    left: "left",
  },
  series: [
    {
      type: "pie",
      radius: "50%",
      // use a computed property to transform the posts data to pie chart data
      data: computed(() => {
        // create an object to store the user ID and post count
        const userPosts = {};
        // loop through the posts data and count the posts by user ID
        for (let post of posts.value) {
          if (userPosts[post.userId]) {
            userPosts[post.userId]++;
          } else {
            userPosts[post.userId] = 1;
          }
        }
        // create an array to store the pie chart data
        const pieData = [];
        // loop through the userPosts object and push the name and value to the pieData array
        for (let userId in userPosts) {
          pieData.push({
            name: `User ${userId}`,
            value: userPosts[userId],
          });
        }
        // return the pieData array
        return pieData;
      }),
      emphasis: {
        itemStyle: {
          shadowBlur: 10,
          shadowOffsetX: 0,
          shadowColor: "rgba(0, 0, 0, 0.5)",
        },
      },
    },
  ],
};
</script>
```

如果后端接口返回的是 `alarmtype` 及其对应的数量，可以先用 Fetch API 发送 GET 请求携带 JSON 格式参数：

```js
// create a ref for the response data
const alarmData = ref(null);

// fetch the alarm data on mounted hook
onMounted(async () => {
  // create a json object with your parameters
  const params = {
    dispose: 0,
    alarmtype: 11,
  };
  // convert the json object to a query string
  const query = new URLSearchParams(params).toString();
  // fetch the alarm data from the backend API with the query string
  const response = await fetch(`https://example.com/api/alarm?${query}`);
  // convert the response to JSON format
  const data = await response.json();
  // assign the data to the alarmData ref
  alarmData.value = data;
});
```

后端返回的数据类似于：

```json
{
  "1": 10,
  "2": 20,
  "3": 30,
  "4": 40,
  "5": 50
}
```

在 `options` 中使用计算属性，将后端数据转换为饼图所需的数据格式：

```js
// define options
const options = {
  // ...
  series: [
    {
      type: "pie",
      radius: "50%",
      // use a computed property to transform the alarm data to pie chart data
      data: computed(() => {
        // create an array to store the pie chart data
        const pieData = [];
        // loop through the alarmData object and push the name and value to the pieData array
        for (let alarmtype in alarmData.value) {
          pieData.push({
            name: `Alarm Type ${alarmtype}`,
            value: alarmData.value[alarmtype],
          });
        }
        // return the pieData array
        return pieData;
      }),
      // ...
    },
  ],
};
```
