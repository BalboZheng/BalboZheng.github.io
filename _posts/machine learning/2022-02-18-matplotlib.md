---
layout:     post
title:      "Matplolib"
subtitle:   ""
date:       2022-02-18 18:04:11
author:     "Balbo"
header-img: "img/post-bg-2022.jpg"
catalog: true
tags:
    - python
---

## 实现一个简单的Matplolib画图

```python
import matplotlib.pyplot as plt

# 创建画布
plt.figure(figsize=(20, 8), dpi=70)

# 绘制画布
x = [1, 2, 3]
y = [4, 5, 6]
plt.plot(x, y)

# 显示图像
plt.show()

```

## Matplolib三层结构

**容器层： 主要由Canvas、Figure、Axes组成。**

Canvas是位于最底层的系统层，在绘图的过程中充当画板的角色，即放置画布(Figure)的工具。

Figure是Canvas上方的第一层，也是需要用户来操作的应用层的第一层，在绘图的过程中充当画布的角色。

Axes是应用层的第二层，在绘图的过程中相当于画布上的绘图区的角色。

- Figure:指整个图形(可以通过plt.figure()设置画布的大小和分辨率等)

- Axes(坐标系):数据的绘图区域

- Axis(坐标轴)：坐标系中的一条轴，包含大小限制、刻度和刻度标签

注意点： x

一个figure(画布)可以包含多个axes(坐标系/绘图区)，但是一个axes只能属于一个figure。

一个axes(坐标系/绘图区)可以包含多个axis(坐标轴)，包含两个即为2d坐标系，3个即为3d坐标系 。

**辅助显示层**

​	辅助显示层为Axes(绘图区)内的除了根据数据绘制出的图像以外的内容，主要包括Axes外观(facecolor)、边框线(spines)、坐标轴(axis)、坐标轴名称(axis label)、坐标轴刻度(tick)、坐标轴刻度标签(tick label)、网格线(grid)、图例(legend)、标题(title)等内容。

​	该层的设置可使图像显示更加直观更加容易被用户理解，但又不会对图像产生实质的影响。

**图像层**

​	图像层指Axes内通过plot、scatter、bar、histogram、pie等函数根据数据绘制出的图像。

> Canvas（画板）位于最底层，用户一般接触不到
> Figure（画布）建立在Canvas之上
> Axes（绘图区）建立在Figure之上
> 坐标轴（axis）、图例（legend）等辅助显示层以及图像层都是建立在Axes之上

## 基础绘图功能

### 图像保存

```python
plt.savefig(path)
```

### 添加x轴，y轴刻度

```python
plt.xticks(ticks, label)
plt.yticks(ticks, label)
# 第一个参数必须是数字，如果不是数字，需要进行替换
```

### 添加网络

```python
plt.grid(True, linestyle='--', alpha='1')
```

### 添加描述信息

```python
# 纵轴标签
plt.xlabel("")
# 横轴标签
plt.ylabel("")
# 标题
plt.title("", fontsize="20")
```

### 同一坐标系多条数据

```python
# 绘制折线图
plt.plot(x, y_1, label="1")
# 画出多条折线
plt.plot(x, y_2, label="2")

# 显示图例
plt.legend(loc="location string")
```

### 图形风格

| 颜色字符 | 风格字符  | Location String | Location Code |
| :------: | :-------: | --------------- | ------------- |
|  r 红色  |  - 实线   | best            | 0             |
|  g 绿色  |  -- 虚线  | upper right     | 1             |
|  b 蓝色  | -. 点划线 | upper left      | 2             |
|  w 白色  | ： 点虚线 | lower left      | 3             |
|  c 青色  | ‘ ‘ 留空  | lower right     | 4             |
|  m 洋红  |           | right           | 5             |
|  y 黄色  |           | center left     | 6             |
|  k 黑色  |           | center right    | 7             |
|          |           | lower center    | 8             |
|          |           | upper center    | 9             |
|          |           | center          | 10            |

### 多个坐标系显示

```python
import matplotlib.pyplot as plt

# 创建画布
fig, axes = plt.subplots(nrows=1, nclos=2, figsize=(20, 8), dpi='100')

# 绘制画布
axes[0].plot(x, y_1, label='1')
axes[1].plot(x, y_2, label='2')

axes[0].set_xticks(x[::5])
axes[0].set_yticks(y[::5])
axes[0].set_xticklabels(x_ticks_label[::5])

# 添加网格
axes[0].grid(True, linestyle='--', alpha='1')

# 显示图例
axes[0].legend(loc=0)
axes[1].legend(loc=0)

# 显示图像
plt.show()

```

## 常见图形绘制

```python
# 折线图
plt.plot(x, y)

# 散点图
plt.scatter(x, y)

# 柱状图
# width 宽度
# align 位置对齐方式(center/edge)
# **kwargs 颜色
plt.bar(x, width, align='center', **kwargs)

# 直方图
# bins 组距
plt.hist(x, bins=None)

# 饼图
# labels 每部分名称
# autopct 占比显示指定%1.2f%
# colors 颜色
plt.pie(x, labels=, autopct=, colors=)

```

