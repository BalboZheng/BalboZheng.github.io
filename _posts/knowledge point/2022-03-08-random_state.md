---
layout:     post
title:      "为什么需要参数random_state？"
subtitle:   ""
date:       2022-03-08 17:25:11
author:     "Balbo"
header-img: "img/post-bg-2022.png"
catalog: true
tags:
    - python
---

在此先简单罗列三种情况：

- 在构建模型时：

  ```python
  forest = RandomForestClassifier(n_estimators=100, random_state=0)
  forest.fit(X_train, y_train)
  ```

- 在生成数据集时：

  ```python
  x, y = make_moons(n_samples=100, noise=0.25, random_state=3)
  ```

- 在拆分数据集为训练集、测试集时：

  ```python
  X_train, X_test, y_train, y_test = train_test_split(
  cancer.data, cancer.target, stratify=cancer.target, random_state=42)
  ```

如果不设置random_state的话会怎样？
例如1中，每次构建的模型是不同的。
例如2中，每次生成的数据集是不同的。
例如3中，每次拆分出的训练集、测试集是不同的。

之所以会这样，是因为模型的构建、数据集的生成、数据集的拆分都是一个随机的过程。

如果你希望结果可以重现，固定random_state是非常重要的。

对于随机森林这个模型，它本质上是随机的，设置不同的随机状态（或者不设置random_state参数）可以彻底改变构建的模型。
对于数据集的生成，它本质上也是随机的，设置不同的随机状态（或者不设置random_state参数）可以彻底改变生成的数据集。
对于数据集的拆分，它本质上也是随机的，设置不同的随机状态（或者不设置random_state参数）可以彻底改变拆分的结果。

固定random_state后，每次构建的模型是相同的、生成的数据集是相同的、每次的拆分结果也是相同的。

## 在sklearn KFold()中：

```python
# n_splits 表示划分为几块（至少是2）
# shuffle 表示是否打乱划分，默认False，即不打乱
# random_state 表示是否固定随机起点，Used when shuffle == True.
# random_state : int, RandomState instance or None, optional, default=None        
KFold(n_splits=’warn’, shuffle=False, random_state=None)
```

> If int, random_state is the seed used by the random number generator;        
> If Random_State instance, random_state is the random number generator;        
> If None, the random number generator is the Random_State instance used by `np.random`.
> Only used when ``shuffle`` is True. This should be left  to None if ``shuffle`` is False.

## 在sklearn的train_test_split中：

我们在使用sklearn的train_test_split函数随机划分数据集生成training set 于test set时，在函数train_test_split中有一个参数为random_state。

**random_state**：随机数种子——其实就是该组随机数的编号，在需要重复试验的时候，保证得到一组一样的随机数。比如每次都为1，其他参数一样的情况下你得到的随机数组是一样的。当为None时，产生的随机数组也会是随机的。

随机数的产生取决于种子，随机数和种子之间的关系遵从以下两个规则：种子不同，产生不同的随机数；种子相同，即使实例不同也产生相同的随机数。

**test_size**：如果是浮点数，在0-1之间，表示test set的样本占比；如果是整数的话就表示test set样本数量。

test_size只是确定training set于test set的各自所占比例或者数量，并没有确定数据的划分规则。比如我们有数据集[1,2,3,4,5,6,7,8,9],我们确定test_size=3,那问题是我们应该取哪三个数作为test set呢，这时候就应该使用random_state来确定我们的划分规则，假设我们取random_state=1，它按一定的规则去取出我们的数据，当我们random_state=2时，它又换成另一种规则去取我们的数据，random_state的取值范围为0-2^32。当random_state=None时，可以理解为随机分配一个整数给random_state，这样就导致每次运行的结果都可能不同。

其他函数中的random_state参数功能是类似的。