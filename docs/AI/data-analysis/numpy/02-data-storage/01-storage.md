---
tags: [AI, 数据分析]
difficulty: 入门
status: published
---

# 0201 NumPy 数据存取

## 1 CSV 文件的存取

CSV 文件：逗号分隔值文件，常见存储一维二维数据的文件格式，用于存储批量数据。

### 1.1 将数据写入 CSV 文件

```python
np.savetxt(frame, array, fmt='%.18e', delimiter=None)
```

- `frame` ：要写入的文件、字符串或产生器的名字，可以是 .`gz` 或 `.bz2` 的压缩文件。

- `array` ：存入文件的数组。

- `fmt` ：写入文件的格式，默认是科学计数法保留 18 位小数 `%.18e`

  【用户主要修改参数】，还有其他参数如：`%d` `%.2f` ...

- `delimiter` ：分割字符串，默认是任何空格。

【注：】

1. CSV 文件中要求的数据分割符是 `,`
1. `np.savetxt()` 函数并不是只为生成 CSV 文件服务的，它还可以生成任何带有特定分割字符串的其他文件。

#### 1.1.1 写入 CSV 文件

&emsp;&emsp;接下来我们会生成一个元素为 0~100，形状为 5行，20列的数组。并将其写入一个名为 `a.csv` 的文件中。

> ipython

![1 写入 CSV 文件 截图](../../../../assets/images/py-image-20220827160841536-012.webp)

> python

```python
import numpy as np
a = np.arange(100).reshape(5, 20)
np.savetxt('a.csv', a, fmt='%d', delimiter=',')
```

#### 1.1.2 打开 CSV 文件

![2 打开 CSV 文件 截图](../../../../assets/images/py-image-20220827163259191-013.webp)

### 1.2 读取 CSV 文件中的数据

```python
np.loadtxt(frame, dtype=np.float, delimiter=None, unpack=False)
```

- `frame` ：要读取的文件、字符串或产生器的名字，可以是 `.gz` 或 `.bz2` 的压缩文件。
- `dtype` ：数据类型，可选，默认是 `np.float` 。
- `delimiter` ：分割字符串，默认是任何空格。
- `unpack` ：如果True，读入属性将分别写入不同变量。

&emsp;&emsp;接下来我们会把之前生成的名为 `a.csv` 的文件读取成数组形式，可以看到默认读取的数据类型为浮点数类型。

> ipython

![2 读取 CSV 文件中的数据 截图](../../../../assets/images/py-image-20220827164657678-014.webp)

> python

```python
import numpy as np
a = np.arange(100).reshape(5, 20)
np.savetxt('a.csv', a, fmt='%d', delimiter=',')
b = np.loadtxt('a.csv', delimiter=',')
print(b)
```

### 1.3 CSV 文件的局限性

&emsp;&emsp;CSV 只能有效存储一维和二维数组。

&emsp;&emsp;因此这里 `np.savetxt()` 和 `np.loadtxt()` 也只能有效存取一维和二维数组。

## 2 多维数组的存取

### 2.1 存储数据

```python
a.tofile(frame="b.txt", sep='', format='%d')
```

- `frame`：存储的文件名或字符串名，将数据存放在 `b.txt` 文件内
- `sep`：数据分割字符串，以空格进行分割
- `format`：存储后数据的格式，设置为整数形式

### 2.2 读取数据

```python
np.fromfile("b.txt", dtype=np.int32, sep=",")
```

- `frame`：可省略，直接写要读取的文件名，读取 `b.txt` 文件内的数据
- `dtype`：读取数据的数据类型
- `count`：读取数据个数，默认为 -1 表示读入整个文件，可省略
- `sep`：同上 2.1 节

【注】：在读取存入数据时，必须知晓数据的维度和元素类型

上一节：[0103 NumPy Quick Start 翻译](../01-basics/02-quickstart.md)

下一节：[0202 WordCloud 词云](./02-wordcloud.md)