---
layout: post
title: "Win10 Anaconda下TensorFlow-GPU2.0环境搭建"
header-style: text
tags:
  - Anaconda
  - Tensorflow
---
## 环境
- Windows10
- NVIDIA GeForce GTX 1050

## 1.安装CUDA Toolkit + cuDNN
①准备工作
- 确认显卡驱动
确认自己的显卡是否支持CUDA：https://developer.nvidia.com/

更新对应显卡的驱动：https://www.nvidia.co.jp/Download/index.aspx?lang=jp

- 确认tensorflow支持的版本
确认最新tensorflow支持的CUDA和cuDNN版本：https://www.tensorflow.org/install/gpu

②CUDA
下载CUDA：https://www.tensorflow.org/install/gpu

③cuDNN
下载cuDNN：https://developer.nvidia.com/rdp/cudnn-archive

zip压缩包解压，文件全部移到CUDA安装的根目录，如```C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.0\```

## 2.安装Anaconda
下载Anaconda：https://www.anaconda.com/download/

## 3.安装tensorflow-gpu
①创建conda环境
运行
```
conda create -n tensorflow-gpu
```

②激活conda环境
运行
```
activate tensorflow-gpu
```

③安装tensorflow-gpu
运行
```
pip install tensorflow-gpu
```
- 错误1 没有访问权限　使用 ```pip install tensorflow-gpu --user```
![](https://SkyoKen.github.io/post-images/1589951670985.png)
- 错误2 环境变数Path没有设定　将错误提示里的路径，加到Path即可
![](https://SkyoKen.github.io/post-images/1589951796808.png)


④安装Anaconda基础包
由于是新创建的conda环境，环境中基本上是空的，需要安装Anaconda基础包
```
conda install anaconda
```

## 4.测试
①确认是否tensorflow安装成功
```
python
>import tensorflow
```
无错误提示即安装成功

②查看是否使用GPU
运行
```
python
>import tensorflow as tf
>tf.test.gpu_device_name()
```
最后一行显示``` '' ``` 为只使用CPU
最后一行显示```'/device:GPU:0'``` 为使用GPU

③查看在使用哪个GPU
运行
```
python
>from tensorflow.python.client import device_lib
>device_lib.list_local_devices()
```
运行结果
```
[name: "/device:CPU:0"
device_type: "CPU"
memory_limit: 268435456
locality {
}
incarnation: 8418430128072869292
, name: "/device:XLA_CPU:0"
device_type: "XLA_CPU"
memory_limit: 17179869184
locality {
}

incarnation: 15942452823761207604
physical_device_desc: "device: XLA_CPU device"
, name: "/device:GPU:0"
device_type: "GPU"
memory_limit: 3140655513
locality {
  bus_id: 1
  links {
  }
}

incarnation: 17254723484844278288
physical_device_desc: "device: 0, name: GeForce GTX 1050, pci bus id: 0000:01:00.0, compute capability: 6.1"
, name: "/device:XLA_GPU:0"
device_type: "XLA_GPU"
memory_limit: 17179869184
locality {
}

incarnation: 10507266387241261141
physical_device_desc: "device: XLA_GPU device"
]
```
安装成功！开始瞎折腾！