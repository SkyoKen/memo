---
layout: post
title: "批量叠加png加字并保存json文件"
header-style: text
tags:
  - 图像处理
  - json
  - python
---

提前在psd文件里分好组，改好分组名，一键输出！

### 安装
```cmd
pip install psd-tools
```

### 分组输出png文件
```python
from psd_tools import PSDImage

#打开psd文件
psd = PSDImage.open('./sample.psd')
#整体输出
psd.composite().save('./sample.png')

for layer in psd:
    print(layer)
    # 以文件大小输出（同尺寸）
    layer_image = layer.composite(viewport=psd.viewbox) 
    # 以图层为文件名输出png图片
    layer_image.save('%s.png' % layer.name)
```

## 参考
CSDN说明资料 [[python库]psd文件操作库--psd_tools](https://blog.csdn.net/qq_33337811/article/details/103036113)