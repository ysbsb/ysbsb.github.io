---
layout: post
title: 딥러닝 Object Detection 가이드 - Object Detection 개념과 논문 리스트
subtitle: Deep Learning Object Detection Guide - Object Detection concept and paper list
category: [Deep_Learning]
tags: [Deep_Learning]
comments: true
---





> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Fdeep_learning%2F2023%2F07%2F30%2Fobject-detection-guide.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> 이번에는 딥러닝 분야 중 하나인 객체 검출 (Object Detection)에 대해 이야기 하려고 합니다.

<br>

# Object Detection의 정의

Object detection은 컴퓨터 비전 분야에서, 이미지나 비디오에서 객체를 탐지하고, 해당 객체의 위치와 크기를 식별하는 기술입니다. 즉, 이미지나 비디오에서 여러 개의 객체를 감지하고, 분류하는 작업을 수행합니다. Object detection은 이미지 분류와 달리 객체가 있는 위치를 찾는 것이기 때문에, 해당 객체의 경계 상자(bounding box)를 표시하여, 객체의 위치를 나타내는 것이 일반적입니다.

Object detection은 다양한 응용 분야에서 활용됩니다. 자율주행 자동차에서는, 주변 환경의 객체를 탐지하여, 자동차의 주행을 제어합니다. 보안 카메라 시스템에서는, 감지된 객체를 식별하여, 위험한 상황을 예방하거나, 범죄 예방에 활용됩니다. 또한, 의료 분야에서는, X-ray, CT, MRI 등의 이미지를 분석하여, 질병을 진단하거나, 유방암 조직 이미지를 분석하여, 양성 종양과 악성 종양을 구분합니다.

Object detection은 딥 러닝 분야에서 다양한 모델과 기술이 개발되고 있습니다. Faster R-CNN, YOLO, SSD 등의 딥 러닝 모델들은 높은 정확도와 빠른 속도로 객체를 감지할 수 있는 기술적 발전을 이루고 있습니다.

![1](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/a905e7f3-1a62-4850-9daa-18b77a97d830)

[https://hoya012.github.io/blog/Tutorials-of-Object-Detection-Using-Deep-Learning-what-is-object-detection/](https://hoya012.github.io/blog/Tutorials-of-Object-Detection-Using-Deep-Learning-what-is-object-detection/)

<br>

# Classification과 Localization

Object detection은 보통 두 가지 작업으로 분류됩니다: 객체 분류(classification)와 객체 위치 식별(localization)입니다.

객체 분류는 이미지에서 객체가 무엇인지 판단하는 작업입니다. 예를 들어, 이미지 내에 개, 고양이, 자동차 등의 객체가 있다면, 객체 분류는 해당 객체가 어떤 카테고리에 속하는지를 판단하는 것입니다.

반면, 객체 위치 식별은 이미지에서 객체가 어디에 있는지를 찾아내는 작업입니다. 객체 위치 식별은 이미지 내에서 각 객체의 경계 상자(bounding box)를 찾는 것입니다. 경계 상자는 객체의 위치와 크기를 식별하는 정보를 담고 있으며, 일반적으로 사각형 형태로 표현됩니다.

이 두 가지 작업을 결합하여 객체 검출(object detection)을 수행합니다. 객체 검출은 이미지에서 객체의 위치와 클래스를 동시에 식별하는 것입니다. 따라서 객체 검출 모델은 이미지 내에 여러 객체를 동시에 감지하고, 각 객체의 클래스와 경계 상자를 출력합니다.

딥 러닝 기반 객체 검출 모델에서는, 일반적으로 객체 분류와 객체 위치 식별을 동시에 학습시킵니다. 예를 들어, Faster R-CNN과 같은 모델은 객체 분류와 객체 위치 식별을 위해 RPN(Region Proposal Network)을 사용합니다. RPN은 이미지 내에서 객체의 경계 상자 후보군을 생성하고, 이후 해당 후보군이 어떤 객체인지 분류하고, 정확한 경계 상자를 예측합니다.

딥 러닝 모델의 학습을 위해서는 대량의 레이블링된 데이터가 필요합니다. 객체 검출 모델에서는 레이블링된 데이터에는 이미지 내에 존재하는 객체의 클래스와 경계 상자 정보가 함께 포함되어 있어야 합니다. 이를 통해 모델은 객체 분류와 객체 위치 식별을 함께 학습할 수 있습니다.

![2](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/58cc6f87-353c-424c-a3cb-d6353a8f1615)

[https://medium.com/analytics-vidhya/object-localization-using-keras-d78d6810d0be](https://medium.com/analytics-vidhya/object-localization-using-keras-d78d6810d0be)

<br>

# Intersection over Union (IoU)

Intersection over Union (IoU)는 객체 검출(object detection)에서 사용되는 평가 지표 중 하나로, 객체의 경계 상자(bounding box)가 얼마나 정확한지를 측정하는 데 사용됩니다. IoU는 두 개의 경계 상자의 겹치는 영역의 크기를 그들의 결합 영역의 크기로 나눈 값입니다.

IoU는 다음과 같은 수식으로 표현됩니다.

IoU = Area of overlap / Area of Union

여기서 Area of Overlap은 두 경계 상자가 겹치는 영역의 면적이고, Area of Union은 두 경계 상자의 결합 영역의 면적입니다. IoU 값은 0과 1 사이의 값이며, 1에 가까울수록 경계 상자가 정확합니다.

객체 검출 모델에서는 IoU를 사용하여 모델이 예측한 경계 상자와 실제 레이블의 경계 상자가 얼마나 일치하는지를 측정합니다. 일반적으로, IoU 값이 임계값(threshold)보다 크면 예측이 올바른 것으로 판단됩니다. 예를 들어, 일반적으로 0.5 이상의 IoU 값을 가지는 예측 경계 상자를 올바른 예측으로 간주합니다.

IoU는 객체 검출에서 가장 널리 사용되는 평가 지표 중 하나이며, 모델 성능을 측정하는 데 중요한 역할을 합니다. 또한, IoU를 최적화하기 위해 모델 학습에 사용되는 손실 함수들 중 하나인 Smooth L1 Loss와 함께 사용됩니다.

![3](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/69610ea3-9d1c-44e4-9564-6732736c637e)

[https://idiotdeveloper.com/what-is-intersection-over-union-iou/](https://idiotdeveloper.com/what-is-intersection-over-union-iou/)

<br>

# 2 stage object detector와 1 stage object detector의 차이

2 stage object detector와 1 stage object detector는 객체 검출(object detection) 모델에서 사용되는 두 가지 주요 접근 방식입니다.

2 stage object detector는 두 단계(Stage)로 구성된 접근 방식입니다. 첫 번째 단계에서는 입력 이미지에서 주요한 객체(proposal) 후보를 선택하고, 두 번째 단계에서는 선택된 객체 후보들에 대해 객체의 위치와 클래스를 정확하게 예측합니다. 이러한 모델은 높은 정확도와 안정성을 가지고 있지만, 상대적으로 느린 속도와 높은 컴퓨팅 자원이 필요합니다. 예를 들어, Faster R-CNN, Mask R-CNN, Cascade R-CNN 등이 2 stage object detector의 대표적인 예시입니다.

1 stage object detector는 단일 단계(Single Stage)로 구성된 접근 방식입니다. 입력 이미지에서 바로 객체의 위치와 클래스를 예측하는 방식으로, 빠른 속도와 낮은 컴퓨팅 자원을 필요로 하지만, 상대적으로 낮은 정확도와 불안정성을 가질 수 있습니다. 예를 들어, YOLO, SSD(Single Shot Detector), RetinaNet 등이 1 stage object detector의 대표적인 예시입니다.

2 stage object detector는 복잡한 문제에서 높은 정확도와 안정성을 보장하지만, 속도와 컴퓨팅 자원이 제한된 환경에서는 사용이 어려울 수 있습니다. 반면, 1 stage object detector는 상대적으로 단순한 문제에서 높은 속도와 낮은 컴퓨팅 자원을 필요로 하지만, 더 복잡한 객체나 작은 객체를 검출하는 데 어려움을 겪을 수 있습니다.

따라서, 실제 객체 검출 태스크에서는 사용하고자 하는 데이터셋, 컴퓨팅 자원, 정확도 등 다양한 요소들을 고려하여 2 stage object detector와 1 stage object detector 중에서 선택하는 것이 적절합니다.

![4](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/97dbd101-4ffa-4ef9-baaa-546388035ead)

[https://velog.io/@qtly_u/Object-Detection-Architecture-1-or-2-stage-detector-차이](https://velog.io/@qtly_u/Object-Detection-Architecture-1-or-2-stage-detector-%EC%B0%A8%EC%9D%B4)

![5](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/1d9ef387-b69d-4ed5-9e91-72e8a40faecd)

[https://velog.io/@qtly_u/Object-Detection-Architecture-1-or-2-stage-detector-차이](https://velog.io/@qtly_u/Object-Detection-Architecture-1-or-2-stage-detector-%EC%B0%A8%EC%9D%B4)



<br>

# Object Detection 논문 소개

<br>

## 1. [R-CNN] Rich feature hierarchies for accurate object detection and semantic segmentation

- Convolution Neural Network (CNN)을 최초로 Object Detection에 적용함. Selective search를 통해 region proposal를 얻음. CNN을 통해 feature를 추출하고 SVM을 통해 classification을 하고 localization 또한 진행함.

  ![6](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/85ee07cf-bbef-40f4-890a-c801c4afea9a)

<br>

## 2. [Fast R-CNN] Fast R-CNN

- 이전에 R-CNN에서 이미지 레벨에서 object candidate를 찾았던 것과 달리, Fast R-CNN에서는 feature level에서 object candidate를 찾음. 또한 SVM을 사용하던 것에서 Softmax를 사용하는 것으로 변경함.

  ![7](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/55e2714b-6c89-46aa-a221-aae5dd14a2c7)

<br>

## 3. [Faster R-CNN] Faster R-CNN: Towards Real-Time Object Detection with Region Proposal Networks

- Region proposal network (RPN)을 제안하여, 기존에 selective search를 통해 CPU에서 작동하는 것과 달리, object candidate를 CNN 기반으로서 GPU에서 찾도록 함.

  ![8](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/6913a0f9-f855-4900-9662-8a66a1249e42)

<br>

## 4. [FPN] Feature Pyramid Networks for Object Detection

- Feature Pyramid Network (FPN)를 제안함. 기존 방법들은 single scale의 receptive field를 얻는 것과 달리, FPN은 multi scale의 다양한 receptive field를 얻음. 다양한 receptive field를 통해 다양한 크기의 객체를 인식할 수 있음.

  ![9](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/a1936030-692d-4a81-a196-c4885c40c120)

<br>

## 5. [Mask R-CNN] Mask R-CNN

- Faster R-CNN에 mask branch를 추가한 것으로, mask branch는 object의 mask를 예측하는 branch 임.

  ![10](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/4733b3f7-90b0-4d78-a878-6b63a0aed26a)

<br>

## 6. [YOLO] You Only Look Once: Unified, Real-Time Object Detection

- 기존 논문은 two stage로 classification과 localization을 수행하여 object를 detect하는 것과 달리, 사람이 이미지를 한번에 인식하는 것과 같이, 뉴럴 네트워크가 region proposal과 classification을 한번에 수행하여 한번에 detection을 수행하도록 함.

  ![11](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/82722cc9-dc07-488e-a626-87d2306e33f8)

  ![12](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/f2bd6e00-886f-405e-a8d9-4b188fafc2f5)

<br>

## 7. [SSD] SSD: Single Shot MultiBox Detector

- 기존 YOLO가 마지막 feature map에서 single scale feature map을 얻어내는 것과 달리, SSD는 multiscale feature map을 얻도록 함. 또한 fully connected layer를 사용하던 것에서 convolutional neural network를 사용하는 것으로 바꿈.

  ![13](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/1ba5fac5-67ad-46aa-b76c-ae3511f89925)

<br>

## 8. [RetinaNet] Focal Loss for Dense Object Detection

- Object Detection에서 발생하는 class imbalance 문제를 Focal loss를 도입함으로써 해결함.

  ![14](https://github.com/ysbsb/ysbsb.github.io/assets/37301677/8ea114f3-18e6-4094-8639-3d33b11ce375)

<br>

여기까지 Object Detection의 개념과 주요 논문에 대한 간단한 리뷰까지 설명을 해보았습니다.

읽어주셔서 감사합니다. 😃