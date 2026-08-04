---
tags: [AI, 深度学习, 课程]
difficulty: 进阶
status: published
---

# 神经网络与深度学习（CS230 C1）

> 本页是吴恩达 CS230 第一门课《Neural Networks and Deep Learning》的结构化学习笔记，涵盖逻辑回归、浅层神经网络与深层神经网络的核心原理和 Python 实现。

## 学习目标

- 理解逻辑回归如何作为神经网络的“最小单元”；
- 掌握神经网络的前向传播与反向传播直觉，而不只是背公式；
- 能够用 NumPy 从零实现一个两层神经网络；
- 理解深层网络为什么有效，以及常见训练陷阱。

## 前置要求

- Python 基础与 NumPy 数组操作；
- 简单的线性代数（矩阵乘法、转置）与微积分（链式法则）；
- 建议先完成本仓库的 [NumPy 入门](../../../AI/data-analysis/numpy/index.md)。

## 1 从逻辑回归开始

### 1.1 问题设定

逻辑回归解决**二分类**问题：输入特征 $x \in \mathbb{R}^{n_x}$，输出 $\hat{y} \in [0,1]$ 表示属于正类的概率。例如判断一张图片是否为猫。

模型只有两步：

1. 线性组合：$z = w^T x + b$
2. 非线性映射：$\hat{y} = a = \sigma(z) = \frac{1}{1+e^{-z}}$

其中 $w$ 是权重向量，$b$ 是偏置。参数数量为 $n_x + 1$。

### 1.2 为什么用 sigmoid 而不是直接输出 z

分类问题的输出需要被解释为概率，sigmoid 将任意实数压缩到 $(0,1)$，并且导数形式优美：

$$
\sigma'(z) = \sigma(z)(1 - \sigma(z))
$$

这个性质让反向传播的计算变得非常简洁。

### 1.3 损失函数

对单个样本使用**对数损失（交叉熵）**：

$$
L(a, y) = -[y \log a + (1-y) \log(1-a)]
$$

为什么不使用平方误差？因为平方误差在 sigmoid 上会导致非凸优化、收敛缓慢；交叉熵在分类问题上是凸的，且当预测错误时梯度更大。

### 1.4 梯度下降

整个训练集的代价函数是单个样本损失的平均：

$$
J(w,b) = \frac{1}{m}\sum_{i=1}^{m} L(a^{(i)}, y^{(i)})
$$

梯度下降迭代更新：

$$
w := w - \alpha \frac{\partial J}{\partial w}, \qquad b := b - \alpha \frac{\partial J}{\partial b}
$$

其中 $\alpha$ 是学习率。核心计算（向量化后）：

$$
\frac{\partial J}{\partial w} = \frac{1}{m} X (A - Y)^T, \qquad \frac{\partial J}{\partial b} = \frac{1}{m} \sum (A - Y)
$$

### 1.5 向量化实现

> 不要用 for 循环遍历样本，用矩阵运算一次算完。

```python
import numpy as np

def sigmoid(z):
    return 1 / (1 + np.exp(-z))

def initialize(dim):
    w = np.zeros((dim, 1))
    b = 0.0
    return w, b

def propagate(w, b, X, Y):
    m = X.shape[1]
    A = sigmoid(np.dot(w.T, X) + b)          # 前向
    cost = -np.sum(Y * np.log(A) + (1 - Y) * np.log(1 - A)) / m
    dw = np.dot(X, (A - Y).T) / m            # 反向
    db = np.sum(A - Y) / m
    return dw, db, cost

def train(X, Y, num_iterations=2000, learning_rate=0.005):
    w, b = initialize(X.shape[0])
    for _ in range(num_iterations):
        dw, db, cost = propagate(w, b, X, Y)
        w -= learning_rate * dw
        b -= learning_rate * db
    return w, b
```

**常见坑**：`X` 的每一列是一个样本，形状为 `(n_x, m)`；初始化 `w` 用零向量即可，但神经网络层**不能用全零初始化**。

## 2 浅层神经网络

### 2.1 为什么需要隐藏层

逻辑回归只能学习线性决策边界。加入一个隐藏层后，网络先学习特征组合（例如边缘、形状），再在输出层做线性分类，从而表达非线性边界。

### 2.2 符号约定

- 上标 $[1]$ 表示第 1 层，$[2]$ 表示输出层；
- 隐藏层有 $n^{[1]}$ 个神经元，权重 $W^{[1]}$ 形状为 $(n^{[1]}, n_x)$，偏置 $b^{[1]}$ 形状为 $(n^{[1]}, 1)$；
- 激活值 $A^{[l]} = g^{[l]}(Z^{[l]})$。

### 2.3 激活函数怎么选

| 函数 | 公式 | 输出范围 | 适用场景 |
| --- | --- | --- | --- |
| sigmoid | $\sigma(z)=1/(1+e^{-z})$ | $(0,1)$ | 二分类输出层 |
| tanh | $\tanh(z)$ | $(-1,1)$ | 隐藏层（一般优于 sigmoid） |
| ReLU | $\max(0,z)$ | $[0,+\infty)$ | 默认隐藏层选择 |

隐藏层建议优先 ReLU：计算快、缓解梯度消失；tanh 是零中心化，收敛比 sigmoid 快。

### 2.4 前向传播

$$
Z^{[1]} = W^{[1]} X + b^{[1]}, \quad A^{[1]} = g^{[1]}(Z^{[1]})
$$

$$
Z^{[2]} = W^{[2]} A^{[1]} + b^{[2]}, \quad A^{[2]} = \sigma(Z^{[2]})
$$

### 2.5 反向传播的直觉

反向传播就是**把输出层的误差逐层往回传**，用链式法则计算每个参数的梯度：

$$
dZ^{[2]} = A^{[2]} - Y
$$

$$
dW^{[2]} = \frac{1}{m} dZ^{[2]} A^{[1]T}, \qquad db^{[2]} = \frac{1}{m} \sum dZ^{[2]}
$$

$$
dZ^{[1]} = W^{[2]T} dZ^{[2]} \odot g^{[1]\prime}(Z^{[1]})
$$

其中 $\odot$ 表示逐元素相乘。记住一句话：**前向传播存下中间量（缓存），反向传播消费这些中间量**。

### 2.6 两层网络实现

```python
def initialize_2layer(n_x, n_h, n_y):
    np.random.seed(1)
    W1 = np.random.randn(n_h, n_x) * 0.01
    b1 = np.zeros((n_h, 1))
    W2 = np.random.randn(n_y, n_h) * 0.01
    b2 = np.zeros((n_y, 1))
    return {"W1": W1, "b1": b1, "W2": W2, "b2": b2}

def forward(X, params):
    Z1 = np.dot(params["W1"], X) + params["b1"]
    A1 = np.tanh(Z1)
    Z2 = np.dot(params["W2"], A1) + params["b2"]
    A2 = sigmoid(Z2)
    cache = {"Z1": Z1, "A1": A1, "Z2": Z2, "A2": A2}
    return A2, cache

def backward(X, Y, params, cache):
    m = X.shape[1]
    dZ2 = cache["A2"] - Y
    dW2 = np.dot(dZ2, cache["A1"].T) / m
    db2 = np.sum(dZ2, axis=1, keepdims=True) / m
    dZ1 = np.dot(params["W2"].T, dZ2) * (1 - np.tanh(cache["Z1"]) ** 2)
    dW1 = np.dot(dZ1, X.T) / m
    db1 = np.sum(dZ1, axis=1, keepdims=True) / m
    return {"dW1": dW1, "db1": db1, "dW2": dW2, "db2": db2}
```

**常见坑**：初始化权重用 `randn * 0.01` 而不是零；偏置可以初始化为零；`tanh` 的导数是 $1 - \tanh^2(z)$，不要写成 sigmoid 的导数。

## 3 深层神经网络

### 3.1 为什么需要“深”

浅层网络需要指数级数量的神经元才能表达某些函数，而深层网络通过**逐层抽象**（边缘 → 部件 → 物体）用更少的参数表达同样函数。深度不是玄学，是表达效率。

### 3.2 通用前向与反向

第 $l$ 层的参数 $W^{[l]}, b^{[l]}$，前向：

$$
Z^{[l]} = W^{[l]} A^{[l-1]} + b^{[l]}, \quad A^{[l]} = g^{[l]}(Z^{[l]})
$$

反向从最后一层开始：

$$
dA^{[L]} = -\frac{Y}{A^{[L]}} + \frac{1-Y}{1-A^{[L]}}
$$

逐层计算 $dZ^{[l]}, dW^{[l]}, db^{[l]}, dA^{[l-1]}$，直到第 1 层。

### 3.3 参数 vs 超参数

- **参数**：$W, b$，由梯度下降学习；
- **超参数**：学习率 $\alpha$、层数、每层神经元数、激活函数、迭代次数、batch size，需要人工调。

## 4 实践建议

1. 先搭一个能跑通的小模型，再逐步加深加宽；
2. 检查维度：每步前向传播都打印 `Z`、`A`、`W` 的形状，90% 的 bug 是维度不匹配；
3. 学习率从 0.01 起，观察代价曲线而不是准确率曲线；
4. 训练集代价不下降：先检查数据预处理（归一化）、梯度实现、学习率；
5. 验证集与训练集差距大：增加数据、正则化、早停。

## 5 动手练习

1. **从零实现逻辑回归**：用 `sklearn.datasets.make_moons` 生成数据，实现 `train` 函数并绘制决策边界（提示：用 `plt.contourf` 画等高线）。
2. **对比浅层与深层**：分别用 1 个隐藏层（4 个神经元）和 3 个隐藏层（各 10 个神经元）训练同一份数据，比较训练准确率与拟合速度。
3. **维度检查器**：写一个函数，输入每层神经元数，自动校验任意一批随机参数下前向传播的中间矩阵形状是否正确。
4. **激活函数对比**：把隐藏层分别换成 sigmoid、tanh、ReLU，训练 2000 轮，画出三者的代价曲线并解释差异。

## 参考

- Coursera 吴恩达《Neural Networks and Deep Learning》C1 课程；
- 本仓库配套作业：[作业 1：逻辑回归](./02-assignment-logistic-regression.md)、[课程项目笔记](./03-assignment-projects.md)。
