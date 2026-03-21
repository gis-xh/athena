# 迁移学习



## 环境配置

> 创建虚拟环境

```sh
conda create -n krs01 python=3.9 -y
```

> 激活虚拟环境

```sh
conda activate krs01
```

> 安装 tensorflow 和 tensorflow 数据集

```sh
conda install -c conda-forge tensorflow==2.10 tensorflow-datasets -y
```

> 安装 cuda 和 cudnn

```sh
conda install -c conda-forge cudatoolkit=11.2 cudnn=8.1 -y
```

> 安装 JupyterLab、matplotlib、Pillow

```sh
conda install -c conda-forge jupyterlab matplotlib Pillow -y
```

> 移除虚拟环境

```sh
conda remove -n krs01 --all
```





## 迁移学习介绍

在迁移学习中需要有些不被使用的层被冻结

使用已有模型帮助完成任务，删掉、冻结某些层、添加自定义层实现新的模型

参考：https://keras.io/guides/transfer_learning/



## 论文分享

迁移学习

全卷积网络进行地物分类

### 研究现状

已有产品，但是没有对应的影像

### 相关方法

知识蒸馏
