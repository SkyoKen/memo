---
layout: post
title: "在win10+RTX30系列显卡下使用StyleGAN2"
header-style: text
date: 2022-06-12
tags:
  - docker
  - Tensorflow
  - StyleGAN2
  - win10
  - RTX30
---
>入手了个RTX 3070，发现按照之前的环境来搭建无法使用

- 2022.06.07

	RTX30系列最低要求CUDA版本为11……也就是说这种情况下tensorflow只能用2.4以上的版本 [（参考）](https://blog.csdn.net/weixin_51557309/article/details/121939430)


- 2022.06.08

	得知nvidia的docker官方镜像的tf1.14支持CUDA11 [（参考）](https://github.com/NVlabs/stylegan2-ada/issues/10)

## 目录
1. [安装WSL2](#安装WSL2)  
2. [安装docker](#安装docker) 
	- 下载docker镜像(tensorflow+gpu)
	- 容器生成
	- 确认GPU是否可以正常使用
3. [StyleGAN2](#StyleGAN2)
4. [可能会用到的东西](#可能会用到的东西)
	- jupyter
	- StyleGAN2可能要安装的库
	- 可能用得到的命令


## [安装WSL2](https://docs.microsoft.com/zh-tw/windows/wsl/install-manual)

## [安装docker](https://www.docker.com/)

①下载docker镜像(tensorflow+gpu)

~~docker pull tensorflow/tensorflow:1.15.2-gpu-jupyter (ubuntu+GTX 1080Ti时用的)~~
```
docker pull nvcr.io/nvidia/tensorflow:20.10-tf1-py3

```

②容器生成

~~docker run -it --gpus all --name StyleGAN2 -p 8888:8888 -p 200:200 tensorflow/tensorflow:1.15.2-gpu-jupyter /bin/bash~~
```
docker run -it --gpus all --name StyleGAN2 -p 8888:8888 -p 200:200 nvcr.io/nvidia/tensorflow:20.10-tf1-py3 /bin/bash
```
进入容器后会出现以下信息，据说直接无视即可
```
WARNING: The NVIDIA Driver was not detected.  GPU functionality will not be available.
   Use 'nvidia-docker run' to start this container; see
   https://github.com/NVIDIA/nvidia-docker/wiki/nvidia-docker .

   nvidia-docker run --shm-size=1g --ulimit memlock=-1 --ulimit stack=67108864
```
➂确认GPU是否可以正常使用
左转之前的[tf-gpu环境搭建](./2020-06-02-win10-anaconda-tf1.md)

## StyleGAN2
①克隆StyleGAN2的库
```
git clone https://github.com/NVlabs/stylegan2.git
cd stylegan2
pip install pillow requests pandas 
```
②②运行Stylegan2的图片生成进行测试
```
python run_generator.py generate-images --network=gdrive:networks/stylegan2-ffhq-config-f.pkl  --seeds=6600-6601 --truncation-psi=0.5
```


## 可能会用到的东西


```
#启动jupyter
nohup jupyter notebook --ip 0.0.0.0 --allow-root --port 8888 --no-browser &

#用浏览器打开jupyter
	ip:8888/_token=???????????????
※token: 用`cat nohup.out`查看


#可能要安装的库
python -m pip install pip==21.0.1
apt install libsm6 libxrender1 libxext-dev
apt install git python3-pip wget
pip3 install keras==2.3.1 matplotlib scikit-learn pillow
pip3 install requests pandas


#可能用得到的命令
docker ps								#查看已启动的容器
docker ps -a						   	#查看全部的容器
docker start 容器ID/容器名				#启动容器
docker stop 容器ID/容器名				#停止容器
docker attach 容器ID/容器名				#进入容器，退出后运行停止
docker exec -it 容器ID/容器名 /bin/bash #进入容器，退出后仍在后台运行
docker rm 容器ID/容器名 					#删除容器

nvidia-smi								#查看GPU

#复制文件 linux——容器
docker cp 容器ID/容器名:容器里的路径 保存路径		#容器——》linux
docker cp 文件路径 容器ID/容器名:容器里的路径		#linux——》容器
```

参考资料

[1][Tensorflow(GPU) Docker環境構築](https://www.tensorflow.org/install/docker#gpu_support)

[2][如何构建包含TensorFlow/Python3/Jupyter的Docker](https://zhuanlan.zhihu.com/p/66278558)
