---
layout: post
title: 베이지언 분류기
date:   2021-01-11 15:15:15
author: Subin Yang
categories: Machine_Learning
tags: [Machine_Learning]
comments: true
use_math: true
---









> 
>
> 베이지언 분류기



<br>





# 최소 오류 베이지언 분류기

<h3>이진 분류 문제</h3>

이진 분류 문제를 수학적으로 다음과 같이 표현할 수 있다.

![슬라이드1](https://user-images.githubusercontent.com/37301677/104152878-2f0eff80-5424-11eb-85ea-16b296c8f0e6.PNG)



<br>

<h3>분류 문제를 위한 베이즈 정리</h3>

베이즈 정리를 사용하면 이렇게 사후 확률을 다른 것으로 대치하여 계산할 수 있다.

![슬라이드2](https://user-images.githubusercontent.com/37301677/104152879-30402c80-5424-11eb-9bb7-2fb6d4657290.PNG)

사후 확률 계산이 우도, 사전 확률, p(x)의 계산으로 바뀌었다. 바뀐 것들은 어떻게 구할까?

- 사전 확률
  - 훈련 집합에 있는 샘플 중에 $\omega_{i}$ 에 속하는 샘플의 개수를 $n{i}$ 라 하면, $P(\omega_{1})=n_{1/N$ 과 $P(\omega_{2})=n_{2}/N$ 로 추정할 수 있음
- 우도 (likelihood)
  - $p(x|\omega_{i})$ 는 훈련 집합에서 $\omega_{i}$ 에 속하는 샘플들을 이용하여 추정하면 됨. 이는 다음 장에서 배우고, 여기서는 적절한 방법을 사용하여 이미 구해 놓았다 가정함
- 분모에 있는 p(x)
  - 사후 확률 값 자체가 필요한 것이 아니라 두 사후 확률 중 어느 것이 더 큰지 판단하면 됨. $\omega_{1}$ 과 $\omega_{2}$ 의 사후 확률이 공통으로 가지므로 생략해도 됨



<h3>최소 오류 베이시언 분류기</h3>



![슬라이드3](https://user-images.githubusercontent.com/37301677/104152880-30402c80-5424-11eb-8296-b9ffd61a3b2b.PNG)





<h3>베이시언 분류기의 오류 확률</h3>



![슬라이드4](https://user-images.githubusercontent.com/37301677/104152881-30d8c300-5424-11eb-9d46-4e52a2e04627.PNG)

<br>





# 




<br>



감사합니다. 😃

