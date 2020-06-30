---
layout: post
title: '【推荐系统】协同过滤SVD,SVD++,NMF算法的使用'
author: "SKyoKen"
header-style: text
tags:
  - 推荐系统
  - deep learning
---

## 准备工作
```
pip install scikit-surprise
```

## 算法的使用

为了方便运行统一使用ml-100k数据

SVD算法（svd.py）
```python
from surprise import SVD
from surprise import Dataset
from surprise.model_selection import cross_validate


# Load the movielens-100k dataset (download it if needed),
data = Dataset.load_builtin('ml-100k')

# We'll use the famous SVD algorithm.
algo = SVD()

# Run 5-fold cross-validation and print results
cross_validate(algo, data, measures=['RMSE', 'MAE'], cv=5, verbose=True)
```

SVD++算法（svdpp.py）
```python
from surprise import SVDpp
from surprise import Dataset
from surprise.model_selection import cross_validate


# Load the movielens-100k dataset (download it if needed),
data = Dataset.load_builtin('ml-100k')

# We'll use the famous SVD++ algorithm.
algo = SVDpp()

# Run 5-fold cross-validation and print results
cross_validate(algo, data, measures=['RMSE', 'MAE'], cv=5, verbose=True)
```

NMF算法（NMF.py）

```python
from surprise import NMF
from surprise import Dataset
from surprise.model_selection import cross_validate


# Load the movielens-100k dataset (download it if needed),
data = Dataset.load_builtin('ml-100k')

# We'll use the famous NMF algorithm.
algo = NMF()

# Run 5-fold cross-validation and print results
cross_validate(algo, data, measures=['RMSE', 'MAE'], cv=5, verbose=True)
```

## 使用其他dataset
如SUSHI Preference Data Sets(sushi3‐2016.zip)

下载地址：http://www.kamishima.net/sushi/

首先先把数据处理成能用的

将`sushi3b.5000.10.score`处理成每行为`用户ID 物品ID 评分`的数据

```python
def convert(input_file_name):
    # 数据处理后保存的文件名
    output_file_name = input_file_name + '_converted'
    output = ''

    with open(input_file_name, mode='r') as f:
        lines = f.readlines()
        user_id = 0
        for line in lines:    #每行为一个用户
            words = line.strip().split(' ')
            for item_id, word in enumerate(words):
                score = int(word)
                if score != -1:    #保存评分不为-1的物品数据
                    output += '{0:04d} {1:02d} {2:01d}\n'.format(user_id, item_id, score)
            user_id += 1

    with open(output_file_name, mode='w') as f:
        f.write(output)

    return output_file_name

# 以「用户ID 物品ID 评分」格式处理数据
output_file_name = convert('sushi3-2016/sushi3b.5000.10.score')
```


读取处理好的数据（sushi3b.5000.10.score_converted）

在上边svd的基础上,将读取数据的部分替换为以下代码即可

```python
from surprise import Reader
reader = Reader(line_format='user item rating', sep=' ')
data = Dataset.load_from_file('./sushi3-2016/sushi3b.5000.10.score_converted', reader=reader)
```

即
```python
from surprise import SVD
from surprise import Reader, Dataset
from surprise.model_selection import cross_validate


#load data from path
reader = Reader(line_format='user item rating', sep=' ')
data = Dataset.load_from_file('./sushi3-2016/sushi3b.5000.10.score_converted', reader=reader)

# We'll use the famous SVD algorithm.
algo = SVD()

# Run 5-fold cross-validation and print results
cross_validate(algo, data, measures=['RMSE', 'MAE'], cv=5, verbose=True)
```