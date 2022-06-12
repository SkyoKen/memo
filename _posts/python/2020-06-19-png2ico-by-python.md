---
layout: post
title: "用python把png转成ico"
author: "SKyoKen"
header-style: text
tags:
  - 图片处理
  - python
---
## 方法一
```Python
from PIL import Image
filename = r'logo.png'
img = Image.open(filename)
img.save('logo.ico')
```
## 方法二
```Python
import imageio

img = imageio.imread('logo.png')
imageio.imwrite('logo.ico', img)
```