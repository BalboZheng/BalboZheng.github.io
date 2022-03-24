---
layout: post
title: 深度学习
subtitle: 
author: Balbo Cheng
categories: python
banner:
  image: https://bkimg.cdn.bcebos.com/pic/6c224f4a20a44623bdbfedcf9222720e0cf3d722?x-bce-process=image/watermark,image_d2F0ZXIvYmFpa2U4MA==,g_7,xp_5,yp_5/format,f_auto
tags: [pytorch]
---

## tensorboard

```python
from torch.utils.tensorboard import SummaryWriter
import numpy as np
from PIL import Image

writer = SummaryWriter('logs')

img_Path = 'ants_bees/train/ants_image/0013035.jpg'
img_PLT = Image.open(img_Path)
img_array = np.array(img_PLT)
print(img_array.shape)

writer.add_image('test', img_array, 1, dataformats='HWC')

for i in range(100):
    writer.add_scalar('y=x', i, i)

writer.close()

```

执行命令：

```shell
tensorboard --logdir=logs 
```

## transforms

```python
import cv2
from torchvision import transforms
from torch.utils.tensorboard import SummaryWriter

writer = SummaryWriter('logs')
img_Path = 'ants_bees/train/ants_image/0013035.jpg'
img_cv = cv2.imread(img_Path)

img_trans = transforms.ToTensor()
img_tensor = img_trans(img_cv)

writer.add_image('Img_Tensor', img_tensor)

writer.close()
```

### 常用transforms

```python
import cv2
from PIL import Image
from torchvision import transforms
from torch.utils.tensorboard import SummaryWriter

writer = SummaryWriter('logs')
img_Path = 'ants_bees/train/ants_image/6743948_2b8c096dda.jpg'
img_PIL = Image.open(img_Path)

# Totensor
trans_tensor = transforms.ToTensor()
img_tensor = trans_tensor(img_PIL)
writer.add_image('ToTensor', img_tensor)

# Normalize
trans_norm = transforms.Normalize([0.5, 0.5, 0.5], [0.5, 0.5, 0.5])
img_norm = trans_norm(img_tensor)
writer.add_image('Normalize', img_norm)

# Resize
trans_resize = transforms.Resize((512, 512))
img_resize = trans_resize(img_PIL)
img_resize = trans_tensor(img_resize)
writer.add_image('Resize', img_resize)

# RandomCrop
trans_random = transforms.RandomCrop(128)
trans_compose = transforms.Compose([trans_random, trans_tensor])
img_crop = trans_compose(img_PIL)
writer.add_image('RandomCrop', img_crop)

writer.close()

```

## torchvisions

### 数据集（datasets）

```python
import torchvision
import ssl
# 全局取消证书验证
ssl._create_default_https_context = ssl._create_unverified_context

datasets_trans = torchvision.transforms.Compose([
    torchvision.transforms.ToTensor()
])

train = torchvision.datasets.CIFAR10('./datasets', transform=datasets_trans, download=True)
test = torchvision.datasets.CIFAR10('./datasets', transform=datasets_trans, train=False, download=True)

```

### DataLoader

```python
import torchvision
import ssl

from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

ssl._create_default_https_context = ssl._create_unverified_context
datasets_trans = torchvision.transforms.Compose([
    torchvision.transforms.ToTensor()
])
writer = SummaryWriter('dataloader')

data = torchvision.datasets.CIFAR10('./datasets', transform=datasets_trans, download=True)

loader = DataLoader(data, 4, True, num_workers=0, drop_last=False)

img, target = data[0]
print('---img, target---')
print(img.shape, target)

print('---imgs,targets---')
for epoch in range(2):
    step = 0
    for data in loader:
        imgs, targets = data
        # print(imgs.shape, targets)
        writer.add_image('Epoch: {}'.format(epoch), imgs, step)
        step += 1

writer.close()

```

## 卷积操作

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

data = torch.tensor([[1, 2, 0, 3, 1],
                     [0, 1, 2, 3, 1],
                     [1, 2, 1, 0, 0],
                     [5, 2, 3, 1, 1],
                     [2, 1, 0, 1, 1]])

kernel = torch.tensor([[1, 2, 1],
                       [0, 1, 0],
                       [2, 1, 0]])

data = torch.reshape(data, (1, 1, 5, 5))
kernel = torch.reshape(kernel, (1, 1, 3, 3))


class Model(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv1 = nn.Conv2d(1, 20, 5)
        self.conv2 = nn.Conv2d(20, 20, 5)

    def forward(self, x):
        x = F.relu(self.conv1(x))
        return F.relu(self.conv2(x))


model = Model()
output = F.conv2d(data, kernel, stride=1)
print(output)

```

### 卷积层

```python
import torch
import torchvision
from torch import nn
from torch.nn import functional as F
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

data = torchvision.datasets.CIFAR10('./datasets', train=False, transform=torchvision.transforms.ToTensor(), download=True)

dataloader = DataLoader(data, 64)


class Model(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv1 = nn.Conv2d(3, 6, 3)

    def forward(self, x):
        x = F.relu(self.conv1(x))
        return x


model = Model()
writer = SummaryWriter('logs')

step = 0
for data in dataloader:
    imgs, tragets = data
    output = model(imgs)
    # print(imgs.shape)
    # print(output.shape)
    writer.add_images("input", imgs, step)

    output = torch.reshape(output, (-1, 3, 30, 30))
    writer.add_images("output", output, step)
    step += 1

```

### 池化层

```python
import torch
import torch.nn as nn
import torch.nn.functional as F
from torch.nn import MaxPool2d

data = torch.tensor([[1, 2, 0, 3, 1],
                     [0, 1, 2, 3, 1],
                     [1, 2, 1, 0, 0],
                     [5, 2, 3, 1, 1],
                     [2, 1, 0, 1, 1]], dtype=torch.float32)

data = torch.reshape(data, (-1, 1, 5, 5))


class Model(nn.Module):
    def __init__(self):
        super().__init__()
        self.maxpool1 = MaxPool2d(3, ceil_mode=True)

    def forward(self, x):
        x = self.maxpool1(x)
        return x


model = Model()
output = model(data)
print(output)
运行结果：
tensor([[[[2., 3.],
          [5., 1.]]]])
```

### 非线性激活

```python
import torch
import torch.nn as nn
import torch.nn.functional as F
import torchvision
from torch.nn import ReLU, Sigmoid
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

data = torch.tensor([[1, -0.5],
                     [-1, 3]])

data = torch.reshape(data, (-1, 1, 2, 2))

dataset = torchvision.datasets.CIFAR10('./datasets', train=False, download=True, transform=torchvision.transforms.ToTensor())

dataloader = DataLoader(dataset, 64)


class Model(nn.Module):
    def __init__(self):
        super().__init__()
        self.relu = ReLU()
        self.sigmoid = Sigmoid()

    def forward(self, x):
        x = self.sigmoid(x)
        return x


model = Model()
writer = SummaryWriter('logs')
step = 0

for data in dataloader:
    imgs, targets = data
    writer.add_images('sigmoid_input', imgs, step)
    output = model(imgs)
    writer.add_images('sigmoid_output', output, step)
    step += 1

writer.close()
```

### 线性层

```python
import torch
import torch.nn as nn
import torchvision
from torch.nn import Linear
from torch.utils.data import DataLoader

data = torch.tensor([[1, -0.5],
                     [-1, 3]])

data = torch.reshape(data, (-1, 1, 2, 2))

dataset = torchvision.datasets.CIFAR10('./datasets', train=False, download=True, transform=torchvision.transforms.ToTensor())

dataloader = DataLoader(dataset, 64)


class Model(nn.Module):
    def __init__(self):
        super().__init__()
        self.linear = Linear(196608, 10)

    def forward(self, x):
        x = self.linear(x)
        return x


model = Model()

for data in dataloader:
    imgs, targets = data
    print(imgs.shape)
    # output = torch.reshape(imgs, (1, 1, 1, -1))
    output = torch.flatten(imgs)
    print(output.shape)
    output = model(output)
    print(output.shape)

```

### Sequential

![微信图片_20220322202452.png](https://s2.loli.net/2022/03/22/FHsRa3I7QVKGPqJ.png)

```python
import torch
from torch import nn
from torch.nn import MaxPool2d, Conv2d, Flatten, Linear, Sequential


class Model(nn.Module):
    def __init__(self):
        super().__init__()
        # self.conv1 = Conv2d(3, 32, 5, padding=2)
        # self.maxpool1 = MaxPool2d(2)
        # self.conv2 = Conv2d(32, 32, 5, padding=2)
        # self.maxpool2 = MaxPool2d(2)
        # self.conv3 = Conv2d(32, 64, 5, padding=2)
        # self.maxpool3 = MaxPo	ol2d(2)
        # self.flatten = Flatten()
        # self.linear1 = Linear(1024, 64)
        # self.linear2 = Linear(64, 10)
        self.sequ = Sequential(
            Conv2d(3, 32, 5, padding=2),
            MaxPool2d(2),
            Conv2d(32, 32, 5, padding=2),
            MaxPool2d(2),
            Conv2d(32, 64, 5, padding=2),
            MaxPool2d(2),
            Flatten(),
            Linear(1024, 64),
            Linear(64, 10)
        )

    def forward(self, x):
        # x = self.conv1(x)
        # x = self.maxpool1(x)
        # x = self.conv2(x)
        # x = self.maxpool2(x)
        # x = self.conv3(x)
        # x = self.maxpool3(x)
        # x = self.flatten(x)
        # x = self.linear1(x)
        # x = self.linear2(x)
        x = self.sequ(x)
        return x


model = Model()
print(model)
intput = torch.ones((64, 3, 32, 32))
output = model(intput)
print(output.shape)

```

## 损失函数

```python
import torchvision
from torch import nn
from torch.nn import MaxPool2d, Conv2d, Flatten, Linear, Sequential
from torch.utils.data import DataLoader

data = torchvision.datasets.CIFAR10('./datasets', transform=torchvision.transforms.ToTensor(), download=True)

loader = DataLoader(data, 1)


class Model(nn.Module):
    def __init__(self):
        super().__init__()
        self.sequ = Sequential(
            Conv2d(3, 32, 5, padding=2),
            MaxPool2d(2),
            Conv2d(32, 32, 5, padding=2),
            MaxPool2d(2),
            Conv2d(32, 64, 5, padding=2),
            MaxPool2d(2),
            Flatten(),
            Linear(1024, 64),
            Linear(64, 10)
        )

    def forward(self, x):
        x = self.sequ(x)
        return x


model = Model()
loss = nn.CrossEntropyLoss()

for data in loader:
    imgs, targets = data
    outputs = model(imgs)
    result = loss(outputs, targets)
    print(result)

```

### 优化

```python
model = Model()
loss = nn.CrossEntropyLoss()
optim = torch.optim.SGD(model.parameters(), lr=0.01)

for data in loader:
    imgs, targets = data
    outputs = model(imgs)
    result = loss(outputs, targets)
    optim.zero_grad()
    result.backward()
    optim.step()
```

## 保存与加载

```python
vgg16 = torchvision.models.vgg16(False)

# 保存1，模型结构+模型参数
torch.save(vgg16, 'method_1.pth')

# 保存2，模型参数
torch.save(vgg16.state_dict(), 'method_2.pth')

# 加载1
load = torch.load('method_1.pth')
print(load)

# 加载2
vgg16 = torchvision.models.vgg16(False)
vgg16.load_state_dict(torch.load('method_2.pth'))
print(load)
```

## 完整模型训练套路

module.py

```python
from torch import nn
from torch.nn import Sequential, Conv2d, MaxPool2d, Flatten, Linear, Sigmoid


# 搭建神经网络
class Model(nn.Module):
    def __init__(self):
        super().__init__()
        self.model = Sequential(
            Conv2d(3, 32, 5, 1, 2),
            MaxPool2d(2),
            Conv2d(32, 32, 5, 1, 2),
            MaxPool2d(2),
            Conv2d(32, 64, 5, 1, 2),
            MaxPool2d(2),
            Flatten(),
            Linear(64*4*4, 64),
            Linear(64, 10),
            # Sigmoid()
        )

    def forward(self, x):
        x = self.model(x)
        return x

```

dataset.py

```python
import torch
import torchvision
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

from module import *

# 数据集
train_data = torchvision.datasets.CIFAR10('./datasets', train=True, transform=torchvision.transforms.ToTensor(),
                                          download=True)

test_data = torchvision.datasets.CIFAR10('./datasets', train=False, transform=torchvision.transforms.ToTensor(),
                                         download=True)

train_data_size = len(train_data)
test_data_size = len(test_data)
# print("训练数据集的长度为：{}".format(train_data_size))
# print("测试训练集的长度为：{}".format(test_data_size))

# 加载数据集
train_dataloader = DataLoader(train_data, 64)
test_dataloader = DataLoader(test_data, 64)

# 创建网络模型
model = Model()

# 损失函数
loss_fn = nn.CrossEntropyLoss()

# 优化器
learning_rate = 0.001
optimizer = torch.optim.SGD(model.parameters(), lr=learning_rate)

# 设置训练网络的一些参数
# 记录训练的次数
total_train_step = 0
# 记录测试的次数
total_test_step = 0
# 训练的轮数
epoch = 10

# 添加 tensorboard
writer = SummaryWriter('logs')

for i in range(epoch):
    print("第 {} 轮训练".format(i+1))

    # 开始训练
    for data in train_dataloader:
        imgs, targets = data
        outputs = model(imgs)
        loss = loss_fn(outputs, targets)
        # 优化器优化模型
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
        # 步骤加一
        total_train_step += 1
        if total_train_step % 100 == 0:
            print("训练次数 {}，loss为 {}".format(total_train_step, loss.item()))
            writer.add_scalar('train_loss', loss.item(), total_train_step)

    # 开始测试
    total_test_loss = 0
    total_accuracy = 0
    with torch.no_grad():
        for data in test_dataloader:
            imgs, targets = data
            outputs = model(imgs)
            loss = loss_fn(outputs, targets)
            total_test_loss += loss.item()
            total_accuracy = (outputs.argmax(1) == targets).sum()
    print("整体测试集上的Loss： {}".format(total_test_loss))
    print("整体测试集上的正确率： {}".format(total_accuracy/test_data_size))
    writer.add_scalar('test_loss', total_test_loss, total_test_step)
    writer.add_scalar('test_accuracy', total_accuracy/test_data_size, total_test_step)
    total_test_step += 1

    torch.save(model, "model_{}.pth".format(i))
    print("模型 {} 已保存".format(i))

writer.close()

```

## 利用GPU训练

```python
# 方法一 .cuda
# 创建网络模型
model = Model()
if torch.cuda.is_available():
	model.cuda()

# 损失函数
loss_fn = nn.CrossEntropyLoss()
if torch.cuda.is_available():
	loss_fn.cuda()

# 开始训练
for data in train_dataloader:
	imgs, targets = data
	if torch.cuda.is_available():
		imgs = imgs.cuda()
		targets = targets.cuda()
		
# 方法二 .to
# 定义训练的设备
device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')

# 创建网络模型
model = Model()
model.to(divice)

# 损失函数
loss_fn = nn.CrossEntropyLoss()
loss_fn.to(divice)

# 开始训练
for data in train_dataloader:
	imgs, targets = data
	imgs = imgs.to(divice)
	targets = targets.to(divice)
```

## 完整模型测试套路

```python
import torch
import torchvision
from PIL import Image

image_path = "../img/airplane.png"
image = Image.open(image_path)
print(image)

transform = torchvision.transforms.Compose([torchvision.transforms.Resize((32, 32)),
                                            torchvision.transforms.ToTensor()])

image = transform(image)
image = torch.reshape(image, (1, 3, 32, 32))
print(image)

model = torch.load('module_1.pth')
print(model)
model.eval()
with torch.no_grad():
    output = model(image)
print(output)
print(output.argmax(1))

```

