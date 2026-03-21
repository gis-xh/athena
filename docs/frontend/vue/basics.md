## 1. 路径问题
    @/  相对路径
    ./  绝对路径

## 2. margin 和 padding 

  - margin: 块级元素之间的距离
  - padding: 块级元素内部的距离

```css
/* 上10 右20 下30 左40 */
margin: 10px 20px 30px 40px;

/* 上10 左右20 下30 */
margin: 10px 20px 30px;

/* 上下10 左右20 */
margin: 10px 20px;


/* 上下左右10 */
margin: 10px;
```

## 3. ~ 和 ^
    版本号: 如 1.2.2，遵循 “大版本.次要版本.小版本”
    ~ tilde: 如 ~1.2.2，表示安装 1.2.x 的最新版本（不低于 1.2.2），但是不安装 1.3.x
    ^ caret: 如 ˆ1.2.2，表示安装 1.x.x 的最新版本（不低于 1.2.2），但是不安装 2.x.x

## 4. position 属性
  - relative 相对定位
  - absolute 绝对定位
  - fixed 固定定位
  - static 默认值

## 5. 居中

```css
justify-content: center;
align-items: center;
text-align: center;
```

## 6. DOM

   [HTML DOM 教程 | 菜鸟教程](https://www.runoob.com/htmldom/htmldom-tutorial.html)

## 7. 函数

   `setInterval()` 是一个实现定时调用的函数，可按照指定的周期（以毫秒计）来调用函数或计算表达式。

## 8. 基本形式

```javascript
const res = {
    // data() 方法
    data() {
        return {
            
        }
    },
    // mounted 方法，钩子函数，在 DOM 页面加载完成后执行内部函数
    mounted() {
        
    },
    // 对象，定义需要使用的方法/事件回调函数
    methods: {
        
    },
    // 对象，可以设置多个监听属性，每个都是一个方法
    computed: {
        
    }
}
// 创建一个 vue 应用
const app = Vue.createApp({})
app.use(res)
app.mount('#res')
```



```javascript
data:functaion() {
    
}
```



![实例的生命周期](https://v3.cn.vuejs.org/images/lifecycle.svg)



## 9. 前后端交互

### 9.1 方法一

> api 文件中

```js
export function getWater() {
  return axios.get("/water/");
}
```

> vue 中

```js
// 引入 getWater 方法
import { getWater } from "@/api/api";
import { onMounted, reactive } from "vue";
const waterData = reactive({ list: [] });
// 在 dom 渲染完毕后进行初始化
onMounted(() => {
  // 获取后端水质数据
  getWater().then((res) => {
    waterData.list.value = res.results;
    total.value = res.count;
  });
});
```

### 9.2 方法二

> api 文件中

```js
export function getWater(params) {
  return axios.get("/water/" + "?page=" + params);
}
```

> vue 中

```js
const pageFrom = ref({
  pagenum: 1,
});
const total = ref(0);
const loading = ref(false);
const waterData = ref([]);
const initGetWaterList = async () => {
  // 开始加载
  loading.value = true;
  // 传入页码值,并等待获取后端水质数据
  const res = await getWater(pageFrom.value.pagenum);
  total.value = res.count;
  waterData.value = res.results;
  // 结束加载状态
  loading.value = false;
};
initGetWaterList();
```



## 10. `forEach()` 循环

### 10.1 示例代码

```js
const typeRadios = ref([
  {
    value: "table",
    label: "表格",
  },
  {
    value: "chart",
    label: "图形",
  },
]);
const currentType = ref("table");
const handleTypeRadioClick = (v) => {
  typeRadios.value.forEach((type) => {
    if (v === type.label) {
      currentType.value = type.value;
    }
  });
  console.log(currentType.value);
};
```

### 10.2 个人理解

将 `typeRadios` 中的每个对象都定义为 `type`，从 `type` 中取对应的值，逐一进行判断。



## 11 `prop` 属性

### 11.1 参考文章

1. [CSDN - vue中组件的props属性（详）](https://blog.csdn.net/qq_41579104/article/details/120997444)
2. 

## 12 `json.parse()` 和 `json.stringify()` 的使用和区别

### 12.1 参考文章

1. [CSDN - vue打印结果时为proxy对象，如何获取值。json.parse()和json.stringify()的使用和区别](https://blog.csdn.net/pipizhou16/article/details/125480695)
2. [CSDN - Vue3 getters打印结果是Proxy对象，怎么获取其中的值 & json.stringify()与json.parse()的区别和使用](https://blog.csdn.net/weixin_44867717/article/details/120461980)

### 12.2 个人理解

```js
console.log(props.rawData);
console.log(JSON.parse(JSON.stringify(props.rawData)));
```

- `JSON.stringify()` ：将 JavaScript 对象转换为 JSON 字符串

- `JSON.parse()` ：将 JSON 字符串转为一个对象



## 参考文章

1. [CSDN - vue中组件的props属性（详）](https://blog.csdn.net/qq_41579104/article/details/120997444)
2. [CSDN - vue打印结果时为proxy对象，如何获取值。json.parse()和json.stringify()的使用和区别](https://blog.csdn.net/pipizhou16/article/details/125480695)
3. [CSDN - Vue3 getters打印结果是Proxy对象，怎么获取其中的值 & json.stringify()与json.parse()的区别和使用](https://blog.csdn.net/weixin_44867717/article/details/120461980)
4. 
