---
layout:     post
title:      "鸢尾花预测"
subtitle:   ""
date:       2022-03-08 17:25:11
author:     "Balbo"
header-img: "img/post-bg-2022.png"
catalog: true
tags:
    - python
	- kNN
---

## 获取数据集

- 小数据集：sklearn.datasets.load.*
- 大数据集：sklearn.datasets.fetch.*

```python
from sklearn.datasets import load_iris, fetch_20newsgroups

# 数据集收集
# 小数据集
iris = load_iris()
# print(iris)

# 大数据集
# data_home 保存位置
# subset train/text/all
groups = fetch_20newsgroups(data_home=None, subset='train')
# print(groups)
```

## 数据集属性描述

```python
# 返回值是一个“字典bunch”
print(iris)
print("数据集中特征值是：\n", iris.data)
print("数据集中目标值是：\n", iris['target'])
print("数据集中特征值名字是：\n", iris.feature_names)
print("数据集中目标值是名字：\n", iris.target_names)
print("数据集描述是：\n", iris.DESCR)
```

## 数据可视化

```python
from sklearn.datasets import load_iris
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd

iris = load_iris()
# 不能正常显示中文时加上一下两行
plt.rcParams['font.sans-serif'] = ['SimHei']  # 显示中文标签
plt.rcParams['axes.unicode_minus'] = False  # 这两行需要手动设置

iris_d = pd.DataFrame(iris['data'],
                      columns=['sepal length (cm)', 'sepal width (cm)', 'petal length (cm)', 'petal width (cm)'])
iris_d['species'] = iris.target
print(iris['target'])


def plot_iris(data, col1, col2):
    # hue 目标值是什么
    # fit_reg 是否进行线性拟合
    sns.lmplot(x=col1, y=col2, data=data, hue='species', fit_reg=False)
    plt.xlabel(col1)
    plt.ylabel(col2)
    plt.title("鸢尾花种类分布图")
    plt.show()


plot_iris(iris_d, 'sepal length (cm)', 'sepal width (cm)')

```

## 数据集的划分

机器学习一般会把数据集划分为两部分

- 训练数据：用于训练，构建模型
- 测试数据：在模型检验时使用，用于评估模型是否有效

数据划分比例一般有以下几种：7：3、8：2、7.5：2.5

```python
# x 数据集的特征值
# y 数据集的标签值
# test_size 测试集的大小，一般为float
# random_tate 随机数种子
# return 训练集特征值、测试集特征值、训练集目标值、测试集目标值
x_train, x_test, y_train, y_test = train_test_split(iris.data, iris.target, random_state=22)
print("训练集特征值：\n", x_train)
print("测试集特征值：\n", x_test)
```

## 完整代码

```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.neighbors import KNeighborsClassifier

# 获取数据集
iris = load_iris()

# 数据基本处理
# 数据分割
x_train, x_test, y_train, y_test = train_test_split(iris.data, iris.target, random_state=22, test_size=0.3)

# 特征工程
# 实例化一个转化器
transfer = StandardScaler()
# 调用fit.transform函数
x_train = transfer.fit_transform(x_train)
x_test = transfer.fit_transform(x_test)

# 机器学习
# 实例化一个估计器
estimator = KNeighborsClassifier(n_neighbors=5)
# 模型训练
estimator.fit(x_train, y_train)

# 模型评估
# 输出预测值
y_pre = estimator.predict(x_test)
print("预测值是：\n", y_pre)
# print("真实值是：\n", y_test)
# print("真实值和预测值对比：\n", y_test == y_pre)
# 输出准确率
ret = estimator.score(x_test, y_test)
print("准确率为：\n", ret)

```

