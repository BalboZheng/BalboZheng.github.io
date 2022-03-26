---
layout: post
title: 人脸识别
subtitle: 
author: Balbo Cheng
categories: python
tags: [cv2, matplotlib]
sidebar: []
---

我们使用机器学习完成人脸检测，首先需要大量的正样本图像（面部图像）和负样本图像（不含面部）来训练分类器。我们需要从其中提取特征（Haar特征）。就像卷积核一样，每一个特征都是一个值，这些值等于黑色矩阵中的像素值之后减去白色矩阵中的像素值之和。

![](https://img-blog.csdn.net/20170106143949414?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjUwNzAyMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

Haar特征反映了图像的灰度变化情况。例如：脸部的一些特征能由矩形特征简单的描述，眼睛要比脸颊颜色要深，鼻梁两侧比鼻梁颜色要深，嘴巴比周围颜色要深。

Haar特征可用于图像任意位置，大小也可以任意改变，所以矩阵特征值是矩阵模板类别、矩阵位置和矩阵大小中三个因素的函数。故类别、大小和位置的变化，使得很小的检测窗口含有非常多的矩阵特征。

得到图像特征后，训练一个决策树构建的adaboost级联决策器来识别是否人脸

```python
import cv2
import matplotlib.pyplot as plt

filepath = 'VOCtrainval_06-Nov-2007/VOCdevkit/VOC2007/JPEGImages/000009.jpg'
img = cv2.imread(filepath)
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

# 实例化级联分类器
face_cas = cv2.CascadeClassifier('haarcascade_frontalface_default.xml')
# 加载分类器
face_cas.load('haarcascade_frontalface_default.xml')

eye_cas = cv2.CascadeClassifier('haarcascade_eye.xml')
eye_cas.load('haarcascade_eye.xml')

# 调用人脸识别
faceRects = face_cas.detectMultiScale(gray, scaleFactor=1.2, minNeighbors=3, minSize=(32, 32))
for faceRects in faceRects:
    x, y, w, h = faceRects
    # 框出人脸
    cv2.rectangle(img, (x, y), (x + h, y + w), (0, 255, 0), 3)
    # 在识别出的人脸中进行眼睛的检测
    roi_color = img[y:y + h, x:x + w]
    roi_gray = gray[y:y + h, x:x + w]
    eyes = eye_cas.detectMultiScale(roi_gray)
    for (ex, ey, ew, eh) in eyes:
        cv2.rectangle(roi_color, (ex, ey), (ex + ew, ey + eh), (0, 255, 0), 3)

plt.figure(figsize=(8, 6), dpi=100)
plt.imshow(img[:, :, ::-1]), plt.title("检测结果")
plt.xticks([]), plt.yticks([])
plt.show()
```

也可以识别视频

```python
import cv2

filepath = 'VOCtrainval_06-Nov-2007/VOCdevkit/VOC2007/JPEGImages/000009.jpg'
cap = cv2.VideoCapture(filepath)

while cap.isOpened():
    ret, frame = cap.read()
    if ret:
        gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
        # 实例化级联分类器
        face_cas = cv2.CascadeClassifier('haarcascade_frontalface_default.xml')
        # 加载分类器
        face_cas.load('haarcascade_frontalface_default.xml')
        # 调用人脸识别
        faceRects = face_cas.detectMultiScale(gray, scaleFactor=1.2, minNeighbors=3, minSize=(32, 32))
        for faceRects in faceRects:
            x, y, w, h = faceRects
            # 框出人脸
            cv2.rectangle(frame, (x, y), (x + h, y + w), (0, 255, 0), 3)
        cv2.imshow('frame', frame)

        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

cap.release()
cv2.destroyWindow('frame')
```