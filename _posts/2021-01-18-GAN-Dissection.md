---
layout: post
title: SinGAN 논문 리뷰 - SinGAN Learning a Generative Model from a Single Natural Image
subtitle: SinGAN paper review - SinGAN Learning a Generative Model from a Single Natural Image
date:   2020-12-30 21:31:31
author: Subin Yang
categories: GAN
tags: [GAN]
comments: true
use_math: true
---







> 
>
> 
>
> 안녕하세요 모카의 머신러닝 입니다. 이번에는 GAN Dissection이라 불리는 <em><strong>GAN Dissection: Visualizing and Understanding Generative Adversarial Networks</strong></em>, ICLR 2019 에 대해 리뷰합니다.
>
> [paper](https://arxiv.org/abs/1811.10597)



<br>

# GAN Dissection 연구 요약

GAN은 최근 많은 real-world 어플리케이션 위한 인상깊은 결과들을 보이고 있지만 잘 시각화 되거나 이해되지 않았습니다. 

GAN은 내부적으로 세상을 어떻게 표현할까요? GAN 결과에 무엇이 artifacts를 만들까요? 아키텍쳐의 선택이 GAN 학습에 어떤 영향을 미칠까요?

본 연구는 unit, object, scene 레벨에서 GAN의 시각화와 이해를 할 수 있는 분석적 프레임워크를 제시합니다.

구체적인 과정을 살펴보자면

1. 세그멘테이션 기반의 네트워크를 사용해서 object 컨셉과 비슷한 해석가능한 unit의 그룹을 구별합니다.
2. 결과에서 object를 컨트롤 할 수 있는 가능성에 대한 측정을 함으로써 해석가능한 unit의 인과적 효과를 정량화 합니다.
3. unit들과 관련된 것들을 새로운 이미지에 발견된 object 컨셉들을 삽입하는 방식으로 맥락적 관계를 조사합니다.



<br>

# GAN Dissection 방법

![architecture](https://user-images.githubusercontent.com/37301677/104919556-ca324700-59d9-11eb-9816-fb2af7c6fa6a.PNG)

크게 두 가지로 나눌 수 있습니다.

- <strong>Dissection</strong> (생성된 이미지의 명시적 표현을 갖는 클래스 확인)

  큰 object 클래스 딕셔너리에서 시작하여 r의 개별 단위와 모든 클래스 c간의 일치를 측정하여 r에서 명시적 표현을 갖는 클래스를 식별합니다.

- <strong>Intervention</strong> (해당 클래스를 on-off 하면서 이미지에서 영향을 확인)

  Dissection을 통해 식별된 대표 클래스의 경우 단위 집합을 강제로 켜고 끄는 방식으로 unit의 인과 집합을 식별하고 unit과 object 클래스 간의 인과적 영향을 측정합니다.



<br>

# 방법 1 - Characterizing units by Dissection

u: unit

r: unit u의 generator 피쳐맵

s: concept c의 semantic segmantation

c: concept

IoU를 사용하여 각 단위와 관련된 개념의 순위를 매기고 가장 일치하는 개념으로 각 단위의 레이블을 지정할 수 
있습니다. Figure 2에서 IoU가 더 높으면 해당 클래스를 선택한 개수가 더 높습니다.

![room](https://user-images.githubusercontent.com/37301677/104919631-e7ffac00-59d9-11eb-8123-3179acf310e5.PNG)

![diff_layer](https://user-images.githubusercontent.com/37301677/104919647-edf58d00-59d9-11eb-9d2a-e101b96e27d2.PNG)

![diff_scene](https://user-images.githubusercontent.com/37301677/104919650-ef26ba00-59d9-11eb-80c9-a8b6995ec0a5.PNG)
![ablation and insertion](https://use

<br>



# 방법 2 - Measuring casual relationships using Intervention

인과 관계에 대한 질문에 답하기 위해, intervention을 사용해서 네트워크를 증명합니다.

1. Intervention and ablation

   ​	![ablation and insertion](https://user-images.githubusercontent.com/37301677/104919670-f6e65e80-59d9-11eb-854a-2afe036c2e8a.PNG)

   1) 원래의 이미지

   2) 원래의 이미지에서 unit을 0으로 만든 이미지

   3) 원래의 이미지에서 unit을 더 넣은 이미지

   3가지를 비교합니다.

2. Averaging casual effect (ACE)

   ​	![ace](https://user-images.githubusercontent.com/37301677/104919672-f77ef500-59d9-11eb-8a57-7ad6afbd7325.PNG)

   인과 관계는 x_i와 x_a에 있는 나무의 존재를 비교하고, 모든 위치와 이미지에 대한 평균 효과를 비교하여 정량화 할 수 있습니다.

   이러한 방법을 사용하면 하나의 unit이 아미라 그 이상의 unit에 의존하는 경향이 있습니다. 따라서 object class c에 대해 average casual effect를 최대화하는 unit U의 집합을 식별해야 합니다.

3. Finding sets of units with high ACE

   ​	![ace_optimize](https://user-images.githubusercontent.com/37301677/104919676-f8178b80-59d9-11eb-9510-de9671372007.PNG)

   Degree of intervention을 암시하는 0과 1사이의 continuous intervention alpha를 최적화 합니다. ACE의 식을 변형하여 목적함수를 만들고 loss를 만듭니다. L2 regularization도 추가합니다. SGD로 최적화를 합니다.



<br>



# Results



![ablation](https://user-images.githubusercontent.com/37301677/104919689-fd74d600-59d9-11eb-8b88-6cfe815f2805.PNG)
![insertion](https://user-images.githubusercontent.com/37301677/104919695-fea60300-59d9-11eb-95df-8dc07a3b0048.PNG)

<br>



# Demo (데모 꼭 해보세요! 정말 신기해요)



MIT and IBM 공식 데모 링크 클릭

[https://ganpaint.io/](https://ganpaint.io/)







![ganpaint](https://user-images.githubusercontent.com/37301677/104919698-006fc680-59da-11eb-8a64-235ca5343a1c.PNG)



<br>



지금까지 GAN Dissection 논문 리뷰를 해보았습니다. 저도 논문을 읽으며 공부해나가는 단계라 부족한 점이 있을 수 있습니다. 같이 잘 공부해나가요! 질문이나 지적, 요청해주실 부분이 있다면 댓글이나 메일 부탁드립니다.

읽어주셔서 감사합니다. 😃



