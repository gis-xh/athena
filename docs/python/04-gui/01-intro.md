# 0401 PyQt5 开发环境配置

## 一、参考文章

1. [conda 克隆环境及导入新环境 / conda 环境移植](https://blog.csdn.net/l8947943/article/details/103179274)
2. [Anaconda3 导入flow环境报错](https://blog.csdn.net/tonighthack/article/details/117703473)

## 二、配置虚拟环境

【注意】：

1. 此处默认已经安装 Anaconda / Miniconda，如未安装，请参考 [Win11 安装配置 Miniconda 全过程记录](../../setup/dev/python-conda-setup.md)
2. 以下命令均是在终端命令行中输入。

### 2.1 创建新的环境

> 创建 python3.7 的虚拟环境

```sh
conda create -n byhy_qt python=3.7
```

> 激活环境

```sh
conda activate byhy_qt
```

> 使用清华源下载安装 PyQt5

```sh
pip install pyqt5-tools -i https://pypi.tuna.tsinghua.edu.cn/simple
```

### 2.2 克隆项目已有环境

&emsp;&emsp;在项目开发中，为了防止重复配置环境，通常 `conda` 的克隆命令，直接复刻已有环境。

```sh
conda env create -n cevo_qt -f conda.yaml
```

- `conda env create` ：无须修改
- `-n` 后的参数：创建名为 `cevo_qt` 的虚拟环境
- `-f` 后的参数：需要复制的 yaml 环境内容

## 三、示例代码

```sh
# 若使用的是 PySide2 直接替换掉 PyQt5 即可正常使用
from PyQt5.QtWidgets import QApplication, QMainWindow, QPushButton,  QPlainTextEdit

app = QApplication([])

# 主窗口
window = QMainWindow()
window.resize(500, 400)
window.move(300, 310)
window.setWindowTitle('薪资统计')

# 文本输入框
textEdit = QPlainTextEdit(window)
textEdit.setPlaceholderText("请输入薪资表")
textEdit.move(10, 25)
textEdit.resize(300, 350)

# 按钮控件
button = QPushButton('统计', window)
button.move(380, 80)

# 将空间全部显示在界面上
window.show()

# 系统进入事件循环运行
app.exec_()

# 定义按钮函数


def handleCalc():
    print('统计按钮被点击了')


# 每次按钮时运行
button.clicked.connect(handleCalc)
```
