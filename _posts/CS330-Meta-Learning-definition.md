---
layout: post
title: [CS330-Stanford-Meta-Learning] 메타러닝(Meta Learning)이란? 메타러닝 문제 정의와 설명, 적용
subtitle: [CS330-Stanford-Meta-Learning] Meta Learning problem definitions and applications
category: [CS330_Stanford_Meta_Learning]
tags: [CS330_Stanford_Meta_Learning]
comments: true
---





> 
>
> 안녕하세요 모카의 머신러닝 입니다. 이번 포스팅에서는 Stanford Chelsea Finn 교수님의 Deep Multi-Task and Meta Learning 강의 Lecture 1에 대한 리뷰와 정리에 대해서 이야기 합니다.
>
> lecture website
>
> http://cs330.stanford.edu/fall2019/index.html
>
> lecture slide
>
> http://cs330.stanford.edu/fall2019/slides/cs330_lecture1.pdf
>
> lecture video
>
> 

<br>







# Deep Multi-Task and Meta-Learning







<h2>주제</h2>

1. 문제 정의
2. 멀티 태스크 러닝 기초
3. 메타 러닝 알고리즘: 블랙박스 접근법, 최적화 기반 메타 러닝, metric learning
4. 계층적 베이지안 모델과 메타 러닝
5. 멀티 태스크 강화학습, goal-conditioned 강화학습, 계층 강화학습
6. 메타 강화학습
7. 열린 문제들, 초대 강의들, 연구 이야기

이 분야들을 딥러닝, 강화학습 분야를 강조하고 있습니다.



<h2>다루지 않을 주제들

강의의 시간 관계상 모든 분야를 다 커버할 수가 없어서, Auto ML topics를 커버하지 않는다고 합니다.

- architecture search
- hyperparameter optimization
- learning optimizers

이 분야들은 딥러닝 분야의 접근법을 강조하는 분야입니다.



<h2>Course Format</h2>

3가지 종류의 세션이 있습니다.

- lecture (9)
- student reading: presentations & discussions (7)
- guest lectures (3)







<h1>연구 분야</h1>



Chelsea Finn 교수님의 연구 분야와, 왜 multi-task learning과 meta-learning에 관심을 가지는지 설명합니다.





<h2>어떻게 우리는 에이전트가 실제 환경에서 스킬을 배우도록 할 수 있을까?



교수님은 다음과 같이 로봇에 대한 여러 연구들을 하셨습니다. 

![Screenshot from 2020-11-13 16-56-00](https://user-images.githubusercontent.com/37301677/99043107-33a86a80-25d1-11eb-8245-c1509e8ffb03.png)



왜 로봇인가? 로봇은 지능에 대한 것들을 우리에게 가르쳐준다! 그리고 아래와 같은 이유들이 있습니다.

로봇은

- 현실 문제에 직면해 있고
- 할 일, 목적, 환경 등에 반드시 일반화 되어야 하고
- 감독이 주어지지 않을 때 잘 할 수 있는 일반적인 상식을 이해하기

가 필요하기 때문입니다.

 

<h2>박사 처음 시절</h2>

교수님의 박사를 하실 때 로봇을 활용한 연구 프로젝트를 진행하셨다고 합니다.

처음에 로봇은 눈이 없는 상태로 비행기 장난감을 조립하는 문제에 대해 학습했다고 합니다.

<img src="https://user-images.githubusercontent.com/37301677/99043291-79653300-25d1-11eb-8eaa-a91e602951d8.png" alt="Screenshot from 2020-11-13 16-58-03" style="zoom: 33%;" />

나중에 눈이 필요한 비전 문제에 관심이 가게 되었다고 합니다.

동그라미, 네모 모양의 교구를 조립하는 일, 텀블러 뚜겅을 닫는일, 물건을 옮기는 일 등 입니다.

<img src="https://user-images.githubusercontent.com/37301677/99043293-7a966000-25d1-11eb-9992-d4ec11a44ea4.png" alt="Screenshot from 2020-11-13 16-58-15" style="zoom: 33%;" />



<img src="https://user-images.githubusercontent.com/37301677/99043294-7b2ef680-25d1-11eb-8c27-e72ab90d8d1d.png" alt="Screenshot from 2020-11-13 16-58-27" style="zoom:33%;" />





<h2>한 가지를 학습 하는 일</h2>



![Screenshot from 2020-11-13 17-00-02](https://user-images.githubusercontent.com/37301677/99043531-baf5de00-25d1-11eb-8be0-3e8efed51134.png)



한가지를 학습하는 일은 하나의 환경에서 처음부터 시작해서 학습하는 것입니다.



<img src="https://user-images.githubusercontent.com/37301677/99043533-bcbfa180-25d1-11eb-8850-015529fa9686.png" alt="Screenshot from 2020-11-13 17-00-15" style="zoom:33%;" />

하지만 예전에 동료가 로봇이 하키를 하는 문제를 학습하기 위해, 로봇이 퍽을 한번 칠 때 사람이 다시 퍽을 원 위치로 되돌리면서, 자신이 더 많이 움직인 일화를 말해주면서 이러한 방법은 효율적이지는 않다고 설명합니다.



앞서 설명한 부분에 덧붙입니다.



한 가지를 학습하는 일은 하나의 환경에서 처음부터 시작해서 학습하면서 자세한 감독이나 가이드에 의존하는 일읍니다.

이것은 강화학습과 로보틱스 문제에 국한되는 일은 아닙니다.

<img src="https://user-images.githubusercontent.com/37301677/99043713-f7c1d500-25d1-11eb-8585-a77f6f814be6.png" alt="Screenshot from 2020-11-13 17-01-40" style="zoom: 33%;" />

Machine translation, speech recognition, object detection 과 같이 다양한 문제들이 아직도 하나의 문제를 시작부터 디테일한 감독과 함께 학습된다고 합니다.



<img src="https://user-images.githubusercontent.com/37301677/99043719-f8f30200-25d1-11eb-8b36-52c122b01968.png" alt="Screenshot from 2020-11-13 17-01-57" style="zoom:33%;" />



하지만 사람은 제네럴리스트 입니다!







<h1>왜 우리는 deep multi-task & meta-learning에 관심을 가져야 할까요?</h1>



Why shoud we care about deep multi-task & meta-learning?

beyond the robots and general-purpose ML systems









기존의 컴퓨터 비전: 직접 설계된 특징들을 사용함

현대의 컴퓨터 비전: end-to-end 학습

<img src="https://user-images.githubusercontent.com/37301677/99043885-3b1c4380-25d2-11eb-9a48-fc0de1221a09.png" alt="Screenshot from 2020-11-13 17-03-22" style="zoom:33%;" />







딥러닝은 정제되지 않은 입력들을 (픽셀, 언어, 센서에서 읽은 값 등) 직접 설계된 특징들들을 사용하지 않고, 도메인 지식을 적제 사용해서 다룰 수 있게 해주었습니다.



객체 인식을 위한 딥러닝 예시

<img src="https://user-images.githubusercontent.com/37301677/99043890-3c4d7080-25d2-11eb-94fd-f8aa2d520a0e.png" alt="Screenshot from 2020-11-13 17-03-46" style="zoom:33%;" />





머신 번역을 위한 딥러닝 예시

<img src="https://user-images.githubusercontent.com/37301677/99043891-3ce60700-25d2-11eb-998b-b6003936c266.png" alt="Screenshot from 2020-11-13 17-03-53" style="zoom:33%;" />









<h2>하지만 왜 deep multi-task learning 과 meta-learning 일까요?</h2>



<h3>큰 데이터셋을 가지고 있지 않으면 어떨까요?</h3>

<img src="https://user-images.githubusercontent.com/37301677/99044016-70c12c80-25d2-11eb-951f-221f967d94d6.png" alt="Screenshot from 2020-11-13 17-05-05" style="zoom:33%;" />

-> 각각의 질병, 각각의 로봇, 각각의 사람, 각각의 언어, 각각의 task를 위해 처음부터 스크래치로 학습하는 것은 실용적이지 않다.

<h3>가지고 있는 데이터가 long tail (아래 그래프와 같은 모습)이면 어떨까요?</h3>

<img src="https://user-images.githubusercontent.com/37301677/99044020-71f25980-25d2-11eb-8bda-92d897159ac2.png" alt="Screenshot from 2020-11-13 17-05-13" style="zoom:50%;" />

-> 이런 셋팅은 기존의 머신러닝의 패러다임을 부쉰다.



<h3>새로운 것을 빨리 배울 필요가 있으면 어떨까요?</h3>

-> 새로운 사람, 새로운 일이나, 새로운 환경 등에 대해서







<h2>잠깐 예시로 쉬어가기</h2>



test datapoint에 있는 그림이 Braque의 그림이라고 생각하시나요 Cezanne의 그림이라고 생각하시나요?

<img src="https://user-images.githubusercontent.com/37301677/99044420-170d3200-25d3-11eb-83fa-b3e1cec2ec33.png" alt="Screenshot from 2020-11-13 17-10-06" style="zoom: 33%;" />



이렇게 적은 수의 그림 (training data)만 보고 test datapoint를 예측하는 것이 few-shot learning 이라고 합니다.

<img src="https://user-images.githubusercontent.com/37301677/99044422-17a5c880-25d3-11eb-81e6-afc27018fc2b.png" alt="Screenshot from 2020-11-13 17-09-38" style="zoom:50%;" />

이것을 어떻게 수행할 수 있을까요? 사전 지식을 활용하면서 할 수 있습니다!







<h2>multi-task learning 적용할 수 있는 곳</h2>





- 일반적인 목적의 AI 시스템을 원하면 어떨까?

- 큰 데이터셋을 가지고 있지 않으면 어떨까?

- 데이터가 long tail을 가지고 있으면 어떨까?

- 새로운 것을 빨리 배워야 한다면 어떨까?

<img src="https://user-images.githubusercontent.com/37301677/99044671-79fec900-25d3-11eb-86fb-4f028b4ba176.png" alt="Screenshot from 2020-11-13 17-12-48" style="zoom:33%;" />



이것들이 multi-task learning을 적용할 수 있는 곳 들입니다!









<h1>task란 무엇인가?</h1>



이제부터 task는 데이터셋과 loss function 이 주어졌을 때 최적의 모델을 찾는 것이라고 합니다.

<img src="https://user-images.githubusercontent.com/37301677/99047247-4887fc80-25d7-11eb-94dc-c2a65f9487fe.png" alt="Screenshot from 2020-11-13 17-39-05" style="zoom:33%;" />







<h2>중요한 가정</h2>





나쁜 소식

다른 task들은 몇가지 구조를 공유해야 합니다.

만약 이것이 되지 않는다면, single-task learning을 학습하는 것이 더 좋을 것 입니다.





좋은 소식

구조를 공유하는 많은 task들이 있습니다!

![Screenshot from 2020-11-13 17-39-14](https://user-images.githubusercontent.com/37301677/99047251-49b92980-25d7-11eb-9cd9-7cd963f1d595.png)



task들이 연관되지 않아 보일지라도

- 실제 데이터는 물리 법칙에 의존한다.
- 사람들은 각자의 의사가 있는 유기체들이다. 각자 의견이 다르기도 하고 같기도 하다.
- 영어 언어 데이터는 영어의 법칙에 의존한다.
- 언어들은 비슷한 목적을 위해 개발되었다.

이러한 것들이 랜덤 task들 보다 더 좋은 구조를 이끈다고 합니다.







<h2>비공식적인 문제 정의</h2>



더 공식적인 정의는 다음 lecture 시간에 한다고 합니다. 

정확한 설명을 위해서 영어 설명을 같이 첨부합니다.



<h3>멀티 태스킹 러닝 문제</h3>

모든 task들을 각각 독립적으로 학습하는 것 보다 더 빨리 또는 더 효율적으로 학습합니다.



<h3>메타 러닝 문제</h3>

이전 task들에 대한 데이터나 경험이 주어졌을 때, 새로운 task를 더 빨리 그리고/또는 더 효율적으로 학습합니다.



![Screenshot from 2020-11-13 17-41-45](https://user-images.githubusercontent.com/37301677/99047405-8553f380-25d7-11eb-8f7a-ef14e57065d0.png)





<h2>멀티 태스크 러닝을 싱글 태스크 러닝으로 줄일 수 있을까?</h2>



할 수 있습니다!

task들 간에 데이터를 aggregating 하거나 단일 모델을 학습하는 것은 멀티 태스크 러닝의 한 가지 접근법 중 하나이다.



하지만, 더 잘 할 수 있습니다!

다른 task들로부터 데이터가 올 것을 알고 있다. 싱글 태스크 러닝을 할 수 도 있지만 멀티 태스크 러닝을 해야할 필요가 있다는 의미이다.





<h1>왜 지금 deep multi-task & meta learning을 배워야 하나요?</h1>



멀티 태스크 러닝과 메타 러닝은 사실상 머신러닝 연구분야에서 이전부터 지속되어 근본적인 역할을 하는 알고리즘 입니다.

<img src="https://user-images.githubusercontent.com/37301677/99047254-4a51c000-25d7-11eb-98c4-08d3a631184d.png" alt="Screenshot from 2020-11-13 17-39-29" style="zoom:50%;" />



<img src="https://user-images.githubusercontent.com/37301677/99047257-4aea5680-25d7-11eb-8e8e-fe3fc1a34ba1.png" alt="Screenshot from 2020-11-13 17-39-40" style="zoom:50%;" />





머신러닝과 딥러닝이 발전하면서, 멀티 태스크 러닝과 메타 러닝의 역할은 증가하고 있습니다.



구글 검색 쿼리에서 메타 러닝과 멀티 태스크 러닝의 수가 증가한 모습입니다.



![Screenshot from 2020-11-13 17-39-51](https://user-images.githubusercontent.com/37301677/99047259-4b82ed00-25d7-11eb-89c6-73f7e34ffdca.png)







<h3>멀티 태스크 러닝과 메타 러닝의 성공은 딥러닝을 보급하는데 중요한 역할을 합니다.</h3>





이전에는 120만개의 이미지와 라벨들을 사용하고, 4080만개의 문장의 쌍을 사용하고, 300 시간의 라벨이 된 데이터를 상용했습니다.



최근에는 3만 5000천개의 라벨된 이미지들, 1시간 이하, 15분 이하의 데이터를 사용합니다. 더 적은 수의 데이터를 학습 하는데에 멀티 태스크 러닝과 메타 러닝을 적용할 수 있습니다.



<img src="https://user-images.githubusercontent.com/37301677/99047260-4b82ed00-25d7-11eb-9541-5c23cc9184d6.png" alt="Screenshot from 2020-11-13 17-40-00" style="zoom: 33%;" />





<br>





여기까지 Stanford Chelsea Finn 교수님의 Deep Multi-Task and Meta Learning 강의 Lecture 1에 대한 리뷰와 정리에 대한 설명을 해보았습니다. 질문이나 지적, 요청해주실 부분이 있다면  댓글이나 메일 부탁드립니다.

읽어주셔서 감사합니다. 😃