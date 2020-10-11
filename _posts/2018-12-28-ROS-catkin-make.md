---
layout: post
title: ROS catkin_make
subtitle : ROS 파일 빌드하기
date:   2018-12-28 00:28:05
author: Subin Yang
categories: ROS
comments: true

---



> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Fros%2F2018%2F12%2F28%2FROS-catkin-make.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> ROS 파일 빌드를 위한 catkin make에 대해 포스팅 합니다.



<h2>ROS 파일 빌드하기</h2>
<em>catkin_make</em>는 cmake의 ROS 버전이라고 볼 수 있습니다.

ROS에서는 아래의 catkin_make 명령어를 통해 패키지를 빌드합니다.

```
$ cd ~/catkin_ws/
$ catkin_make
```

<br>

이는 다음과 같은 과정을 catkin_make 과정에 모두 포함시켰다고 보면 됩니다.


```
$ mkdir build
$ cd build
$ cmake ../
$ make
```

<br>

