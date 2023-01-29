---
layout: post
title: 市场菜篮子分析
subtitle: 
author: Balbo Cheng
categories: programming-language
tags: [python, pandas, matplotlib, sklearn]
sidebar: []
---

[数据来源](https://download.csdn.net/download/weixin_38547397/16210808?utm_medium=distribute.pc_relevant_download.none-task-download-2~default~BlogCommendFromBaidu~Rate-3.dl_default&depth_1-utm_source=distribute.pc_relevant_download.none-task-download-2~default~BlogCommendFromBaidu~Rate-3.dl_default&dest=https%3A%2F%2Fdownload.csdn.net%2Fdownload%2Fweixin_38547397%2F16210808&spm=1003.2020.3001.6616.3)

```python
"""
1. 获取数据集
2. 数据基本处理
3. 特征工程
4. 机器学习
5. 模型评估
"""
import pandas as pd
from sklearn.decomposition import PCA
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score

aisles = pd.read_csv('market-basket/aisles.csv')
order_products__prior = pd.read_csv('market-basket/order_products__prior.csv')
orders = pd.read_csv('market-basket/orders.csv')
products = pd.read_csv('market-basket/products.csv')

# 合并表格
table1 = pd.merge(order_products__prior, products, on=['product_id', 'product_id'])
table2 = pd.merge(table1, orders, on=['order_id', 'order_id'])
table = pd.merge(table2, aisles, on=['aisle_id', 'aisle_id'])

# 交叉表合并
table = pd.crosstab(table['user_id'], table['aisle'])

# 数据截取
new_table = table[:1000]

# 特征工程
transfer = PCA(n_components=0.9)
trans_data = transfer.fit_transform(new_table)

# 机器学习
estimator = KMeans(n_clusters=5)
y_pre = estimator.fit_predict(trans_data)

# 模型评估
ret = silhouette_score(trans_data, y_pre)
print(ret)
```