<<<<<<< HEAD
<<<<<<< HEAD:_posts/2019-10-05-python-mapping.md
---
layout:     post
title:      "不可变映射类型"
subtitle:   "什么是不可变映射类型"
date:       2019-10-05 16:45:30
author:     "Balbo"
header-img: "img/post-bg-2019.jpg"
tags:
    - python
=======
---
layout: post
title: 不可变映射类型
subtitle: 什么是不可变映射类型
author: Balbo Cheng
categories: python
tags: []
sidebar: []
>>>>>>> e5c77ca (新页面)
---
从 Python3.3 开始，types 模块中引入了一个封装类名叫 MappingProxyType。如果给这个类一个映射，他会返回一个只读的映射视图。虽然是个只读视图，但是他是动态的。这意味着如果对元映射做出了改动，我们通过这个视图可以观察到，但是无法通过这个视图对原映射做出修改。
```python
from types import MappingProxyType

d = {1: 'A'}
d_proxy = MappingProxyType(d)
print(d_proxy)
print(d_proxy[1])
d[2] = 'B'
print(d_proxy[2])
d_proxy[2] = 'C'
```
运行结果：
```
{1: 'A'}
A
B
Traceback (most recent call last):
  File "E:/FP/Topic_2/Section_2/read.py", line 9, in <module>
    d_proxy[2] = 'C'
TypeError: 'mappingproxy' object does not support item assignment

<<<<<<< HEAD
```
=======
---
layout:     post
title:      "不可变映射类型"
subtitle:   "什么是不可变映射类型"
date:       2019-10-05 16:45:30
author:     "Balbo"
header-img: "img/post-bg-2019.jpg"
catalog: true
tags:
    - python
---
从 Python3.3 开始，types 模块中引入了一个封装类名叫 MappingProxyType。如果给这个类一个映射，他会返回一个只读的映射视图。虽然是个只读视图，但是他是动态的。这意味着如果对元映射做出了改动，我们通过这个视图可以观察到，但是无法通过这个视图对原映射做出修改。
```python
from types import MappingProxyType

d = {1: 'A'}
d_proxy = MappingProxyType(d)
print(d_proxy)
print(d_proxy[1])
d[2] = 'B'
print(d_proxy[2])
d_proxy[2] = 'C'
```
运行结果：
```
{1: 'A'}
A
B
Traceback (most recent call last):
  File "E:/FP/Topic_2/Section_2/read.py", line 9, in <module>
    d_proxy[2] = 'C'
TypeError: 'mappingproxy' object does not support item assignment

```
>>>>>>> f0bfe13 (new update):_posts/知识点/2019-10-05-python-mapping.md
=======
```
>>>>>>> e5c77ca (新页面)
