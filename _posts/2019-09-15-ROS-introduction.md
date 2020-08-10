---
layout: post
title: 1. ROS introduction
subtitle: ROS란? ROS에 대한 소개, ROS 개념
date:   2019-09-15 22:02:51
author: Subin Yang
categories: ROS
comments: true
---

> ROS(Robot Operating System)에 대해 연재합니다.
>
> <h2>Contents</h2>
><b>[1. ROS Introduction](https://subinlab.github.io/ros/2019/09/15/ROS-introduction.html)</b>
> 
>[2. ROS basic concepts](https://subinlab.github.io/ros/2019/09/16/ROS-basic-concept.html)
> 
>[3. ROS step by step으로 설치하고 실습하기](https://subinlab.github.io/ros/2018/12/28/ROS-step-by-step.html)
> 
>[4. ROS TF (Transformation)](https://subinlab.github.io/ros/2019/04/04/ROS-tf.html)

<br>

이번 포스팅에서는 ROS란 무엇인가? 에서부터 시작하여 ROS의 목적, 구성요소, 기능, 생태계, 버전 정보 등을 알아보고자 합니다.

1. ROS란?
2. ROS의 목적
3. ROS의 구성요소
4. ROS의 기능



--------

<h1>1. ROS란 무엇일까요?</h1>
ROS는 Robot Operating System의 약자입니다. ROS는 오픈소스 메타 오퍼레이팅 시스템(Open source Meta Operating System)이라고 부릅니다. 메타 오퍼레이팅 이라는 용어는 ROS 측에서 만들어서 사용하는 용어라고 합니다.

메타 오퍼레이팅 시스템에 앞서, OS (운영체제, Operating system)에 대해서 알아보도록 하겠습니다. OSs는 하드웨어와 소프트웨어를 연결해줍니다. 소프트웨어 개발을 해서 운영체제에 넘겨주면 하드웨어가 알아서 하는 방식입니다. 윈도우용 어플리케이션을 개발을 해서 윈도으 운영체제에서 이용하는 것처럼 말이죠.

ROS는 operating system이라는 운영체제의 의미가 이름에 들어갔지만, 실제 운영체제는 아닙니다. ROS는 OS 위에 설치하여 사용하는 플랫폼이라고 할 수 있습니다. OS 위에 설치하지만, ROS는 이렇게 OS에서 제공하는

- 하드웨어 추상화
- 저수준 기기 제어
- 프로세스간 메세지 전달
- 패키지 관리 기능

등이 구현되어 있고 관련한 기능들을 제공합니다. 

ROS는 소프트웨어 프래임워크(Software Framework)이며 미들웨어(middleware)라고 할 수 있습니다. OS 위에 설치하여 ROS의 기능들과 OS 사이를 연결해주기 때문입니다. 지금 개발되어있는 로봇들은 다양한 OS를 사용하고 있습니다. 아직 로봇 산업의 개발이 발전 단계에 있어, 로봇 자체에 대한 OSemfdms 통일이 되지 않았습니다. 로봇의 운영체제들이 너무 다르지만, ROS를 사용하여 공통적으로 OS 위에 올라갈 수 있는 장점이 있습니다. 

서로 다른 로봇 센서에 맞는 메세지들을 ROS에서 사용할 수도 있고, 유저가 메세지를 만들어서 사용할 수 있습니다. 이러한 메세지를 만들고 ROS의 구성요소 중 하나인 노드로 메세지 정보를 교환할 수 있어 여러 유저들과 협업 개발을 하여 복잡한 프로그램을 만들 수 있습니다. 





<h1>2. ROS의 목적</h1>
ROS의 목적은 로봇 소프트웨어 공동 개발 생태계를 만들자는 것 입니다. 또한 로봇의 연구 개발에 있어서 코드 재사용을 편리하게 하는 목적도 있습니다. ROS는 앞서 말했듯이 다른 OS 위에 설치할 수 있는 미들웨어이며, 다양한 패키지 기능들을 제공하고 있고, 유저가 작은 프로그램을 만들어 복잡한 프로그램에 정보를 전달할 수 있기 때문입니다.

- 분산 프레임워크
- 가벼움
- ROS에 의존적이지 않은 라이브러리
- 언어 독립성
- Scailing

 이러한 메타 오퍼레이팅 시스템 역할을 하는 ROS의 구성요소들이 있습니다.

- 디바이스 드라이버
- 라이브러리
- 디버그 툴
- 메세지 통신 툴
- 컴파일러
- 인스톨러
- 패키지 생성 및 릴리즈



<h1>3. ROS의 구성요소</h1>
ROS에는 다음과 같은 구성요소들이 있습니다. 

- 클라이언트 레이어
- 로보틱스 어플리케이션
- 로보틱스 어플리케이션 프레임워크
- 통신 레이어
- 하드웨어 인터페이스 레이어
- 소프트웨어 개발 툴
- 시뮬레이션





<h1>4. ROS의 기능</h1>
ROS의 기능들을 크게 3가지로 살펴보겠습니다.

<h3>1. 통신 기능</h3>
- 메세지 parsing
- 메세지 recording and re-playing: 상황 전체를 recording 하는게 중요합니다. sensor로 부터 들어오는 모든 데이터들을 녹음하듯이 recording 할 수 있습니다. ex) 이런 영상이 들어올 때 이런 레이저가 들어오고... 메세지 recording한 파일 이름 = ros bag
- 언어의 독립성: 메세지를 통해 서로 다른 프로그래밍 언어 사이에서 통신할 수 있습니다.
- 분산 프레임워크

<h3>2. 로봇, 센서와 관련된 기능</h3>
- standard message: 카메라, IMU 등등
- gedometry libraries: tf(transformation, 좌표변환)
- robot description
- diagnose system
- sensing/recognition
- navigation
- manipulation

<h3>3. 개발의 편의를 위한 툴</h3>
- command line tools: GUI가 없는 ROS command
- rviz
- rqt
- gazebo




