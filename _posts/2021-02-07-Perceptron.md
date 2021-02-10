---
layout: post
title: 퍼셉트론 (Perceptron) 구조와 학습, 퍼셉트론 python 코드
subtitle: Perceptron structure and learning, code
date:   2021-02-07 17:21:21
author: Subin Yang
categories: Machine_learning
tags: [Machine_learning]
comments: true
use_math: true
---







> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Fmachine_learning%2F2021%2F02%2F07%2FPerceptron.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> 안녕하세요 모카의 머신러닝 입니다. 이번 포스팅은 패턴인식 스터디에서 공부한 퍼셉트론 (Perceptron)에 대해서 포스팅 합니다.
>



<br>

포스팅 하기에 앞서 MLVC 연구실 패턴인식 (오일석 저) 스터디를 하면서 공부 내용을 정리했음을 밝힙니다.

<br>

<h1>퍼셉트론 구조와 동작</h1>

퍼셉트론은 선형 분류기라고 합니다. 따라서 아직 완벽하지는 않지만 나중에 다층 퍼셉트론의 토대가 되었습니다. 그림 또한 직접 그렸습니다.

![슬라이드1](https://user-images.githubusercontent.com/37301677/107141159-5e088a80-696a-11eb-8424-b3bc3c054b99.PNG)

입력층은 d+1개의 노드를 갖고, 나머지 한 개는 바이어스 노드로 항상 1을 갖습니다. 출력층은 하나의 노드를 갖습니다. 퍼셉트론은 두개의 부류 1과 -1로 분류할 수 있고, 두개로 분류해서 이진 분류기라고 합니다. 입력 노드와 출력 노드는 에지로 연결되어 있고 이 에지를 가중치라고 합니다. 



출력층에서는 활성 함수가 같이 있어서 합 계산과 활성 함수 계산을 순차적으로 같이 합니다. 가중치와 특징 벡터를 곱해서 더한 후 , 활성함수로 계단함수를 사용하고 이게 1아니면 -1로 분류를 합니다. 활성함수 이후에 나온 값이 0보다 크면 1이고, 아니면 -1로 분류합니다. 그리고 이렇게 분류를 하는 것을 선형 분류기라고 합니다. 

<br>

<h2>예제1: OR 분류</h2>

![슬라이드2](https://user-images.githubusercontent.com/37301677/107141160-5f39b780-696a-11eb-8c43-0b7dfd63cfc8.PNG)

퍼셉트론에 대한 예제가 있습니다. 이건 가중치나 부류를 구하는 문제는 아니고, 이미 값이 다 주어지고 어떻게 동작하는지 확인하는 그런 예제 입니다. a,b,c,d 라는 점이 네개 있고 a는 -1이고 나머지 b,c,d가 1인 OR 분류 문제라 볼수 있습니다. 여기서는 예시로 가중치가 1,1일 때 네 개의 샘플 중에 c=0,1을 입력한 경우에 출력이 1이어서 옳게 인식한다고 볼 수 있습니다.

![슬라이드3](https://user-images.githubusercontent.com/37301677/107141161-5fd24e00-696a-11eb-84ff-c0b1ab4c1eb9.PNG)

<h2>예제2: AND 분류</h2>

![슬라이드4](https://user-images.githubusercontent.com/37301677/107141162-5fd24e00-696a-11eb-9925-1e2ccb096281.PNG)

AND 분류도 OR 분류와 같은 방식으로 퍼셉트론과 결정 직선을 구할 수 있습니다.

![슬라이드5](https://user-images.githubusercontent.com/37301677/107141163-606ae480-696a-11eb-8fe3-ce6e89ca8db1.PNG)



<br>



<h1>퍼셉트론 학습</h1>



퍼셉트론의 구조와 동작을 이해했는데, 이제 퍼셉트론을 어떻게 학습할 수 있을까요? 

학습이란 훈련 집합이 주어졌을 때 주어진 훈련 집합을 옳게 분류하는 퍼셉트론을 찾는 것 입니다.

알고리즘은 다음과 같이 됩니다.

- 1단계: 분류기 구조를 정의하고 분류 과정을 수학식으로 표현한다. 

- 2단계: 분류기의 품질을 측정할 수 잇는 비용 함수를 정의한다고 하고 

- 3단계 비용 함수를 최대 또는 최소로 하는 파라미터 쎄타를 찾기 위한 알고리즘을 설계합니다. 



알고리즘을 더 자세히 설명 해보겠습니다.

<h3>1단계</h3>

![슬라이드1](https://user-images.githubusercontent.com/37301677/107142028-9fe7ff80-696f-11eb-9def-eff09917ad85.PNG)

매개변수를 퍼셉트론이 구성하는 가중치와 바이어스 w와 b라고 말할 수 있습니다.

<h3>2단계</h3>

![슬라이드2](https://user-images.githubusercontent.com/37301677/107142029-a0809600-696f-11eb-8a50-960bd58f07c2.PNG)

여기서는 퍼셉트론이 발생하는 오류율을 비용함수로 정의를 했고, 오류율이 작을수록 분류를 더 잘 한다고 볼 수 있어 분류기의 품질이 더 좋다고 말할 수 있습니다. 

X는 샘플이고 Y는 오분류된 샘플의 집합이라고 할 수 있습니다. 이 때 오분류된 샘플 x가 w1에 속한다면 tk=1이고 오분류 되었으므로 wx+b<0 입니다. 따라서 전체식은 양수가 됩니다. 오분류된 샘플이 w2에 속하는 경우도 마찬가지 입니다. 따라서 비용함수는 항상 양수가 됩니다. 또한 오분류된 샘플의 개수가 많을수록 비용함수가 커지고, Y가 공집합일 때는 즉 오분류된 샘플이 없을 때는 당연히 비용함수가 0이 됩니다. 

<h3>3단계</h3>

![슬라이드3](https://user-images.githubusercontent.com/37301677/107142030-a1192c80-696f-11eb-9151-eafd766c5d02.PNG)

앞서 정의한 비용함수를 가지고 최적의 파라미터를 찾으면 됩니다. 이런 파라미터를 찾는 방법은 Gradient descent로 찾으면 됩니다. 

![슬라이드4](https://user-images.githubusercontent.com/37301677/107142031-a1192c80-696f-11eb-971a-c3ed48d15019.PNG)
![슬라이드5](https://user-images.githubusercontent.com/37301677/107142032-a1b1c300-696f-11eb-8836-e1d484bde7e3.PNG)

초기해를 설정 한 후에 멈춤조건이 만족될 때가지 현재 해를 그래디언트의 –방향으로 조심씩 이동한다고 하는데요. 그래디언트는 어떤 함수의 기울기라고 할 수 있고, 또 이것의 – 방향은 아래로 내려가는 방향이기 때문에 그렇다고 볼 수 있습니다. 

![슬라이드6](https://user-images.githubusercontent.com/37301677/107142164-50ee9a00-6970-11eb-89bb-059bb618fb27.png)

최종적으로 이 퍼셉트론의 값이 0보다 크면 부류 w1이라고 하고, 퍼셉트론의 값이 1보다 작으면 부류 w2이라고 합니다. 





<br>

<h1>코드 예제</h1>

코랩에서 돌릴 수 있는 OR 분류 문제에 대한 코드를 작성해보았습니다.



우선 훈련 집합에 필요한 X, Y 데이터 쌍을 준비합니다.

```python
import numpy as np

X = np.array([[0,0], [1,0], [0,1], [1,1]])
Y = np.array([-1,1,1,1])
```



그 다음에는 퍼셉트론의 weight와 bias를 하나의 어레이로 표현했습니다. weight과 bias는 1로 초기화 되었습니다.



```python
w=np.array([1. , 1. , 1.])  # [bias, w1, w2]
```



퍼셉트론의 전방 계산을 하는 forward 함수, 부류를 예측하는 predict 함수 입니다.

```python
def forward(x):
  return np.dot(x, w[1:]) + w[0]

def predict(X):
  return np.where(forward(X) > 0, 1, -1)
```



함수를 업데이트 하고 학습 전후를 비교합니다.



```python
print("predict (before traning)", w)

for epoch in range(50):
  for x_val, y_val in zip(X, Y):
    update = 0.01 * (y_val - predict(x_val))
    w[1:] += update * x_val
    w[0] += update

print("predict (after traning)", w)
```



<br>

코드는 해당 링크를 해서 직접 확인하실수도 있습니다. [colab](https://colab.research.google.com/drive/1xdaW8yNNIxS0Lic-4VN1Znvj0yO0_BHH?usp=sharing)



<br>



지금까지 퍼셉트론에 대한 내용을 정리 해보았습니다.  저도 공부해나가는 단계라 부족한 점이 있을 수 있습니다.  질문이나 지적, 요청해주실 부분이 있다면 댓글이나 메일 부탁드립니다.

읽어주셔서 감사합니다. 😃



