---
tags: [前端]
difficulty: 进阶
status: published
---

# Vue3 setup 语法糖下的 ECharts 使用

## 1 引入

## 2 入门案例

```vue
<template>
  <div>
    <!-- ECharts 默认容器宽高为 0,需要自行设置宽高 -->
    <div ref="myChart" class="demoDiv"></div>
  </div>
</template>
<script setup>
import { onMounted, ref } from "vue";
//  1. 引用 echarts
import * as echarts from "echarts";
const myChart = ref();
// onMount 挂载后调用
// 在 dom 渲染完毕后进行初始化
onMounted(() => {
  // 基于准备好的dom，初始化echarts实例
  const myECharts = echarts.init(myChart.value);
  // 指定图表的配置项和数据
  const option = {
    // 标题
    title: {
      text: "ECharts 入门示例",
    },
    tooltip: {},
    //
    legend: {
      data: ["销量"],
    },
    // X 轴数据
    xAxis: {
      data: ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"],
    },
    // Y 轴数据
    yAxis: {},
    // 区域缩放显示
    dataZoom: [
      {
        // startValue: '2014-06-01'
      },
      {
        type: "inside",
      },
    ],
    // 左上角小工具
    toolbox: {
      left: "left",
      feature: {
        dataZoom: {
          yAxisIndex: "none",
        },
        restore: {},
        saveAsImage: {},
      },
    },
    // **系列** 设置当前数据所能映射的图形
    series: [
      {
        name: "销量",
        // 图标类型, bar柱状图, line折线图
        type: "line",
        data: [5, 20, 36, 10, 10, 20],
      },
    ],
  };
  // 使用刚指定的配置项和数据显示图表
  myECharts.setOption(option);
});
</script>
<style scoped>
.demoDiv {
  width: 100%;
  height: 350px;
}
</style>
```

## 3 封装子组件

> 父组件中

```vue
<template>
  <div>
    <waterChart :height="'300px'" :width="'100%'" :rawData="waterData" />
  </div>
</template>
<script setup>
import waterChart from "@/components/form/waterChart.vue";
</script>
```

> 图表子组件中

```vue
<template>
  <div>
    <div ref="indexChart" :style="{ width: width, height: height }"></div>
  </div>
</template>
<script setup>
import { onMounted, ref, watch, computed } from "vue";
import * as echarts from "echarts";
const props = defineProps({
  width: "",
  height: "",
  rawData: [],
});
const xdata = ref([]);
const ydata = ref([]);
const initSetting = () => {
  const useData = JSON.parse(JSON.stringify(props.rawData));
  console.log("useData", useData);
  xdata.value = useData.map((v) => v.Time);
  ydata.value = useData.map((v) => v.cl);
  console.log("xdata", JSON.parse(JSON.stringify(xdata.value)));
  console.log("ydata", JSON.parse(JSON.stringify(ydata.value)));
};
initSetting();
const indexChart = ref();
const initWaterChart = async () => {
  const e = indexChart.value;
  if (!e) return;
  const myECharts = echarts.init(e);
  const option = {
    title: {
      text: "余氯",
      left: "center",
    },
    tooltip: {
      trigger: "axis",
    },
    xAxis: {
      data: xdata.value,
    },
    yAxis: {},
    dataZoom: [
      {
        // startValue: '2014-06-01'
      },
      {
        type: "inside",
      },
    ],
    toolbox: {
      left: "left",
      feature: {
        dataZoom: {
          yAxisIndex: "none",
        },
        restore: {},
        saveAsImage: {},
      },
    },
    series: [
      {
        name: "销量",
        type: "line",
        data: ydata.value,
      },
    ],
  };
  myECharts.setOption(option);
};
onMounted(() => {
  initWaterChart();
});
</script>
<style scoped></style>
```

不要使用ref包裹ECharts实例，这会使得其自带的图表交互工具无法使用：https://github.com/apache/echarts/issues/16642