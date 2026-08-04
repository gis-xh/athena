---
tags: [AI, 深度学习, 课程]
difficulty: 进阶
status: published
---

# CS230 作业 1：用逻辑回归实现二分类

> 本页整理 CS230 C1 第一周编程作业的核心思路：不依赖深度学习框架，用 NumPy 从零搭建逻辑回归分类器，并理解数据预处理、前向/反向传播与优化流程。

## 学习目标

- 掌握图像数据从原始像素到特征矩阵的转换；
- 理解中心化、标准化为什么能加速收敛；
- 亲手实现 sigmoid、初始化、前向传播、代价函数、梯度下降和预测；
- 能独立分析“训练集表现好但测试集差”的原因。

## 前置要求

- 已阅读 [神经网络与深度学习（CS230 C1）](./01-neural-networks.md)；
- 熟悉 NumPy 的 `reshape`、`transpose`、广播机制。

## 1 数据准备

### 1.1 数据集形状

以猫图识别数据集为例（`train_catvnoncat.h5`）：

- `train_set_x` 原始形状：`(m_train, num_px, num_px, 3)`；
- 标签 `train_set_y`：`(1, m_train)`，取值为 0 或 1；
- 测试集同理。

### 1.2 展平图像

把每张图的三维像素块展平成向量：

```python
train_set_x_flatten = train_set_x.reshape(train_set_x.shape[0], -1).T
```

`X` 的最终形状为 `(num_px * num_px * 3, m_train)`，**每一列是一个样本**。

### 1.3 标准化

像素值范围是 0-255，直接除以 255 即可把特征缩放到 $[0,1]$：

```python
train_set_x = train_set_x_flatten / 255.
```

标准化能避免某些特征数值过大主导梯度，让梯度下降更快更稳。

## 2 模型构建

按照“辅助函数 → 初始化 → 前向/反向 → 优化 → 预测”五步组织代码。

### 2.1 sigmoid

```python
def sigmoid(z):
    return 1 / (1 + np.exp(-z))
```

### 2.2 初始化参数

```python
def initialize_with_zeros(dim):
    w = np.zeros((dim, 1))
    b = 0.0
    return w, b
```

逻辑回归是凸优化，`w` 初始化为零没有问题；神经网络才需要随机初始化。

### 2.3 前向传播与代价

```python
def propagate(w, b, X, Y):
    m = X.shape[1]
    A = sigmoid(np.dot(w.T, X) + b)
    cost = -np.sum(Y * np.log(A) + (1 - Y) * np.log(1 - A)) / m
    dw = np.dot(X, (A - Y).T) / m
    db = np.sum(A - Y) / m
    return grads, cost
```

### 2.4 优化（梯度下降）

```python
def optimize(w, b, X, Y, num_iterations, learning_rate):
    for i in range(num_iterations):
        grads, cost = propagate(w, b, X, Y)
        w = w - learning_rate * grads["dw"]
        b = b - learning_rate * grads["db"]
    return params, grads, costs
```

### 2.5 预测

```python
def predict(w, b, X):
    m = X.shape[1]
    A = sigmoid(np.dot(w.T, X) + b)
    return (A > 0.5).astype(int)
```

阈值取 0.5；对正负样本不均衡的数据，可考虑调整阈值。

## 3 常见坑与调试

1. **形状不匹配**：始终检查 `X` 是 `(n_x, m)` 还是 `(m, n_x)`，`reshape` 后立刻打印 `.shape`；
2. **`np.squeeze` 误用**：代价是标量而不是数组时再压缩，避免把维度信息丢掉；
3. **`log(0)` 警告**：数值极小时 `np.log(A)` 可能溢出，可给 `A` 加一个极小值如 `1e-8`；
4. **过拟合**：训练准确率 100% 而测试集 70%，说明模型容量超过数据量，后续课程用正则化解决。

## 4 动手练习

1. 在 `optimize` 中每 100 轮记录一次代价，画出代价曲线，观察学习率 0.001、0.01、0.1 的区别；
2. 把阈值从 0.5 改为 0.3 和 0.7，观察预测结果的变化，思考什么时候阈值不该是 0.5；
3. 尝试去掉标准化步骤重新训练，比较收敛速度和最终准确率；
4. 将实现改写为批处理版本：一次处理全部样本（当前已经是），再改成小批量（每轮 64 个样本）并对比代价曲线。

## 参考

- [神经网络与深度学习（CS230 C1）](./01-neural-networks.md)；
- 作业原题来自 Coursera《Neural Networks and Deep Learning》Week 2 编程作业。
