---
tags: [AI, 数据分析]
difficulty: 入门
status: published
---

# NumPy 常用函数

> 本页整理 NumPy 最常用的三类函数：随机数、统计与梯度，它们是模拟实验、数据分析和机器学习代码中的高频工具。

## 学习目标

- 掌握 `np.random` 常用分布与随机种子；
- 能用 `np.mean`、`np.std`、`np.percentile` 等函数快速描述数据；
- 理解 `np.gradient` 在数值分析中的作用。

## 前置要求

- 已完成 [NumPy 基础操作](../01-basics/01-basics.md)；
- 了解数组（ndarray）的创建与切片。

## 1 随机数函数

### 1.1 固定随机种子

```python
import numpy as np

np.random.seed(42)
```

设置种子后每次生成的随机数相同，保证实验可复现。

### 1.2 常用分布

```python
# 均匀分布 [0, 1)
a = np.random.rand(3, 4)

# 标准正态分布
b = np.random.randn(3, 4)

# 整数随机数
c = np.random.randint(0, 10, size=(2, 5))

# 从数组中随机抽取
d = np.random.choice(["猫", "狗", "鸟"], size=5)
```

## 2 统计函数

```python
data = np.array([3.2, 4.1, 5.0, 6.3, 7.8])

print("均值:", np.mean(data))
print("中位数:", np.median(data))
print("标准差:", np.std(data))
print("方差:", np.var(data))
print("最小值:", np.min(data), "最大值:", np.max(data))
print("分位数:", np.percentile(data, [25, 50, 75]))
print("求和:", np.sum(data))
```

二维数组可以指定轴：

```python
matrix = np.arange(12).reshape(3, 4)
print("按列求均值:", np.mean(matrix, axis=0))
print("按行求和:", np.sum(matrix, axis=1))
```

注意 `axis=0` 表示沿行方向移动（对每一列计算），与直觉相反，容易记错。

## 3 梯度函数

`np.gradient` 计算数组的数值梯度，用于近似导数：

```python
x = np.linspace(0, 2 * np.pi, 100)
y = np.sin(x)
dy = np.gradient(y, x)      # 第二个参数是 x 的间距

print(dy[:5])
```

梯度在优化算法（梯度下降）、图像边缘检测和物理模拟中经常出现。

## 4 综合示例：模拟掷骰子

```python
import numpy as np

np.random.seed(1)
rolls = np.random.randint(1, 7, size=10000)

print("平均点数:", np.mean(rolls))
print("出现 6 的概率:", np.mean(rolls == 6))
print("点数分布:", np.bincount(rolls)[1:])
```

随着实验次数增加，平均值会接近理论值 3.5，这就是大数定律。

## 5 动手练习

1. 生成 1000 个标准正态分布随机数，计算均值与标准差，验证它们接近 0 和 1；
2. 用 `np.percentile` 找出数据的前 10% 与后 10% 分位点；
3. 对 `y = x**2` 在区间 `[-2, 2]` 上计算梯度，找到梯度为零的位置；
4. 用 `np.random.choice` 模拟抛硬币 10000 次，统计正面概率。

## 参考

- [NumPy 数据存取](./01-storage.md)；
- NumPy 官方文档：https://numpy.org/doc/stable/
