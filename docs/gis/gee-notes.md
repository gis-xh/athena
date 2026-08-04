---
tags: [GIS]
difficulty: 进阶
status: published
---

# GEE 笔记

Google Earth Engine（GEE）使用中的问题记录与遥感指数整理。

## 数据选择

### 老挝森林扰动调查用哪个哨兵数据？

如果想调查老挝地区的森林扰动情况，可以使用 S2 或 S1 数据，根据目标和需求选择不同方法：

- **S2**：提供高分辨率多光谱影像，适用于监测植被、土壤和水体覆盖、土地覆盖变化，以及人道主义和灾害风险等应用。可以使用经典的图像对图像变化检测方法，或时间序列分析方法（基于阈值、基于指数、基于模型、基于机器学习四类）识别森林扰动的时间和空间分布。各方法均有优缺点，可根据数据质量、可用性、计算能力和精度要求选择合适的方案。
- **S1**：提供双极化 C 波段合成孔径雷达（SAR）数据，能够在所有天气条件（甚至云层）下于白天和夜晚获取有意义的数据，常用于海上活动、海冰制图、人道援助、危机响应和森林管理等。在云层覆盖严重或数据缺失的情况下，可用 SAR 数据补充或替代 S2 数据。不过需要注意 SAR 的特殊性质（雷达几何效应、散射机制、相干性等），并选择合适的预处理步骤和分析算法。

## 土地覆盖数据

- GEE 调用 Esri 全球土地覆盖数据（2017~2021）：<https://www.gisrsdata.com/pages/a8b180/>

## 字段计算器

将 `class` 字段的类别映射为数字编码的 `class_id`。

逐行运行：

```python
if("class"='agricultural',0,"class_id")
if("class"='building',1,"class_id")
if("class"='human forest',2,"class_id")
if("class"='nature forest',3,"class_id")
if("class"='road',4,"class_id")
if("class"='water',5,"class_id")
```

批量运行：

```python
case
  when "class"='agricultural' then 0
  when "class"='building' then 1
  when "class"='human forest' then 2
  when "class"='nature forest' then 3
  when "class"='road' then 4
  when "class"='water' then 5
  else "class_id"
end
```

## geemap 使用问题

### 用 geemap 实现朴素贝叶斯土地分类

问题：本地上有存放样本的 shp 数据，也有对应的哨兵影像数据，如何用 geemap 实现朴素贝叶斯的土地分类？

（待补充）

### 按掩膜图层裁剪栅格数据

问题：geemap 如何实现按掩膜图层裁剪栅格数据？

（待补充）

### 分割图两边图层各自添加图层控件

问题：在使用 geemap 创建分割图时，能否将两边图层都添加各自的图层控件？

（待补充）

参考：[吴秋生 GEEMAP 教程 120 javascript](https://geemap.org/notebooks/120_javascript/)

### 获取当前最上层图层

```python
cluster_layer = Map.layers[-1]
```

### Code Editor out of Sync

提示 `Code Editor out of Sync` 时，打开浏览器 JavaScript 控制台查看 bug report 说明。

### 贝叶斯分类结果异常

问题：在 GEE 平台上手动采样了 200 个采样点，使用同一份数据做随机森林分类、CART 分类结果都没问题，但贝叶斯分类的结果不行。

（待补充）

## 在 Python 中调用 Earth Engine JavaScript 库

基于开放地球引擎库（OEEL）调用 Earth Engine JavaScript 库中的函数，详细信息见 <https://www.open-geocomputing.org/OpenEarthEngineLibrary>。

为 JavaScript 库启用选项卡补全：

```python
%config IPCompleter.use_jedi = False
```

导入 Earth Engine JavaScript 库：

```python
requireJS(lib_path=None, Map=None)
```

oeel 是 requireJS 所需的依赖，需要先安装：

```sh
pip install oeel
```

## 遥感变化检测算法

按发布时间重新排序的遥感影像变化检测算法列表：

| 算法名称   | 发布时间 | 算法介绍                                                                                                                                                                                             | 算法优点                                                                                                                                                                  | 算法缺点                                                             |
| :--------- | :------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :------------------------------------------------------------------- |
| CUSUM      | 1954     | Cumulative Sum (CUSUM) 是一种用于检测时间序列数据集的均值值的变化的统计方法，可以检测数据的均值值和方差随时间的变化。                                                                                | 1. 可以检测时间序列数据集中均值值的突变和渐变变化。2. 可以与不同类型的遥感数据一起使用。                                                                                  | 1. 需要大量数据来训练模型。2. 该模型计算密集，需要大量内存才能运行。 |
| MOSUM      | 1994     | Moving Sum (MOSUM) 是一种用于检测时间序列数据集中均值值在短于整个时间序列数据集长度的时间间隔内发生变化的统计方法，可以检测数据的均值值和方差随时间间隔而变化。                                      | 可以检测时间序列数据集中均值值在短于整个数据集长度的时间间隔内发生的突变和渐变变化。                                                                                      | 需要大量数据来训练模型                                               |
| BFAST      | 2008     | Breaks For Additive Seasonal and Trend (BFAST) 是一种基于时间序列的变化检测算法，使用统计方法识别遥感数据时间序列中的突变点，可以检测数据的均值值和方差随时间的变化，以及数据季节性模式的变化。      | 1. 可以检测时间序列数据集中均值值和方差随时间的突变和渐变变化。2. 可以处理缺失数据和云覆盖。3. 可以与不同类型的遥感数据一起使用。                                         | 1. 需要大量数据来训练模型。2. 该模型计算密集，需要大量内存才能运行。 |
| CCDC       | 2010     | Continuous Change Detection and Classification (CCDC) 是一种基于时间序列的遥感影像变化检测算法，专门设计用于遥感影像，使用所有可用地球卫星数据对时空光谱特征进行建模，包括季节性、趋势和光谱可变性。 | 1. 在检测土地覆盖和土地利用方面具有高精度。2. 可以检测幅度和方向上的变化。3. 可以处理缺失数据和云覆盖。4. 可以在不同类型的遥感数据上使用。5. 可以在不同时间检测多个更改。 | 1. 需要大量数据来训练模型。2. 该模型计算密集，需要大量内存才能运行。 |
| LandTrendr | 2013     | LandTrendr 是一种用于检测遥感图像时序中趋势和干扰的算法，由美国地质调查局 EROS 中心科学家开发，使用分段回归方法从卫星图像时序中识别趋势、干扰和恢复期。                                              | 可以从卫星图像时序中检测趋势、干扰和恢复期。                                                                                                                              | 需要大量数据来训练模型                                               |

## 遥感指数

### ADEI 指数

ADEI 指数是一种用于评估植被干旱程度的指标，基于 EVI 和 LST 的线性组合。计算公式：

$$ADEI = 0.69 \times EVI - 0.01 \times LST + 0.18$$

在 GEE 平台上可以使用 JavaScript 或 Python 语言编写表达式计算：

```javascript
var adei = image
  .expression("0.69 * EVI - 0.01 * LST + 0.18", {
    EVI: image.select("EVI"),
    LST: image.select("LST"),
  })
  .rename("ADEI");
```

其中，EVI 是增强植被指数，LST 是地表温度，image 是输入的影像数据。

### AWEI 指数

AWEI 指数是一种自动水体提取指数，基于蓝光、绿光、近红外、中红外 1 和中红外 2 波段的线性组合，用于区分水体和非水体。计算公式：

$$AWEI = \frac{blue + 2.5 \times green - 1.5 \times (nir - swir1) - (0.25 \times swir2)}{blue + green + nir + swir1 + swir2}$$

GEE 表达式：

```javascript
var awei = image
  .expression(
    "(blue + 2.5 * green - 1.5 * (nir - swir1) - (0.25 * swir2))/ (blue + green + nir + swir1 + swir2)",
    {
      green: image.select("B3"),
      blue: image.select("B2"),
      nir: image.select("B8"),
      swir1: image.select("B11"),
      swir2: image.select("B12"),
    },
  )
  .rename("AWEI");
```

其中，image 是输入的影像数据；blue 为蓝光波段，green 为绿光波段，nir 为近红外波段，swir1 为中红外 1 波段，swir2 为中红外 2 波段。

## 参考链接

- [Qiusheng Wu (wetlands.io)](https://wetlands.io/)