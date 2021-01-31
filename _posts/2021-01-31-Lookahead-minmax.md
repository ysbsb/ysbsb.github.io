---
layout: post
title: Lookahead-Minmax 논문 리뷰 - Taming GANs with Lookahead-Minmax
subtitle: Lookahead-Minmax paper review - Taming GANs with Lookahead-Minmax 
date:   2021-01-31 15:37:37
author: Subin Yang
categories: GAN
tags: [GAN]
comments: true
use_math: true
---







> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Fgan%2F2021%2F01%2F18%2FGAN-Dissection.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> 안녕하세요 모카의 머신러닝 입니다. 이번에는 <em><strong>Taming GANs with Lookahead-Minmax</strong></em>, ICLR 2021 에 대해 리뷰합니다. [paper](https://arxiv.org/abs/2006.14567)
>
> 이번에 accepted된 ICLR 2021 paper 목록들은 이곳에서 참고했습니다. [github](https://github.com/evanzd/ICLR2021-OpenReviewData)



<br>

# Lookahead-minax 연구 요약



- Minmax optimization 문제 (GAN의 목적 함수)를 위한 Lookahead-minnax 라는 방법을 제안합니다.
- Lookahead를 사용하면 수렴이 더 잘 된다고 합니다.



<br>





# Related work: Lookahead Optimizer

사전 논문은 Lookahead 라는 새로운 Optimizer를 제안한 논문입니다.

<strong>Lookahead Optimizer: k steps forward, 1 step back</strong> (NIPS 2019)  [paper](Lookahead Optimizer: k steps forward, 1 step back)

<img src="https://user-images.githubusercontent.com/37301677/106376506-50d32500-63d9-11eb-846e-84693fe0f1d9.png" alt="image"  />

그림을 먼저 살펴보면 파란색 업데이트 (fast update)와 빨간색 업데이트 (slow update)가 있습니다.

그래디언트에서 현재 점이 start 지점입니다. 이 때 fast step으로 k번 업데이트 할 수 있습니다. (파란색 선) k번 이후에는 끝지점에 도달한 것을 볼 수 있습니다.

이 때 끝지점과 시작지점의 중간 지점이, 실제 업데이트 지점이라고 볼 수 있습니다. 끝지점에서 뒤로 1 step 돌아간다는 의미로 backtracking 한다고 표현하기도 합니다. 

만약에 lookahead라는 방법을 사용하지 않았으면, 오른쪽 위의 그림과 같이 파란색 선을 따라서 작은 step으로 연속적으로 앞으로 진행했을 것입니다. Lookahead라는 방법을 사용하면 중간 지점을 다시 시작 지점으로 여기기 때문에, 오른쪽 아래와 같은 그림이 됩니다.

그래디언트 분포에서 노란색으로 갈수록 test 정확도가 높은 것 입니다. 이렇게 Lookahead를 통해 그래디언트가 안쪽으로 오는 효과가 있어서, rotation을 더 잘 한다라고 표현하기도 합니다.



![222](https://user-images.githubusercontent.com/37301677/106376544-a4de0980-63d9-11eb-9a42-426d14b9db63.PNG)



알고리즘은 다음과 같습니다.

1. 현재 iterate를 복사함
2. k>=1 번 동안 업데이트 함
3. 실제 업데이트는, 두 개의 iterates (현재와 예측된 것) 사이의 선에 있는 점을 통해 얻어진다.

번호에 따라서 수식에 표현을 했습니다.



그리고 일반적인 SGD 업데이트와 다르게, 두 개의 파라미터가 추가 됩니다.

- k: 예측 지점을 얻기 위해 사용되는 step 수

- alpha: 예측된 iterate를 위해 얼마나 step을 조절할지에 대한 파라미터





<br>

# Lookahead-minmax 방법



본 논문에서는 GAN Objective를 다음과 같이 표현합니다.

![111](https://user-images.githubusercontent.com/37301677/106376528-81b35a00-63d9-11eb-827c-ec5786c0bdcc.PNG)





GAN의 objective 또한 그래디언트의 그림으로 표현할 수 있습니다. 

![223](https://user-images.githubusercontent.com/37301677/106376540-9ee82880-63d9-11eb-9d05-de62980f194e.PNG)

알고리즘은 오른쪽과 같습니다.  Generator와 Discriminator 모두 같은 방법으로 업데이트 합니다.

1. 파란색 내부 루프에서 fast weight update 를 진행합니다.
2. 이후 초록색 박스에서 slow weight update를 한다면 초록색 선을 따라 이동할 것 입니다.
3. fast weight update를 통해 예측된 지점과 slow weight update를 따르는 선 사이의 중간 지점으로 backtracking 합니다.





<br>



# Results



![image](https://user-images.githubusercontent.com/37301677/106376488-1e292c80-63d9-11eb-8c84-55d625c5c053.png)
![image](https://user-images.githubusercontent.com/37301677/106376489-241f0d80-63d9-11eb-9419-7fddedd4d4a7.png)

<br>





지금까지 Lookahead for Minmax 논문 리뷰를 해보았습니다. 저도 논문을 읽으며 공부해나가는 단계라 부족한 점이 있을 수 있습니다. 같이 잘 공부해나가요! 질문이나 지적, 요청해주실 부분이 있다면 댓글이나 메일 부탁드립니다.

읽어주셔서 감사합니다. 😃



