# 0203 回调函数监控训练状态

## 1 循环神经网络模型

&emsp;&emsp;在循环神经网络模型中，需要多次调用 `fit()` 函数，每个训练周期的新 `history` 对象会覆盖上个训练周期的相应对象，需要自定义回调函数，保存每一次的训练状态。

```python
for epoch_idx in range(1000):
    print('epochs : ' + str(epoch_idx))
    hist = model.fit(x_train, y_train, epochs=1, batch_size=1, verbose=2, shuffle=False)
is X.shape[0]
	model.reset_states()
```

## 2 示例代码

注：以下代码均在同一个 `*.ipynb` 文件中运行的。

### 2.1 调用相关包

```python
import keras
from keras.utils import np_utils
from keras.datasets import mnist
from keras.models import Sequential
from keras.layers import Dense, Activation
import numpy as np

np.random.seed(3)
```

### 2.2 自定义 history 类

&emsp;&emsp;自定义新的回调函数来监控训练状态。

```python
class CustomHistory(keras.callbacks.Callback):
    def init(self):
        self.train_loss=[]
        self.val_loss=[]
        self.train_acc=[]
        self.val_acc=[]

    def on_epoch_end(self, batch, logs={}):
        self.train_loss.append(logs.get('loss'))
        self.val_loss.append(logs.get('val_loss'))
        self.train_acc(logs.get('accuracy'))
        self.val_acc(logs.get('val_accuracy'))
```

### 2.3 生成数据集

```python
# 调用训练集和测试集
(x_train, y_train), (x_test, y_test) = mnist.load_data()

# 分离训练集和测试集
x_val = x_train[50000:]
y_val = y_train[50000:]
x_train = x_train[:50000]
y_train = y_train[:50000]

# 数据集预处理
x_train = x_train.reshape(50000, 784).astype('float32')/255.0
x_val = x_val.reshape(10000, 784).astype('float32')/255.0
x_test = x_test.reshape(10000, 784).astype('float32')/255.0

# 训练集 & 验证集配比
train_rand_idxs = np.random.choice(50000, 700)
val_rand_idxs = np.random.choice(10000, 700)

x_train = x_train[train_rand_idxs]
y_train = y_train[train_rand_idxs]
x_val = x_val[val_rand_idxs]
y_val = y_val[val_rand_idxs]

# 标签数据独热编码处理
y_train = np_utils.to_categorical(y_train)
y_val = np_utils.to_categorical(y_val)
y_test = np_utils.to_categorical(y_test)
```

### 2.4 模型构建

```python
model = Sequential()
model.add(Dense(units=2, input_dim=28*28, activation='relu'))
model.add(Dense(units=10, activation='softmax'))
```

### 2.5 设置模型训练过程

```python
model.compile(loss='categorical_crossentropy', optimizer='sgd', metrics=['accuracy'])
```

### 2.6 训练模型

```python
custom_hist = CustomHistory()
custom_hist.init()
# 循环神经网络模型
for epoch_idx in range(1000):
    print('epoch : ' + str(epoch_idx))
    model.fit(x_train, y_train, epochs=1, batch_size=10, validation_data=(x_val, y_val), callbacks=[custom_hist])
```

&emsp;&emsp;这里我们将训练周期 1000 次修改为调用 1000 次 `fit()` 函数。

### 2.7 查看训练过程

```python
%matplotlib inline
import matplotlib.pyplot as plt

fig, loss_ax = plt.subplots()

acc_ax = loss_ax.twinx()

loss_ax.plot(custom_hist.train_loss, 'y', label='train loss')
loss_ax.plot(custom_hist.val_loss, 'r', label='val loss')

acc_ax.plot(custom_hist.train_accuracy, 'b', label='train acc')
acc_ax.plot(custom_hist.val_accuracy, 'g', label='val acc')

loss_ax.set_xlabel('epoch')
loss_ax.set_ylabel('loss')
acc_ax.set_ylabel('accuracy')

loss_ax.legend(loc='upper left')
acc_ax.legend(loc='lower left')

plt.show()
```

- 训练监控结果曲线图

![](../../../../assets/images/dl-image-20220912110134769-070.webp)

&emsp;&emsp;这里我们可以看到，监控结果与 [0201 训练 MNIST 手写体数据集](../02-training/01-mnist.md) 所展示的示例基本一致。

上一节：[0202 TensorBoard 监控模型](./02-tensorboard.md)

下一节：[暂无]()
