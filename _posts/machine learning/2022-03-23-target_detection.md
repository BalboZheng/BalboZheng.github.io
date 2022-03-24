---
layout: post
title: 目标检测
subtitle: 
author: Balbo Cheng
categories: python
tags: [pytorch]
---

## 目标检测数据集

### 常见数据集

#### VOC数据集

官网：http://host.robots.ox.ac.uk/pascal/VOC/

常用：VOC2012/VOC2007

![](https://s2.loli.net/2022/03/23/9C7jWrtg3RJZa8S.png)

- Annotaions
  - 包含了xml文件，描述了图片的各种信息
- ImageSets
  - （主要关注main文件夹的内容）包含了不同类别目标的训练/验证数据集图片名称
- JPEGImages
  - 原图片
- SegmentationClass/SegmentationObject
  - 语义分割

#### COCO数据集

官网：https://cocodataset.org/#home

常用：COCO2017

### 标注数据集

https://www.makesense.ai/

https://cvat.org/

## 加载COCO数据集

```python
import torchvision
from PIL import ImageDraw

coco_datasets = torchvision.datasets.CocoDetection('val2017/val2017', 'val2017/annotations/captions_val2017.json')

image, info = coco_datasets[0]
image_handle = ImageDraw.ImageDraw(image)

for annotation in info:
    x_min, y_max, width, height = annotation['bbox']
    image_handle.rectangle(((x_min, y_max), (x_min+width, y_max+height)))

image.show()
```

