---
layout: post
title: '【推荐系统】树莓派3b+上安装scikit-surprise库时出现了scipy无法正常安装的问题'
header-style: text
tags:
  - 推荐系统
  - Raspberry Pi
---
```
sudo pip3 install scikit-surprise
```
提示scipy无法正常安装


`sudo pip3 install scipy`
⇒ 提示安装失败

`sudo apt-get install python3-scipy`
⇒ 安装成功！