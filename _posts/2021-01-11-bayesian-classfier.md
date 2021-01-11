---
layout: post
title: 베이시언 분류기 (Bayesian classifier)
subtitle: Bayesian classifer
date:   2021-01-11 15:15:15
author: Subin Yang
categories: Machine_Learning
tags: [Machine_Learning]
comments: true
use_math: true
---









> 베이지언 분류기



<br>





<h2>최소 오류 베이시언 분류기</h2>

<h3>이진 분류 문제</h3>

이진 분류 문제를 수학적으로 다음과 같이 표현할 수 있다.

![슬라이드1](https://user-images.githubusercontent.com/37301677/104166216-cb91cb80-543d-11eb-968c-a2447a664001.PNG)



<br>



<h3>분류 문제를 위한 베이즈 정리</h3>

베이즈 정리를 사용하면 이렇게 사후 확률을 다른 것으로 대치하여 계산할 수 있다.

![슬라이드2](https://user-images.githubusercontent.com/37301677/104166218-ccc2f880-543d-11eb-87e8-4a467ab1e218.PNG)

사후 확률 계산이 우도, 사전 확률, p(x)의 계산으로 바뀌었다. 바뀐 것들은 어떻게 구할까?

- 사전 확률
  - 훈련 집합에 있는 샘플 중에 $\omega_{i}$ 에 속하는 샘플의 개수를 $n_{i}$ 라 하면, $P(\omega_{1})=n_{1}/N$ 과 $P(\omega_{2})=n_{2}/N$ 로 추정할 수 있음
- 우도 (likelihood)
  - $p\left(x \mid \omega_{1}\right)$ 는 훈련 집합에서 $\omega_{i}$ 에 속하는 샘플들을 이용하여 추정하면 됨. 이는 다음 장에서 배우고, 여기서는 적절한 방법을 사용하여 이미 구해 놓았다 가정함
- 분모에 있는 p(x)
  - 사후 확률 값 자체가 필요한 것이 아니라 두 사후 확률 중 어느 것이 더 큰지 판단하면 됨. $\omega_{1}$ 과 $\omega_{2}$ 의 사후 확률이 공통으로 가지므로 생략해도 됨

<br>



<h3>최소 오류 베이시언 분류기</h3>

특징 벡터를 여러 개 부류 주의 하나로 분류하는 규칙을 <strong>결정 규칙 (decision rule)</strong> 이라 하고, 결정 규칙을 사용하는 분류기를 <strong>최소 오류 베이시언 분류기 (minimum error Bayesian classifier)</strong> 라고 부른다.

![슬라이드3](https://user-images.githubusercontent.com/37301677/104166219-cd5b8f00-543d-11eb-9b94-0dfc875231eb.PNG)

몇 가지 특수한 경우를 가지고 베이시언 분류기의 이미를 생각해보자.

- 사전 확률이 0.5인 경우
  - 위의 식에서 사전 확률을 제거해도 됨
- $\omega_{1}$ 의 샘플이 $\omega_{2}$ 의 샘플보다 훨씬 많은 경우.
  - 예를 들어, $\omega_{1}$ 이 하나 나타날 때, $\omega_{2}$  가 10000개 정보 나타난다면, $\omega_{2}$ 라고 말하며도 10001개 중에 하나만 틀리게 되서 오류율이 0.01% 로 매우 낮다.



<br>



<h3>베이시언 분류기의 오류 확률</h3>

베이시언 분류기는 어느 정도 오류를 범할까? 베이시언 분류기의 오류 확률을 정확하게 예측할 수 있다.

![슬라이드4](https://user-images.githubusercontent.com/37301677/104166221-cd5b8f00-543d-11eb-9304-a2c63ce0de1a.PNG)

문제를 쉽게 하기 위해 두 부류의 사전 확률이 같다고 가정하자.  그림은 사전 확률이 같다는 가정 하에 부류 조건부 확률을 보여준다. 두 확률 분포 함수의 값이 같아지는 점을 t라고 하였다.

- x>t 이면
  - $\omega_{2}$ 로 분류하고, x<t 이면 $\omega_{1}$로 분류한다.
- x=a 일 때
  - $p\left(x \mid \omega_{1}\right) < p\left(x \mid \omega_{2}\right)$ 이므로 베이시언 분류기는 $\omega_{2}$  로 분류한다. 
  - 하지만 x=a 인 샘플 중에 $\omega_{1}$ 에 속하는 것이 확률 e 만큼 발생한다. $\omega_{1}$ 가 $\omega_{2}$  로 분류되면 오류가 발생한다. $\omega_{1}$ 의 사전 확률이 1/2 이므로 실제 오류 확률은 e/2 이다.
- 전체 영역으로 따지면
  - 오류 확률은 색칠한 면적의 1/2 이다.

   



<br>





감사합니다. 😃

