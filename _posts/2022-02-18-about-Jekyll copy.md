---
layout: post
title: "jekyll配置开发环境"
header-style: text
tags:
  - 环境搭建
---
# 环境搭建
## 1. windows
### ①安装ruby
[RubyInstaller](https://rubyinstaller.org/downloads/)

### ②安装jekyll

```shell
gem install jeykll
```

### ③快速开始

```shell
jeykll new helloworld
cd helloworld
jeykll serve
```

### ④预览
[http://127.0.0.1:4000](http://127.0.0.1:4000)


## 2. ubuntu

※当天搭建完之后忘记写下来了 之后再搭建的时候补上

#### memo：
比windows麻烦不少，直接```sudo apt update sudo apt install ruby-full```会出很多问题，似乎是因为不是最新版？

装好后，直接```jeykll serve```也有问题，我是切到root之后用```bundler exec jeykll serve```（可能是我哪步没弄好）

---
## 一些可能会用上的东西

##### 1. 如何让jekyll服务可以在局域网中访问？后边加上```--host=0.0.0.0```即可
```shell
jeykll serve --host=0.0.0.0
```
##### 2. 如何修改端口？后边加上```-P 端口```即可（如1234）
```shell
jeykll serve -P 1234
```
##### 3. 如何在设置中直接修改地址和端口？在`_config.yml`文件顶端写好host和port即可
```yml
title: example
email: example@example.com
description: example
baseurl: "" 
host: 0.0.0.0
port: 1234
```
##### 4. 如何在关闭命令行后还启动该服务？
```shell
jekyll serve --detach
```
##### 5. 如何停止该服务？

查了网上的资料，说是可以用```ps aux |grep jekyll |awk '{print $2}' | xargs kill -9```停止，但是我试了不行。

放置了几天想要再试试其他方法的时候，发现服务已经停止了，所以暂时没测试以下的方法hhhhh 改天试试

（以下未测试 不保证能用
```shell
pkill -f jekyll
```
或者
```shell
ps -ef | grep jekyll
kill -9 jekyll服务进程编号
```

---

现阶段是直接编辑服务器文件来更新的

打算之后试试自动部署（用github）

[之后要参考的资料](http://huyan.couplecoders.tech/%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA/blog/jekyll/2018/09/05/%E4%BD%BF%E7%94%A8jekyll%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)