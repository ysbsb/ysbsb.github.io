---
layout: post
title: SqueezeNet 논문 리뷰
subtitle: SqueezeNet paper review
date:   2020-02-13 14:50:13
author: Subin Yang
categories: CNN
tags: [CNN]
comments: true
---

<strong><em>SqueezeNet: AlexNet-level accuracy with 50x fewer parameters and <0.5MB model size, Forreset N. landola et al, ICLR2017</em></strong>

[Paper](https://arxiv.org/abs/1602.07360)

<br>

<h2>SqueezeNet 방법 요약</h2>

기존의 CNN 모델들은 정확도를 높이는데에 집중한다. <strong>SqueezeNet은 적은 수의 파라미터를 가진 small CNN architecture를 제시한다.</strong>

작은 CNN 모델은 크게 3가지 장점이 있다.

1. 분산처리로 학습을 할 때 서버간의 통신이 적게 필요하다.
2. Cloud부터 자율주행차까지 새로운 모델을 적용하는데 더 수월하다.
3. 제한된 메모리가 있는 FPGA나 다른 하드웨어 같은곳에 적용하기 더 쉽다.

- SqueezeNet은 50배 적은 파라미터 수를 가지고 ImageNet 데이터셋에서 AlexNet-level accuracy를 달성했다.

- Model compression 테크닉을 가지고 SqueezeNet을 압축하여 더 적은 모델 크기를 달성했다.

<br>

<h2>Architecture 설계 전략</h2>

![2](https://user-images.githubusercontent.com/37301677/78473145-3b6d7a00-7779-11ea-8563-92cfed68edab.PNG)

- <strong>Strategy 1. 3x3 filter를 1x1 filter로 대체한다.</strong>

1x1 filter를 사용하면 3x3 filter를 사용했을 때 보다 9배 적은 파리미터를 사용한다.

- <strong>Strategy 2. 3x3 filter의 input channel의 수를 줄인다.</strong>

Convolution layer는 대부분 3x3 filter로 구성되어 있다.

이러한 layer의 total parameter의 양은 (input channel의 수) * (filer의 수) * ( 3 * 3 )이 된다.

따라서 CNN의 parameter의 수를 줄이는데는 3x3 filter의 개수뿐만 아니라, 3x3 filter의 input channel의 수도 중요하다.

- <strong>Strategy 3. Convolution layer가 large activation을 갖기 위해서 downsample을 늦게 한다.</strong>

Convolution network에는, 각 레이어마다 적어도 1x1에서 그 이상에 해당하는 spatial resolution을 가지는 output activation map을 만듭니다. Activation map의 높이와 너비는 (1) input data의 크기, (2) CNN architecture에서 어떤 레이어를 downsample 할 것인지에 의해서 조절된다.   

일반적으로, CNN에서 downsampling은 convolution이나 pooling layers들에 stride를 1보다 크게 설정하는 것으로 설계한다. 초기 레이어들에서 큰 stride를 가지면, 대부분의 레이어들은 작은 activation map을 가질 것이다. 반대로, 대부분의 레이어들이 크기 1의 stride를 가지고 있다면, 1보다 큰 stride는 네트워크의 끝 부분에 집중될 것이다. 저자들의 직관은 다른 부분들은 같다고 했을 때 큰 activation map은 더 높은 classification 정확도를 이끌 수 있다는 것이다.

<br>

<h2>Fire Module</h2>

Fire module은 1x1 filter로만 되어있는 squeeze convolution layer와, 1x1과 3x3 convolution이 섞여있는 expand layer로 구성되어 있다. 

![1](https://user-images.githubusercontent.com/37301677/78473144-39a3b680-7779-11ea-9bda-7de5bd3a3877.PNG)

3개의 튜닝 가능한 차원(하이퍼파라미터)가 있다.

- s1x1: squeeze layer의 filter의 수
- e1x1: expand layer에서 1x1 filter의 수
- e3x3: expand layer에서 3x3 filter의 수

<strong>Fire module은 s1x1은 e1x1과 e3x3을 합한 것보다 작게 설정하기 때문에, squeeze layer는 3x3 filter로 들어가는 input channel의 수를 줄이는데 도움을 준다.</strong>

<br>

<h2>Evaluation</h2>

- Network Pruning은 9배 model reduction을 하면서 baseline보다는 성능이 조금 낮고, Deep Compression은 35배 model을 reduction 하면서 baseline의 성능을 유지한다.
- SqueezeNet은 50배 model을 reduction 하면서 AlexNet의 top1과 top5 accuracy와 만나거나 능가한다.
- 작은 모델도 compression이 가능할까?
- Deep compression을 33% sparsity에 8 bit quantization으로 SqueezeNet에다 적용해보았는데 32bit AlexNet보다 363배 적은 모델로 AlexNet과 동등한 정확도를 보였다. 

![3](https://user-images.githubusercontent.com/37301677/78473147-3c061080-7779-11ea-9f3d-27e126286f68.PNG)
![4](https://user-images.githubusercontent.com/37301677/78473148-3c9ea700-7779-11ea-8d1b-4bc6058998c0.PNG)
![5](https://user-images.githubusercontent.com/37301677/78473149-3d373d80-7779-11ea-8a15-6d9b184eecb4.PNG)