---
layout: post
title: Navigating the ROS Filesystem
subtitle : ROS 파일시스템 탐색하기
date:   2018-12-28 00:07:34
author: Subin Yang
categories: ROS
comments: true

---





> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Fros%2F2018%2F12%2F28%2FROS-navigating-file-system.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> ROS 파일 시스템 탐색에 대한 내용을 포스팅 합니다.





<h2>ROS 파일 시스템 탐색</h2>

<em>rospack</em> : 패캐지의 정보 제공 (패키지 경로 반환 등)

<em>roscd</em> : 패키지 이름으로 폴더 이동

<em>roscdlog</em> : ROS의 로그파일이 저장되는 폴더로 이동

<em>rosls</em> : 사용자가 지정한 패키지나 경로에서 ls 실행

<em>Tab 자동완성</em>

<br>

<h3>1.rospack 사용하기</h3>

```
예시
$ rospack find roscpp

결과
->
```

<br>



<h3>2.roscd 사용하기</h3>


```
예시
$ roscd roscpp
$ pwd

결과
->
```

<br>

<h3>3.roscd log 사용하기</h3>


```
예시
$ roscd log

결과
->
```

<br>

<h3>rosls 사용하기</h3>


```
예시
$ rosls roscpp_tutorialas

결과
->
```



<br>

