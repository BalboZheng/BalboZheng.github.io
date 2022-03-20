---
layout:     post
title:      "波士顿房价预测"
subtitle:   ""
date:       2022-03-12 23:22:11
author:     "Balbo"
header-img: "img/post-bg-2022.png"
catalog: true
tags:
    - python
---

## 分析

回归当中的数据大小不一，是否会导致结果影响较大，需做标准化处理

- 数据分割与标准化处理
- 回归预测
- 线性回归的算法效果评估

## 回归性能评估

均方误差：
$$
MSE=\frac1m\sum_i^m=1(y^i-\bar{y})^2（y为预测值，\bar{y}为真实值）
$$

```python
from sklearn.metrics import mean_squared_error

# y_true 真实值
# y_pred 预测值
# return 浮点数
mse = mean_squared_error(y_true=, y_pred=)
```

## 代码

```python
"""
1. 获取数据集
2. 数据基本处理
3. 特征工程
4. 机器学习
5. 模型评估
"""
from sklearn.datasets import load_boston
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LinearRegression, SGDRegressor, Ridge, RidgeCV
from sklearn.metrics import mean_squared_error


def linear_model1():
    """
    回归方程：正规方程
    :return: None
    """
    data = load_boston()
    x_train, x_test, y_train, y_test = train_test_split(data.data, data.target, test_size=0.2)

    transfer = StandardScaler()
    x_train = transfer.fit_transform(x_train)
    x_test = transfer.fit_transform(x_test)

    estimator = LinearRegression()
    estimator.fit(x_train, y_train)

    y_pre = estimator.predict(x_test)
    score = estimator.score(x_test, y_test)
    ret = mean_squared_error(y_test, y_pre)

    print("---正规方程---")
    print("预测值为：\n", y_pre)
    print("准确率为：\n", score)
    print("均方误差：\n", ret)


def linear_model2():
    """
    线性回归：梯度下降法
    :return:
    """
    data = load_boston()
    x_train, x_test, y_train, y_test = train_test_split(data.data, data.target, test_size=0.2)

    transfer = StandardScaler()
    x_train = transfer.fit_transform(x_train)
    x_test = transfer.fit_transform(x_test)

    estimator = SGDRegressor(max_iter=1000)
    estimator.fit(x_train, y_train)

    y_pre = estimator.predict(x_test)
    score = estimator.score(x_test, y_test)
    ret = mean_squared_error(y_test, y_pre)

    print("---梯度下降法---")
    print("预测值为：\n", y_pre)
    print("准确率为：\n", score)
    print("均方误差：\n", ret)


def linear_model3():
    """
    线性回归：岭回归
    :return:
    """
    data = load_boston()
    x_train, x_test, y_train, y_test = train_test_split(data.data, data.target, test_size=0.2)

    transfer = StandardScaler()
    x_train = transfer.fit_transform(x_train)
    x_test = transfer.fit_transform(x_test)

    # alpha 正则化力度
    # solver 会根据数据自动选择方法 sag：如果数据集、特征都比较大，选择改随机梯度下降优化
    # normalize 是否标准化
    # estimator = Ridge(alpha=1.0)
    estimator = RidgeCV(alphas=(0.1, 1, 10))
    estimator.fit(x_train, y_train)

    y_pre = estimator.predict(x_test)
    score = estimator.score(x_test, y_test)
    ret = mean_squared_error(y_test, y_pre)

    print("---岭回归---")
    print("预测值为：\n", y_pre)
    print("准确率为：\n", score)
    print("均方误差：\n", ret)


if __name__ == '__main__':
    linear_model1()
    linear_model2()
    linear_model3()


```

