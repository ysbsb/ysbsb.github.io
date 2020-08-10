---
layout: post
title: 2. ROS basic concept
subtitle: ROS 기본 개념
date:   2019-09-16 22:02:51
author: Subin Yang
categories: ROS
comments: true
---

> ROS(Robot Operating System)에 대해 연재합니다.
>
> <h2>Contents</h2>
>[1. ROS Introduction](https://subinlab.github.io/ros/2019/09/15/ROS-introduction.html)
> 
><b>[2. ROS basic concepts](https://subinlab.github.io/ros/2019/09/16/ROS-basic-concept.html)</b>
> 
>[3. ROS step by step으로 설치하고 실습하기](https://subinlab.github.io/ros/2018/12/28/ROS-step-by-step.html)
> 
>[4. ROS TF (Transformation)](https://subinlab.github.io/ros/2019/04/04/ROS-tf.html)



<br>

이번 포스팅에서는 ROS의 용어와 통신 개념에 대해서 이야기 해보도록 하겠습니다. 

1. ROS의 3가지 개념

2. ROS 용어

3. 메세지 통신

4. 파라미터

5. 좌표변환



<h1>1. ROS 3가지 개념</h1>
- Node: 하나의 실행 가능한 프로그램 단위. rosrun 하면 하나가 실행된다. Task의 덩치가 클 수록 작은 것들로 나눠서 해야 수정하기에 좋다. 업무가 주어진 것에 대해서 여러개의 노드를 만들어서 하나의 패키지로 만들어 업무를 수행하고 패키지를 릴리즈 하도록 한다.
- 패키지: 노드의 덩어리이다. 노드 여려개를 묶어서 패키지 형태로 사람들에게 공개한다. 노드 뿐만 아니라 노드를 실행하는데 필요한 여러가지 정보를 모두 담고 있다. ex) 계단을 올라간다. 나사를 뺀다. 이런것들은 패키지 단위라고 생각하면 된다. 나사를 뺄 때 detection이나 팔이 움직이는 것 들은 노드로 실행한다.
- 메세지: 통신하고 명령 내리는 것이다.







<h1>2. ROS 용어</h1>
메세지들의 종류가 여러가지가 있다. 3가지(or 4가지) topic 70프로 사용 service 20프로

![Screenshot from 2019-09-16 13-43-43](https://user-images.githubusercontent.com/37301677/64934945-eaec0900-d888-11e9-93f5-79f372ba6edd.png)
![Screenshot from 2019-09-16 13-44-13](https://user-images.githubusercontent.com/37301677/64934946-eaec0900-d888-11e9-8470-14ce4f1f07d8.png)
![Screenshot from 2019-09-16 13-44-26](https://user-images.githubusercontent.com/37301677/64934948-eaec0900-d888-11e9-8ad8-325970ff9542.png)

- topic: 단방향 메세지. 퍼블리셔 서브스크라이버 관계로 되어있다. Odometry 바퀴가 얼마나 돌아갔는지. 일대일, 일대다, 다대일, 다대다 모두 된다. 토픽은 한번 발행되면 그만하라고 할 때까지 연속적으로 계속 발행된다.
- service: 양방향 통신을 한다. 연속적이지 않고 한번 왔다갔다 하면 끝난다. 서버와 클라이언트가 있다. 클라이언트가 서비스를 리퀘스트 하고 서버가 정보를 리스폰스로 보내준다. 그럼 끝이다. 양방향 1회성 메세지.
- action: 양방형 1회성 메세지. 서버가 응답을 단계적으로 한다. 서버가 단계적으로 응답하면서 클라이언트에게 피드백을 준다. 마지막 결과를 주고 통신이 끝난다.







<h1>3. 메세지 통신</h1>
메세지 통신이 어떤 식으로 구동이 될 것인가

- master가 하나 있다. (roscore)

- 노드 1,2 (turtlesim, teleop_key) 처럼 생각해보자.

- 마스터와 노드 사이에는 맨처음 노드를 실행될 때 노드에 대한 정보를 받아놓는다. 

- 노드2를 실행될 때는 노드1에 대한 정보가 없으므로 노드1에 대한 정보를 받아 통신한다.



<h2>[메세지 통신 과정]</h2>
![Screenshot from 2019-09-16 13-46-41](https://user-images.githubusercontent.com/37301677/64934897-b37d5c80-d888-11e9-89aa-1fe4162a1450.png)
![Screenshot from 2019-09-16 13-49-16](https://user-images.githubusercontent.com/37301677/64934928-d576df00-d888-11e9-81c6-ce1fc2e00f8d.png)
![Screenshot from 2019-09-16 13-47-01](https://user-images.githubusercontent.com/37301677/64934899-b415f300-d888-11e9-842c-328d72902109.png)
![Screenshot from 2019-09-16 13-49-35](https://user-images.githubusercontent.com/37301677/64934929-d576df00-d888-11e9-9fc1-1cce96979207.png)
![Screenshot from 2019-09-16 13-47-19](https://user-images.githubusercontent.com/37301677/64934901-b415f300-d888-11e9-9c26-6af36c7e3e90.png)
![Screenshot from 2019-09-16 13-47-27](https://user-images.githubusercontent.com/37301677/64934902-b415f300-d888-11e9-897d-9b920ee1c622.png)
![Screenshot from 2019-09-16 13-47-35](https://user-images.githubusercontent.com/37301677/64934903-b4ae8980-d888-11e9-826a-cc901116f7f9.png)
![Screenshot from 2019-09-16 13-47-47](https://user-images.githubusercontent.com/37301677/64934904-b4ae8980-d888-11e9-9c5a-ad215bf6fd9e.png)
![Screenshot from 2019-09-16 13-48-04](https://user-images.githubusercontent.com/37301677/64934905-b4ae8980-d888-11e9-8991-a33f742f3c96.png)

두 개의 노드 topic 통신 과정

1. master 실행. 마스터 url이 있다.
2. subscriber 노드 실행 (turtlesim 노드 실행). 노드2에 대한 정보를 마스터에 등록을 한다. 각각의 노드에 대한 host가 있다. 노드2에 대한 ip를 마스터에 등록한다.
3. publisher 노드 실행 (teleopkey 노드 실행): 노드1 실행하고 마스터에 노드1 ip를 등록한다. 
4. 받을 수 있는 토픽은 정해져있다. 코드에 받을 수 있는 토픽은 무엇이다라고 명시되어있다. 노드2에서 이런 토픽을 발행하는 노드가 있으면 알려달라고 노드1에 대한 정보를 마스터에게 요청한다. 
5. 노드2에서 노드1에서 연결을 요청한다. 
6. 노드1에서 노드2에게 연결 요청에 대한 응답을 한다.
7. TCPROS 통신을 한다. 일종의 TCP 통신. 통신 요청을 accept 한다. 
8. 노드1이 발행하는 토픽을 보내준다. 

서비스는 연결 과정이 끝나면 토픽과 다르게 종료가 된다.



<h3>[노드의 정보] </h3>
- 노드 이름
- 발행하고 서브스크라이브 하는 토픽 이름
- 메세지 정보
- ip



<h1>4. 파라미터</h1>
 전역 변수처럼 생각하면 된다. 이번 노드에서는 parameter를 3으로 하겠습니다. ROS 마스터로부터 파라미터로 값을 받아 실행하는 것처럼 한다. 그냥 코드에서 하면 코드 수정하고 컴파일하고 빌드하고 다 해야한다. 노드끼리 통신을 하는 것은 아니고 마스터랑 통신을 하는 것이다. 실험할 때 숫자 한두개 바꿔보면서 할 때 좋다.



<h1>5. TF</h1>
좌표변환. 휴머노이드 로봇의 관절마다 좌표가 있다. 손끝을 움직이기 위해서 팔이나 다른 곳도 다 같이 움직이고, 이런것들이 연결되어 계산하고 결정된다. 관절과 좌표가 연결이 된 것이다.

[ROS tf (transformation) - 수빈 블로그](https://subinlab.github.io/ros/2019/04/04/ROS-tf.html)





