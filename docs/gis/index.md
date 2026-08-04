---
comments: false
tags: [GIS]
difficulty: 进阶
status: published
---

# GIS 与遥感

> 用代码处理地理数据：Google Earth Engine 云端计算、Python 栅格数据处理与遥感指数计算。

## 适合谁 / 前置要求

- 有 Python 基础（至少完成 [NumPy 入门](../AI/data-analysis/numpy/index.md)）；
- 了解基本地理概念（坐标系、栅格、矢量）。

## 学习路径

1. [GEE 笔记](gee-notes.md)（进阶）：问题与技巧、遥感指数、变化检测算法；
2. [GIS-Python](gis-python.md)（进阶）：rasterio、GDAL 与栅格数据处理。

## 能力目标

学完后你能在 GEE 中完成影像筛选、指数计算与导出，也能用 Python 处理本地栅格数据。

## 配套练习 / 项目

- 在 GEE 中计算某区域 NDVI 并导出时间序列；
- 用 rasterio 读取一张影像并输出波段统计信息；
- 尝试把结果接入[前端 Cesium](../frontend/04-cesium.md) 做可视化。

## 参考资源

- [Google Earth Engine 文档](https://developers.google.com/earth-engine)；
- [rasterio 文档](https://rasterio.readthedocs.io/)。
