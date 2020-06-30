---
layout: post
title: '【推荐系统】用Flask搭建一个简单的推荐系统'
author: "SKyoKen"
header-style: text
tags:
  - 推荐系统
  - deep learning
---
## 前期准备
```
pip install scikit-surprise
```
[树莓派上安装库时出现了scipy无法安装问题](/2020/06/30/recommendation-raspi-error/)
## 代码准备
reommender.py
```python
from collections import defaultdict

from surprise import SVD
from surprise import Dataset

import json

def get_top_n(predictions, n=10):
    '''
    根据预测集向每个用户返回前N条推荐
    '''

    # 预测集
    top_n = defaultdict(list)
    for uid, iid, true_r, est, _ in predictions:
        top_n[uid].append((iid, est))

    # 每个用户返回前N条推荐
    for uid, user_ratings in top_n.items():
        user_ratings.sort(key=lambda x: x[1], reverse=True)
        top_n[uid] = user_ratings[:n]

    return top_n


# 用SVD算法学习movielens数据
data = Dataset.load_builtin('ml-100k')
trainset = data.build_full_trainset()
algo = SVD()
algo.fit(trainset)

# 预测推荐结果
testset = trainset.build_anti_testset()
predictions = algo.test(testset)

top_n = get_top_n(predictions, n=10)

# 向各个用户显示推荐项目
for uid, user_ratings in top_n.items():
    print(uid, [iid for (iid, _) in user_ratings])

# 结果以json形式保存
with open('./results.json', 'w') as f:
    json.dump(top_n, f, indent=2, ensure_ascii=False)
```
api.py
```
import json

import flask
from flask import request, jsonify

app = flask.Flask(__name__)
app.config["DEBUG"] = True
app.config['JSON_AS_ASCII'] = False

# 读取数据
with open('./results.json') as f:
    movies = json.loads(f.read())

# 首页
# http://127.0.0.1:5000/

@app.route('/', methods=['GET'])
def home():
    return '''<h1>简单的推荐系统</h1>
<p>电影推荐</p>'''

# 全结果
# http://127.0.0.1:5000/api/v1/resources/movies/all

@app.route('/api/v1/resources/movies/all', methods=['GET'])
def api_all():
    return jsonify(movies)

# 特定用户的推荐
# http://127.0.0.1:5000/api/v1/resources/movies?id=1

@app.route('/api/v1/resources/movies', methods=['GET'])
def api_id():
    # 确认URL里有没有包含ID
    # 含有ID的话，往变数id里带入
    # 不含有ID的话，错误显示
    if 'id' in request.args:
        id = str(request.args['id'])
    else:
        return "エラー: URL中不包含ID，请指定ID"

    items = movies.get(id)

    # 空的list
    results = []

    # 往list添加推荐的电影
    for i in range(len(items)):
        item = items[i][0]
        results.append(item)

    # 把Python的list转为JSON格式
    return jsonify(results)

app.run()
```

## 搭建
1.运行`python reommender.py`生成result.json

2.运行`python api.py`启动

3.打开浏览器输入`http://127.0.0.1:5000/`

`http://127.0.0.1:5000/`
⇒ 首页

`http://127.0.0.1:5000/api/v1/resources/movies/all`
⇒ 全结果

`http://127.0.0.1:5000/api/v1/resources/movies?id=1`
⇒ 对用户ID为1的推荐结果