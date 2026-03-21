# 0103 NumPy QuickStart 翻译

官网原文：https://numpy.org/devdocs/user/quickstart.html

## 生词

| 单词 / 词组                                        | 翻译                                  |
| -------------------------------------------------- | ------------------------------------- |
| refresher                                          | n. 补习课程；a. 专业性复习进修的      |
| for a refresher                                    | 如需复习                              |
| in addition to (NumPy)                             | 除了 (NumPy) 之外                     |
| learner profile                                    | 学习特征                              |
| overview                                           | n. 概述，综述；v. 概述，综述          |
| quick overview                                     | 简要概述                              |
| dimension                                          | n. 方面；尺寸；维度                   |
| demonstrate                                        | v. 证明；示范，演示                   |
| dimensional                                        | a. 维度的                             |
| n-dimensional                                      | 多维的                                |
| represent                                          | v. 代表；表示                         |
| manipulate                                         | v. 操作；使用                         |
| particular                                         | a. 特别的；特殊的；详细的 n. 详情     |
| in particular                                      | 尤其，特别是                          |
| apply (common functions) to (n-dimensional arrays) | 将 (常用函数) 应用于 (多维数组)       |
| this article                                       | 本文                                  |
| linear                                             | a. 线性的；连续的；（关系）直接明显的 |
| algebra                                            | n. 代数；代数学                       |
| operation                                          | n. 运作；运营 ；（数学）运算          |
| linear algebra                                     | 线性代数                              |
| some linear algebra operations                     | 一些线性代数运算                      |
| homogeneous                                        | a. 同类型的                           |
| multidimensional                                   | a. （数学）多维的                     |
| index by (...)                                     | 由（...）进行索引                     |
| coordinate                                         | n. 坐标                               |
| functionality                                      | n. 功能；[数] 泛函性，函数性          |
| attribute                                          | n. 属性                               |
| indicating                                         | v. 表明；暗示                         |
| matrix                                             | n. 矩阵；模型                         |
| therefore                                          | ad. 因此，所以                        |
| the total number of (elements of the array)        | (数组元素) 的总数                     |
| is equal to (...)                                  | 等于 (...)                            |
| the product of (...)                               | (...) 之积                            |
| describe                                           | v. 描述                               |
| specify                                            | v. 明确指出                           |
| additionally                                       | 另外；此外                            |
| provide                                            | v. 提供                               |



## 1 要求

你需要了解一点 Python 的知识，如需复习，请参考 [Python tutorial](https://docs.python.org/tutorial/)

要运行示例，除了 NumPy 之外，还需要安装 matplotlib

### 1.1 学习特征

&emsp;&emsp;这是 NumPy 中数组的一个快速概括。它演示了如何表示和操作多维数组（n >= 2)。特别是，如果你不知道如何将常用函数应用于多维数组（不使用 for 循环），或者如果你想去了解多维数组的轴和形状属性，本文可能会有所帮助。

### 1.2 学习目标

在阅读后，你应该能够：

- 了解在 NumPy 中一维、二维以及多维数组间的不同
- 了解如何在不使用 for 循环的情况下，将一些线性代数运算应用于多维数组
- 了解多维数组的轴和形状属性



## 2 基础知识

&emsp;&emsp;NumPy 的主要对象是同类型的多维数组。它是一个元素表（通常是数字），所有相同的类型，由非负整数元组索引。在 NumPy 中，维度也被称为轴。

&emsp;&emsp;例如，在三维空间中点的坐标数组 `[1, 2, 1]` ，有一个轴。这个轴有三个元素在其中，因此我们说它的长度为 3。在下面的例子中，数组有两个轴。第一个轴长度为 2，第二个轴长度为 3。

```python
[[1., 0., 0.],
 [0., 1., 2.]]
```

&emsp;&emsp;NumPy 的数组类型被称为 ndarray。它也作为别名数组而被知晓。注意 `numpy.ndarray` 与标准 Python 库类的 `array.array` 不同，`array.array` 它只能处理一维数组且提供的功能较少。ndarray 对象更重要的属性是：

**ndarray.ndim**

&emsp;&emsp;数组轴数（维度）

**ndarray.shape**

&emsp;&emsp;数组的尺寸。这是一个整数元组，表示每个维度中数组的大小。对于一个 n 行，m 列的矩阵，`shape` 为 `(n,m)` 。`shape` 元组的长度就是轴的数量，ndim。

**ndarray.size**

&emsp;&emsp;数组元素的总和。这等于 `shape` 元素的乘积。

**ndarray.dtype**

&emsp;&emsp;一个描述数组元素类型的对象。可以使用标准 Python 类型创建或指定 dtype。另外，NumPy 还提供了它自己的类型。`numpy.int32` ，`numpy.int16` 以及 `numpy.float64` 都是一些例子。

**ndarray.itemsize**

&emsp;&emsp;数组中每个元素的字节大小。例如，一个数组的元素类型 `float64` 有 8 个字节的元素大小（=64/8），另一种 `complex32` 的元素大小为 4（=32/8）。

**ndarray.data**

&emsp;&emsp;





上一节：[0102 NumPy 库基础操作](./02-basics.md)

下一节：[0201 NumPy 数据存取](../02-data-storage/01-storage.md)

