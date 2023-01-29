---
layout: post
title: 癌症分类预测
subtitle: 
author: Balbo Cheng
categories: programming-language
tags: [python, pandas, numpy, matplotlib, sklearn]
sidebar: []
---

## 方法

数据来源：https://archive.ics.uci.edu/ml/machine-learning-databases/breast-cancer-wisconsin/breast-cancer-wisconsin.data

```python
from sklearn.linear_model import LogisticRegression, LogisticRegressionCV

# solver ('liblinear', 'sag', 'saga', 'newton-cg', 'lbfgs')
# penalty 正则化种类
# C 正则化力度
estimator = LogisticRegression(solver='liblinear', penalty='12', C=1.0)
```

## 完整代码

```python
"""
1. 获取数据集
2. 数据基本处理
3. 特征工程
4. 机器学习
5. 模型评估
"""
import numpy as np
import pandas as pd
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler

# 获取数据
names = 'Sample code number', 'Clump Thickness', 'Uniformity of Cell Size', 'Uniformity of Cell Shape', 'Marginal Adhesion', 'Single Epithelial Cell Size', 'Bare Nuclei', 'Bland Chromatin', 'Normal Nucleoli', 'Mitoses', 'Class'
filePath = "breast-cancer-wisconsin.data"
data = pd.read_csv(filePath, names=names)

# 数据处理
# 缺失值处理
data = data.replace(to_replace="?", value=np.NAN)
data = data.dropna()
print(data)
# 确定特征值、目标值
x = data.iloc[:, 1:10]
y = data["Class"]
# 数据分割
x_train, x_test, y_train, y_test = train_test_split(x, y, random_state=22, test_size=0.2)

# 特征工程
transfer = StandardScaler()
x_train = transfer.fit_transform(x_train)
x_test = transfer.fit_transform(x_test)

# 机器学习
estimator = LogisticRegression()
estimator.fit(x_train, y_train)

# 模型评估
y_pre = estimator.predict(x_test)
score = estimator.score(x_test, y_test)
ret = mean_squared_error(y_test, y_pre)

print("---逻辑回归--")
print("预测值为：\n", y_pre)
print("准确率为：\n", score)
print("均方误差：\n", ret)
```
