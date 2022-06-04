---
layout: post
title: "批量修改json文件"
header-style: text
tags:
  - json
  - python
---

```python
import os
import json

path = './json/'

for _ in os.listdir(path):
    fullpath = path + _
    data = {}
    with open(fullpath, "r") as f:
        data = json.load(f)
        # example
        data["name"] = ''
        data["description"] = ''
        data["image"]= ''
    with open(fullpath, "w") as f:
        json.dump(data, f)

```
