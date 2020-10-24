---
layout: post
title: Linux 우분투에 docker 설치하기 docker 설치 방법
subtitle: Linux Ubuntu docker install
date:   2020-10-24 17:55:55
author: Subin Yang
category: [Linux]
tags: [Linux]

---





> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Flinux%2F2020%2F08%2F18%2FLinux-ffmpeg.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> 안녕하세요. 모카의 머신러닝 입니다.
>
> 오늘은 개발환경에 많이 사용되는 docker를 설치하려고 합니다.
>
> 저는 Linux 계열의 우분투 OS를 사용하여 우분투 관련 docker 패키지를 설치했습니다.

<br>



[Install Docker Engine on Ubuntu]([docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/))



![docker](https://user-images.githubusercontent.com/37301677/97077810-b61cba80-1621-11eb-8c30-03afcb168293.PNG)



docker 공식 홈페이지 문서를 참고하시면 좋습니다.

<br>

설치는 다음과 같은 명령어를 입력하시면 바로 설치됩니다.





```shell
$ sudo apt-get update

$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
    
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

$ sudo apt-key fingerprint 0EBFCD88

$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
 

$ sudo apt-get update

$ sudo apt-get install docker-ce docker-ce-cli containerd.io

$ sudo docker run hello-world

```



<br>

이제 설치가 완료되었습니다.

아래는 터미널에 $docker 커맨드를 치면 나오는 화면입니다.

다음과 같이 나오면 정상적으로 설치가 된 것입니다.

 <br>



![docker_capture](https://user-images.githubusercontent.com/37301677/97077811-b74de780-1621-11eb-9e80-e87d797ddbcd.png)





<br>

docker 설치 수고하셨습니다!

궁금하신 점이나 문의하실 점은 댓글로 남겨주세요. 

글 읽어주셔서 감사합니다. :) 