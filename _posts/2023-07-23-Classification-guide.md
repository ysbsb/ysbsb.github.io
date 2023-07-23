---
layout: post
title: 딥러닝 Classification 가이드 - Classification 개념과 논문 리스트
subtitle: Deep Learning Classification Guide - Classification concept and paper list
category: [Deep_Learning]
tags: [Deep_Learning]
comments: true
---





> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2F2023%2F07%2F23%2F2023-07-23-Classification-guide.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> 이번에는 딥러닝에서 가장 널리 사용되는 분야 중 하나인 이미지 분류에 대해 이야기하려고 합니다. 이미지 분류를 이야기하기 전에, 뉴럴 네트워크는 무엇인지 설명하고, Fully connected neural network와 Convolutional neural network의 차이에 대해 설명하고, 이미지 분류를 성장하게 만든 계기인 ILSVRC 대회에 대해서 이야기하려고 합니다.

<br>





# 뉴럴 네트워크란?

뉴럴 네트워크는 인공지능 분야에서 가장 인기 있는 알고리즘 중 하나로, 뇌의 신경 세포인 뉴런의 작동 원리를 모델링한 컴퓨터 알고리즘입니다. 뉴럴 네트워크는 입력층, 은닉층, 출력층으로 구성된 다층 퍼셉트론(MLP)이라는 구조를 가지고 있습니다.

뉴럴 네트워크의 입력층에는 데이터가 입력되며, 이 데이터는 각각의 뉴런에 전달됩니다. 각 뉴런은 입력값에 가중치를 곱하고, 편향(bias)을 더하여 결과값을 출력합니다. 은닉층은 여러 개의 뉴런으로 구성되어 있으며, 입력층의 출력값이 전달되어 가중치와 편향을 반복적으로 계산합니다. 마지막으로, 출력층에서는 최종 결과값이 출력됩니다.

뉴럴 네트워크는 처음에는 무작위로 가중치와 편향을 초기화하고, 이후에는 훈련 데이터를 이용하여 학습을 진행합니다. 학습 과정에서는 출력값과 정답 사이의 오차를 최소화하기 위해 가중치와 편향을 조정하는 과정을 거치며, 이를 역전파(backpropagation) 알고리즘이라고 합니다.

![1](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/00014a64-2c67-4763-bcf9-ef624d7f937c)

Reference: [https://medium.com/codex/introduction-to-how-an-multilayer-perceptron-works-but-without-complicated-math-a423979897ac](https://medium.com/codex/introduction-to-how-an-multilayer-perceptron-works-but-without-complicated-math-a423979897ac)

# Image Classfication이란?

이미지 분류(image classification)는 컴퓨터 비전 분야에서 가장 기본적인 문제 중 하나로, 컴퓨터가 입력된 이미지를 어떤 클래스(카테고리)에 속하는지 분류하는 문제입니다. 예를 들어, 고양이와 개 사진이 주어졌을 때, 각각을 고양이와 개로 분류하는 것이 이미지 분류의 한 예입니다.

이미지 분류를 수행하는 머신 러닝 모델은 일반적으로 뉴럴 네트워크(딥 러닝)를 사용합니다. 딥 러닝 모델은 입력 이미지에서 특징(feature)을 추출하고, 이를 이용하여 이미지를 분류합니다.

이미지 분류를 위한 딥 러닝 모델은 일반적으로 컨볼루션 신경망(CNN)을 사용합니다. CNN은 이미지의 특징을 추출하기 위한 컨볼루션(Convolution) 레이어와, 추출된 특징을 이용하여 이미지를 분류하는 완전 연결(Fully Connected) 레이어로 구성됩니다.

컨볼루션 레이어에서는 이미지의 특징을 추출하기 위해 필터(filter)를 이용합니다. 필터는 이미지에서 작은 영역을 선택하여, 해당 영역의 정보를 추출합니다. 이러한 필터를 이용하여 이미지의 여러 영역에서 특징을 추출하고, 이를 이용하여 이미지를 분류합니다.

이미지 분류는 다양한 분야에서 활용됩니다. 예를 들어, 의료 영상에서 종양 여부를 판단하거나, 보안 카메라에서 위험 상황을 감지하는 등의 용도로 활용됩니다. 또한, 인터넷 상에서의 이미지 검색 등에서도 이미지 분류 기술이 사용됩니다.

![2](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/2adfa972-4458-41e5-b855-b12b91f151f8)

Reference: [https://campk12.com/project/syed.ghaleb/image-classification-using-mobile-net](https://campk12.com/project/syed.ghaleb/image-classification-using-mobile-net)

# ILSVRC 대회

ILSVRC(ImageNet Large Scale Visual Recognition Challenge) 대회는 2010년부터 시작된 컴퓨터 비전 분야의 대회로, 이미지 분류, 객체 탐지, 이미지 분할 등의 문제를 다룹니다. ILSVRC 대회는 대규모 이미지 데이터셋인 ImageNet 데이터셋을 사용하여 진행되며, 대회 참가자들은 주어진 데이터셋을 이용하여 딥 러닝 모델을 학습하고, 테스트 데이터셋에서 최고의 성능을 보이는 모델을 제출합니다.

ILSVRC 대회에서는 일반적으로 Top-5 error rate을 평가 지표로 사용합니다. Top-5 error rate은 모델이 테스트 데이터셋에서 가장 높은 확률로 예측한 5개의 클래스 중에서 실제 클래스가 포함되어 있지 않은 이미지의 비율을 의미합니다. 이 지표는 모델이 불확실한 경우에도, 적어도 5개의 클래스 중 하나를 선택하도록 유도합니다.

ILSVRC 대회에서는 매년 최신 딥 러닝 모델들이 경쟁하며, 대회에서 우승한 모델들은 이미지 분류 분야에서 대규모 이미지 데이터셋을 이용한 딥 러닝 모델의 발전에 큰 기여를 하였습니다. 또한, ILSVRC 대회에서 사용된 ImageNet 데이터셋은 딥 러닝 분야에서 가장 큰 이미지 데이터셋 중 하나로, 다양한 분야에서 연구와 응용에 활용되고 있습니다.

![3](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/5ce7aeda-9852-4393-b5dc-a58bb22c8ecf)

Reference: [https://it.chosun.com/site/data/html_dir/2020/07/31/2020073102344.html](https://it.chosun.com/site/data/html_dir/2020/07/31/2020073102344.html)

# Image Classification 논문 소개

## 1. [AlexNet] ImageNet Classification with Deep Convolutional Neural Networks (NeurIPS 2012)

- 5개의 컨볼루션 레이어와 3개의 fully connected 레이어를 가지고 top 1 error rate 37.5%, top 5 error rate 17.0%를 달성함

  ![4](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/c3bbcf96-e1cf-410b-9013-2cf086add42c)

## 2. [VGG] Very Deep Convolutional Networks for Large-Scale Image Recognition (ICLR 2015)

- 여러개의 컨볼루션 레이어를 더 깊에 쌓는 것을 목표로 함. 7x7, 5x5 convolution을 사용하기 보다 3x3 convolution을 사용하여 네트워크를 깊이를 16에서 19로 깊게 쌓음.

  ![5](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/75225897-bf73-4b23-9730-41678c0bc797)

## 3. [GoogLeNet] Going deeper with convolutions

- 3x3 convolution, 5x5 convolution, 1x1 convolution을 조합하여 네트워크를 구성함. 1x1 convolution의 역할은 계산 병목 현상을 없애기 위한 차원 축소를 하거나 네트워크의 사이즈를 제한하기 위함임. 22개의 네트워크를 쌓음. 2014년 ILSVRC 대회에서 1위를 달성함.

  ![8](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/0d4511a5-6c74-4d39-9978-ab41a5f2e573)

## 4. [ResNet] Deep Residual Learning for Image Recognition

- 잔차 학습 (Residual learning)을 통하여 네트워크를 더 깊에 쌓으면서 학습할 수 있도록 함. ResNet 이전의 네트워크들은 단순히 쌓는다고 해서 성능이 좋아지지 않았음. 이유는 네트워크를 쌓을수록 vanishing gradient 문제가 발생하기 때문임. ResNet은 이를 해결하기 위하여 convolution layer를 통과한 후 통과하기 전 indentity를 더하는 방식으로 residual learning을 수행함.

  ![6](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/ca0572e3-7511-4fdf-b8fe-fd73b9d2a410)

  ![7](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/eb4b840f-ef23-4640-ae1f-307c47d8af73)

## 5. [DenseNet] Densely Connected Convolutional Networks

- DenseNet은 정보의 흐름을 최대로 하는 것을 보장하기 위해서 모든 layer를 연결함. feed forward 특성을 유지하기 위해서, 각각의 layer들은 모든 이전 layer들로부터 추가적인 input을 받음. 그리고 현재의 feature map을 모든 다음의 layer들에 전달함. ResNet과 다르게 summation으로 feature를 합치지 않고, DenseNet은 feature들을 concatenate함.

  ![9](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/23398963-8103-46e2-91c4-2b711a2120ab)

![10](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/0ebf5553-bba2-43bb-87db-474a5a3772a7)

## 6. [MobileNet] MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications

- MobileNet은 depthwise seperable convolution을 사용하여 모바일 환경에서도 사용할 수 있는 가벼운 딥뉴럴넷을 만들도록 함. Depthwise seperable convolution은 레이어에서 filtering하는 부분 (depthwise convolution)과 combining하는 부분 (pointwise convolution)의 두 부분으로 나눈 것임. 이러한 분해 방법은 계산량과 모델 사이즈 모두를 줄일 수 있는 효과가 있음.

  ![11](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/a4192c4a-73d2-4d17-b1f5-e7e836d22163)

  ![12](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/4ce1ec10-2870-4fae-a947-ec3d97c9b178)

## 7. [SqueezeNet] SqueezeNet: AlexNet-level accuracy with 50x fewer parameters and <0.5MB model size

- SqueezeNet의 architecture 설계 전략은 3x3 filter 대신 1x1 filter로 대체하고, 3x3 filter의 input channel 수를 줄임. SqueezeNet은 Fire Module로 구성되어있고, Fire Module은 Squeeze layer와 Expand layer로 구성이 됨. Squeeze layer는 1x1 convolution으로 구성되어 있고, Expand lyaer는 1x1과 3x3 convolution으로 구성이 되어 있음. Squeeze layer의 channel 수가 expand layer의 channel 수보다 작게 설정함.

  ![13](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/eab0a89c-8993-45cc-b7a2-e6a411618e39)

## 8. [ShuffleNet] ShuffleNet: An Extremely Efficient Convolutional Neural Network for Mobile Devices

- Group convolution과 channel shuffle을 사용하여 네트워크의 연산량을 감소시킴. Pointwise group convolution은 1x1 convolution에서 입력 채널을 그룹으로 나눠 연산하는 방법임. Group convolution을 사용하면 채널 간의 정보 교환을 막기 때문에 표현력이 약해짐. 이를 해결하기 위해 channel shuffle을 사용함. Channel shuffle은 3x3 convolution의 입력이 각 group으로부터 오도록 함.

  ![14](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/c5ec3428-8c72-4144-a9a9-5236e0b8e849)

  ![15](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/2a8fe23b-772c-4da0-9c4a-df16110a2f0e)





여기까지 Classification의 개념과 주요 논문에 대한 간단한 리뷰까지 설명을 해보았습니다. 질문이나 지적, 요청해주실 부분이 있다면 댓글이나 메일 부탁드립니다.

읽어주셔서 감사합니다. 😃