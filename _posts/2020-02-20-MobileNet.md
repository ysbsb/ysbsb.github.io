---

layout: post
title: MobileNet 논문 리뷰, MobileNetv1 pytorch 코드 구현
subtitle: MobileNet paper review, MobileNetv1 pytorch code
date:   2020-02-20 15:07:07
author: Subin Yang
categories: CNN
tags: [CNN]
comments: true
---

<strong><em>MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications, Andrew G. Howard et al</em></strong>

[Paper](https://arxiv.org/abs/1704.04861)

<br>

<h2>MobileNetv1 방법 요약</h2>

MobileNet은 depthwise seperable convolution을 사용하여 모바일 환경에서도 사용할 수 있는 가벼운 딥뉴럴넷을 만들도록 했다. 논문의 후반부에는 이미 연산이 효율적으로 설게된 MoblieNet을 더 효율적으로 사용할 수 있도록 width multiplier와 resolution multiplier 두가지 파라미터를 도입한다.

<br>

<h2>Depthwise Seperable Convolution</h2>

Depthwise seperable convolution은 레이어에서 filtering 하는 부분(depthwise convolution)과 combining 하는 부분(pointwise convolution)의 두가지 부분으로 나눈 것이다. 이러한 factorization (분해) 방법은 computation과 모델 사이즈 모두를 줄일 수 있는 효과가 있다.

일반적인 covolutional layer는 feature map F에 대한 Df x Df x M의 input을 얻고, Df x Df x N에 해당하는 feature map G를 생성한다.

<br>

- Standard convolution computational cost

$$
D_K \cdot D_K \cdot M \cdot N \cdot D_F \cdot D_F
$$

- Depthwise convolution computational cost

$$
D_K \cdot D_K \cdot M \cdot D_F \cdot D_F
$$

- Depthwise separable computational cost

$$
D_K \cdot D_K \cdot M \cdot D_F \cdot D_F  +  M \cdot N \cdot D_F \cdot D_F
$$

<br>

예를 들어 3x3 detpthwise separable convolution을 사용한다면 8에서 9배 적은 computational cost를 사용할 수 있다.

<br>

![1](https://user-images.githubusercontent.com/37301677/74913879-d77b3600-5404-11ea-9918-28669e550a92.png)



<br>

<h2>Network Structure</h2>

- 3x3 Depthwise Conv - BN - ReLU - 1x1 Conv - BN - ReLU 가 하나의 Depthwise seperable convolution 모듈이 되도록 구현하였다.
- weight decay는 약간만 사용하거나 사용하지 않는다.
- Multi-Adds 연산을 비교하여 합성곱 연산을 얼마나 줄일 수 있는지 확인한다.

<br>

![2](https://user-images.githubusercontent.com/37301677/74913880-d8ac6300-5404-11ea-9ef0-c918f8c8992b.png)

<br>

```python
class SeparableConv2d(nn.Module):
    def __init__(self, in_channels, out_channels, stride):
        super(SeparableConv2d, self).__init__()

        self.depthwise = nn.Sequential(
            nn.Conv2d(in_channels, in_channels, kernel_size=3, padding=1, stride=stride),
            nn.BatchNorm2d(in_channels),
            nn.ReLU(inplace=True),
        )
        self.pointwise = nn.Sequential(
            nn.Conv2d(in_channels, out_channels, kernel_size=1, stride=1),
            nn.BatchNorm2d(out_channels),
            nn.ReLU(inplace=True)
        )

    def forward(self, x):
        x = self.depthwise(x)
        x = self.pointwise(x)
        return x
```

<br>

![3](https://user-images.githubusercontent.com/37301677/74913881-d944f980-5404-11ea-90c8-17017f7e97a5.png)

<br>

```python
import torch
import torch.nn as nn


class SeparableConv2d(nn.Module):
    def __init__(self, in_channels, out_channels, stride):
        super(SeparableConv2d, self).__init__()

        self.depthwise = nn.Sequential(
            nn.Conv2d(in_channels, in_channels, kernel_size=3, padding=1, stride=stride),
            nn.BatchNorm2d(in_channels),
            nn.ReLU(inplace=True),
        )
        self.pointwise = nn.Sequential(
            nn.Conv2d(in_channels, out_channels, kernel_size=1, stride=1),
            nn.BatchNorm2d(out_channels),
            nn.ReLU(inplace=True)
        )

    def forward(self, x):
        x = self.depthwise(x)
        x = self.pointwise(x)
        return x


class MobileNetv1(nn.Module):
    def __init__(self, num_classes=100):
        super(MobileNetv1, self).__init__()

        self.conv1 = nn.Conv2d(3, 32, kernel_size=3, padding=2)
        self.separable_conv2 = SeparableConv2d(32, 64, stride=1)
        self.separable_conv3 = SeparableConv2d(64, 128, stride=2)
        self.separable_conv4 = SeparableConv2d(128, 128, stride=1)
        self.separable_conv5 = SeparableConv2d(128, 256, stride=2)
        self.separable_conv6 = SeparableConv2d(256, 256, stride=1)
        self.separable_conv7 = SeparableConv2d(256, 512, stride=2)
        self.separable_conv8 = SeparableConv2d(512, 512, stride=1)
        self.separable_conv9 = SeparableConv2d(512, 512, stride=1)
        self.separable_conv10 = SeparableConv2d(512, 512, stride=1)
        self.separable_conv11 = SeparableConv2d(512, 512, stride=1)
        self.separable_conv12 = SeparableConv2d(512, 512, stride=1)
        self.separable_conv13 = SeparableConv2d(512, 1024, stride=2)
        self.separable_conv14 = SeparableConv2d(1024, 1024, stride=2)

        self.avgpool = nn.AvgPool2d(2)
        self.fc = nn.Linear(1024, num_classes)

        for m in self.modules():
            if isinstance(m, nn.Conv2d):
                nn.init.kaiming_normal_(m.weight, mode='fan_out', nonlinearity='relu')
                if m.bias is not None:
                    nn.init.constant_(m.bias, 0)
            elif isinstance(m, nn.BatchNorm2d):
                nn.init.constant_(m.weight, 1)
                nn.init.constant_(m.bias, 0)
            elif isinstance(m, nn.Linear):
                nn.init.normal_(m.weight, 0, 0.01)
                nn.init.constant_(m.bias, 0)

    def forward(self, x):
        x = self.conv1(x)
        x = self.separable_conv2(x)
        x = self.separable_conv3(x)
        x = self.separable_conv4(x)
        x = self.separable_conv5(x)
        x = self.separable_conv6(x)
        x = self.separable_conv7(x)
        x = self.separable_conv8(x)
        x = self.separable_conv9(x)
        x = self.separable_conv10(x)
        x = self.separable_conv11(x)
        x = self.separable_conv12(x)
        x = self.separable_conv13(x)
        x = self.separable_conv14(x)
        x = self.avgpool(x)
        x = x.view(x.size(0), -1)
        x = self.fc(x)
        return x

def mobilenetv1():
    return MobileNetv1()

```



<br>

![4](https://user-images.githubusercontent.com/37301677/74913882-d9dd9000-5404-11ea-8075-cf6628bce075.png)

여기서 주목할만한 점은 depthwise 연산과 pointwise 연산을 분리하였고, 이에 대한 분석으로 pointwise convolution이 합성곱 연산의 대부분을 차지한 다는 것을 알 수 있다.

<br>
