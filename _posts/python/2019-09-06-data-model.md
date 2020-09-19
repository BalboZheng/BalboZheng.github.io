---
layout:     post
title:      "Python数据模型"
subtitle:   ""
date:       2019-09-06 12:30:11
author:     "Balbo"
header-img: "img/post-bg-2019.jpg"
catalog: true
tags:
    - python
---
## 特殊方法
特殊方法的存在是为了被Python解释器调用的，你自己并不需要调用他们。也就是说没有 my\_object.\_len\_ 这种写法，而应该使用 len(my\_objext)。

### 字符串表示形式
在_repr_的实现中，我们用到了%r来获取对象各个属性的标准字符串表示形式——这是个好习惯，他暗示了一个关键：Vector(1, 2)和Vector('1', '2')是不一样的，后者在我们的定义中会报错，因为向量对象的构造函数只接受数值，不接受字符串。

<font face="STCAIYUN" color=#00FFCC size=5> \_repr\_ 和 \_str\_ </font>的区别在于，后者是在str()函数被使用，或是在用print函数打印一个对象的时候才被调用的，并且塔返回的字符串对终端用户更友好。

如果你只是想实现这两个特殊方法中的一个，\_repr\_是更好的选择，因为如果一个对象没有\_str\_函数，二Python又需要调用他的时候，解释器会用_repe_作为替代。

参考：[Difference between _str_ and _repr_ in Python](http://stackoverflow.com/questions/1436703/difference-between-str-and-repe-in-python)

### 算术运算符
<font face="STCAIYUN" color=#00FFCC size=5> \_add\_ 和 \_mul\_ </font>为向量带来了 + 和 * 这两个算术运算符。值得注意的是，这两个方法的返回值都是新插件的向量对象，代码里只是读取了他们的值 (self, other) 而已。

### 自定义的布尔值
默认情况下，我们自己定义的累的实力总被认为是真的。

在Python标准库的文档中，有一节叫做[Built-in Types](http://docs.python.org/3/library/stdtypes.html#truth)，其中规定了真值检验的标准。通过实现\_bool\_，你定义的对象就可以与这个标准保持一致。

\* 如果想让Vector.\_bool\_更高效，可以采用这种实现：
```python
    def __bool__(self):
        return bool(self.x or self.y)
```
他不那么易读，却能省掉 abs 到 \_abs\_ 到平方再到平方根这些中间步骤。通过 bool 把返回类型显示装换为布尔值是为了符合 \_bool\_ 对返回值的规定，因为 or 运算符可能会返回 x 或者 y 本身的值：若 x 的值等价于真，则 or 返回的 x 的值，否则返回 y 的值。
***

## 特殊方法一览
| 类别  | 方法名 |
| ------------- | ------------- |
| 字符串/字节序列表示形式 | \_repr\_、 \_str\_、 \_format\_、 \_bytes\_ |
| 数值装换 | \_abs\_、 \_bool\_、 \_complex\_、 \_int\_、 \_float\_、 \_hash\_、 \_index\_ |
| 集合模拟 | \_len\_、 \_getitem\_、 \_setitem\_、 \_delitem\_、 \_contains\_ |
| 迭代枚举 | \_iter\_、 \_reversed\_、 \_next\_ |
| 可调用模拟 | \_call\_ |
| 上下文管理 | \_enter\_、 \_exit\_ |
| 实例插件和销毁 | \_new\_、 \_init\_、 \_del\_ |
| 属性管理 | \_getattr\_、 \_getattribute\_、 \_setattr\_、 \_delattr\_、 \_dir\_ |
| 属性描述符 | \_get\_、 \_set\_、 \_delete\_ |
| 跟类相关的服务 | \_prepare\_、 \_instancecheck\_、 \_subclasscheck\_ |


| 类别 | 方法名和对应的运算符 |
| ------------- | ------------- |
| 一元运算符 | \_neg\_ -、 \_pos\_ +、 \_abs\_ abs() |
| 众多比较运算符 | \_lt\_ <、 \_le\_ <=、 \_eq\_ ==、 \_ne\_ ！=、 \_gt\_ >、 \_ge\_ >= |
| 算术运算符 | \_add\_ +、 \_sub\_ -、 \_fmul\_ *、 \_truediv\_ /、 \_floordiv\_ //、 \_mod\_ %、 \_divmod\_ divmod()、 \_pow\_ **或pow()、 \_round\_ round() |
| 反向算术运算符 | \_radd\_、 \_rsub\_、 \_rmul\_、 \_rtruediv\_、 \_rfloordiv\_、 \_rmod\_、 \_rdivmod\_、 \_rpow\_ |
| 增量赋值算术运算符 | \_iadd\_、 \_isub\_、 \_imul\_、 \_itruediv\_、 \_ifoordiv\_、 \_imod\_、 \_ipow\_ |
| 位运算符 | \_invert\_ -、 \_lshift\_ <<、 \_rshift\_ >>、 \_and\_ &、 \_or\_ |、 \_xor\_ ^ |
| 反向位运算符 |  \_rlshift\_、 \_rrshift\_、 \_rand\_、 \_ror\_、 \_rxor\_ |
| 增量赋值位运算符 | \_ilshift\_、 \_irshift\_、 \_iand\_、 \_ior\_、 \_ixor\_ |