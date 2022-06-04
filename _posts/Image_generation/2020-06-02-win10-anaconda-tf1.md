---
layout: post
title: 'Win10 Anaconda下TensorFlow-GPU1.15环境搭建'
header-style: text
tags:
  - Anaconda
  - Tensorflow
---
>接上篇《Win10 Anaconda下TensorFlow-GPU2.0环境搭建》

实际运行网上的代码时发现，大多用的是tf1而不是tf2

好在anaconda创建环境特别方便，这里创建一个python3.6的环境（python3.7以上不支持tf1.15

## anaconda创建新环境

```
conda create -n tf1.15 python=3.6

conda activate tf1.15
```

## 安装tf1.15
```
pip install tensorflow-gpu==1.15
```

## 确认tf版本
```
python -c "import tensorflow as tf; print( tf.__version__ )"
```

出现问题
```
UserWarning: h5py is running against HDF5 1.10.5 when it was built against 1.10.4, this may cause problems
```
简单解决方法
```
pip uninstall h5py
pip install h5py
```