---
layout: post
title: ROS Tutorial [3] ROS step by step Install and Run
subtitle: ROS step by step으로 설치하고 실습하기
date:   2019-09-17 03:10:51
author: Subin Yang
categories: ROS
comments: true
---

> ROS(Robot Operating System)에 대해 연재합니다.
>
> <h2>Contents</h2>
>[1. ROS Introduction](https://subinlab.github.io/ros/2019/09/15/ROS-introduction.html)
> 
>[2. ROS basic concepts](https://subinlab.github.io/ros/2019/09/16/ROS-basic-concept.html)
> 
><b>[3. ROS step by step으로 설치하고 실습하기](https://subinlab.github.io/ros/2018/12/28/ROS-step-by-step.html)</b>
> 
>[4. ROS TF (Transformation)](https://subinlab.github.io/ros/2019/04/04/ROS-tf.html)

<br>

(New Update Version. Original 2018-12-28)

<br>

이번 포스팅에서는 ROS 설치부터, ROS 통신에서 기본적인 publisher and subscriber, server and client를 테스트 하는 것 까지 step by step으로 진행합니다.

1. <em>ROS 설치</em>
2. <em>Workspace 생성</em>
3. <em>ROS Package</em>
4. <em>ROS Node</em>
5. <em>ROS Topic</em>
6. <em>ROS Service</em>
7. <em>ROS msg, srv</em>
8. <em>Publisher and Subscriber</em>
9. <em>Server and Client</em>



<br>

--------

<h2>ROS 설치</h2>
<h4>우분투 16.04, ROS Kinetic</h4>
```
$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
$ sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
$ sudo apt-get update
$ sudo apt-get install ros-kinetic-desktop-full
```



<h4>rosdep 초기화</h4>
rosdep을 사용하면 컴파일하려는 소스에 대한 시스템 종속성을 쉽게 설치할 수 있습니다.

```
$ sudo rosdep init
$ rosdep update
```

<br>



<h4>환경 변수 설정</h4>
새 shell이 시작될 때마다 ROS 환경변수가 bash 세션에 자동으로 추가되도록 설정합니다.



bashrc 파일을 수정해줍니다.

```
$  gedit ~/.bashrc
```



bashrc 파일 맨 하단에

아래의 text를 모두 적습니다.


```shell

 alias eb='nano ~/.bashrc'
 alias sb='source ~/.bashrc'
 alias cw='cd ~/catkin_ws'
 alias cs='cd ~/catkin_ws/src'
 alias cm='cd ~/catkin_ws && catkin_make'

 source /opt/ros/kinetic/setup.bash
 source ~/catkin_ws/devel/setup.bash

 export ROS_MASTER_URI=http://localhost:11311
 export ROS_HOSTNAME=localthost

 #exprot ROS_MASTER_UTR=https://192.168.1.100:11311
 #export ROS_HOSTNAME=192.168.1.100
```

파일을 수정하고 저장한 후 마무리 해줍니다.

```
$  source ~/.bashrc
```

<br>



<h4>빌드 패키지에 대한 종속성 설치</h4>
ROS 명령어를 사용하는데 필요한 의존성 패키지 설치를 설치합니다.

설치하면 이후에 사용할 roscore, rospack, roscd 등의 명령어를 사용할 수 있습니다.

```
$  sudo apt install python-rosinstall python-rosinstall-generator python-wstool build-essential
```

<br>



<h4>ROS 패키지를 찾거나 사용하는데 문제가 있을 때</h4>
ROS에서 사용되는 환경변수인 ROS_ROOT, ROS_PACAKGE_PATH 등이 제대로 설정되어 있는지 확인하는 명령어 입니다.

```
$  export | grep ROS
```

<br>

<br>

------------

<h2>Workspace 생성</h2>
<h4>catkin workspace 생성</h4>
```sh
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws/src
$ catkin_init_workspace

처음 작업공간을 만들었을 땐는 catkin_ws/src/ 폴더 안에 CMakeLists.txt 파일 하나가 존재합니다.
```

<br>

catkin_make 에 대한 추가 내용은 다음의 포스트를 참조해주세요.

[post] https://subinlab.github.io/2018/12/28/ROS-catkin-make.html

<br>

![7](https://user-images.githubusercontent.com/37301677/50489675-50267280-0a4c-11e9-9913-49a3c101f9ac.PNG)

![8](https://user-images.githubusercontent.com/37301677/50489667-48ff6480-0a4c-11e9-8699-b5751e68d5b1.PNG)



![9](https://user-images.githubusercontent.com/37301677/50489660-3f75fc80-0a4c-11e9-9d42-ed432e2f248b.PNG)

```sh
CMakeLists.txt 파일만 존재하지만 catkin_make 명령으로 이 작업공간을 "빌드"하는 것이 가능합니다.
cakin_make에 대한 추가 설명은 다음 링크를 참고해주세요.

$ cd ~/catkin_ws/
$ catkin_make
```

<br>



![10](https://user-images.githubusercontent.com/37301677/50489647-32f1a400-0a4c-11e9-9e31-1dd7bc0ee799.PNG)





![11](https://user-images.githubusercontent.com/37301677/50489624-1bb2b680-0a4c-11e9-9b62-115a07668d61.PNG)



![12](https://user-images.githubusercontent.com/37301677/50488150-67f9f880-0a44-11e9-9419-3a8a49e3f92a.PNG)

<br>

<br>

-------------------

<h2>ROS Package 생성/빌드</h2>
<h4>ROS Package 구성</h4>
```
workspace_folder/        -- 작업공간
  src/                   -- 소스 폴더
    CMakeLists.txt       -- catkin이 제공하는 '최상위'의 CMake 파일,
    package_1/
      CMakeLists.txt     -- package_1에 대한 CMakeLists.txt 파일
      package.xml        -- package_1에 대한 매니패스트
    ...
    package_n/
      CMakeLists.txt     -- package_n에 대한 CMakeLists.txt 파일
      package.xml        -- package_n에 대한 매니패스트
```



std_msgs, roscpp, rospy에 대한 의존성을 가지는 'beginner_tutorials' 패키지를 만들기 위해 catkin_creaet_pkg 스크립트를 사용합니다. 

 아래를 수행하면  package.xml과 CMakeLists.txt이 들어있는 beginner_tutorials 폴더가 만들어집니다. 

     $ cd ~/catkin_ws/src
     $ catkin_create_pkg beginner_tutorials std_msgs rospy roscpp
     # catkin_create_pkg <package_name> [depend1] [depend2] [depend3]


 <br>

<h4>catkin_make</h4>
     $ cd ~/catkin_ws
     $ catkin_make



rospack 명령어를 사용하여 의존성을 가지는 패키지들을 확인할 수 있습니다.

beginner_tutorials > rospy > roscpp

```
 $ rospack depends1 beginner_tutorials
 $ roscd beginner_tutorials
 $ cat package.xml
```

<br>

<br>

----------

<h2>ROS 노드 이해하기 </h2>
ROS graph의 개념을 소개하고 roscore, rosnode, rosrun 을 다루고 있습니다.

    그래프 컨셉 소개
    - Nodes
    - Messages
    - Topics
    - Master
    - rosout
    - roscore


rospack 명령어를 사용하여 의존성을 가지는 패키지들을 확인할 수 있다.

beginner_tutorials > rospy > roscpp

```
 $ roscore
```



rosnode list 명령어는 실행중인 노드를 보여줍니다.

```
 $ rosnode list
 
   /rosout
```



패키지 안의 노드를 직접 실행할 수 있게 해줍니다. (package path를 몰라도 됩니다.)

```
 $ rosrun turtlesim turtlesim_node
 
 # rosrun [package_name] [node_name]
```



```
 $ rosnode list
 
   / rosout
   / tutlesim
   
 $ rosnode ping my_turtle
```

<br>

<br>

---------

<h2>ROS Topic 이해하기</h2>
```
 $ roscore
 $ rosrun tutlesim turtlesim_node
 위는 잘 따라왔다면 원래 되어있는 상태 
 아래의 명령어부터 시작하기
 
 $ rosrun turtlesim turtle_teleop_key
 
 
 rqt_graph를 사용할 수 있도록 아래의 명령어로 패키지를 설치해줍니다. rqt_graph는 rqt 패키지의 일부입니다.
 $ sudo apt-get install ros-kinetic-rqt
 $ sudo apt-get install ros-kinetic-rqt-common-plugins
 
 $ rosrun rqt_graph rqt_graph
```

처음 위의 명령어를 입력하면 아래와 같은 그래프를 볼 수 있습니다.

 



![24](https://user-images.githubusercontent.com/37301677/50514992-3be28400-0ae5-11e9-8f6d-9ede6e1f2956.png)

파란색, 초록색 : nodes

빨간색 : topic



키보드로 거북이를 움직이게 합니다.

teleop_turtle 노드가 키보드 입력 정보를 topic에 담아 publishing을 하면, turtlesim 노드는 키보드 입력 정보를 받기 위해서 같은 topic을 subscribe 합니다.

teleop_turtle 노드와 turtlesim 노드는 하나의 topic에 대해서 통신하고 있습니다. topic의 이름은 'turtle1/command_velocity' 입니다.



```
 $ rostopic -h
 
     rostopic bw	display bandwidth used by topic
     rostopic echo	화면에 메세지들을 보여준다.
     rostopic hz	display publishing rate of topic
     rostopic list	현재 활성중인 topic의 정보를 보여준다.
     rostopic pub	publish data to topic
     rostopic type	print topic type
```

<br>

<h4>rostopic 사용하기 (Using rostopic)</h4>
- rostopic echo

  키보드 자판을 입력하지 않았을 때는 topic에 data가 publish되지 않았기 때문입니다. 키보드 자판을 입력한 후 에는 topic에 publish된 데이터가 보일 것입니다.

```
 $ rostopic echo /turtle1/cmd_vel
 
 # rostopic echo [topic]
```

![25](https://user-images.githubusercontent.com/37301677/50515765-efe60e00-0ae9-11e9-9a84-7408ea70de8b.PNG)



![26](https://user-images.githubusercontent.com/37301677/50514948-ed34ea00-0ae4-11e9-81f3-214f2af3bdbc.PNG)

rostopic echo node : /rostopic_14245_1355179857944

teleop_turtle 노드가 tutle1/command_vel이라는 topic으로 publish하면, rostopic echo 노드가 turtle1/command_vel topic으로부터 subsribe 한다.



- rostopic list

```
 $ rostopic list -h
 $ rostopic list -v
```

<br>

<h4>ROS Messages</h4>
topic에 대해 통신하는 일은, 노드 사이에 ROS 메세지를 보내는 것을 통해 수행됩니다. publisher와 subscriber가 통신을 하기 위해서, publisher와 subscriber는 같은 타입의 메세지를 보내고 받아야 합니다.

즉, topic 타입은, topic에 publish 된 메세지 타입에 의해 정의가 됩니다. topic에 보내진 메세지 타입은 rostopic type을 통해 결정이 될 수 있습니다.



- rostopic type

- rostopic pub

rostopic pub은 topic에 데이터를 publish 한다. 

```
 $ rostopic pub -1 /turtle1/cmd_vel geometry_msgs/Twist -- '[2.0, 0.0, 0.0]' '[0.0,0.0,1.8]'
 
 # rostopic pub [topic] [msg_type] [args]
```

![27-1](https://user-images.githubusercontent.com/37301677/50515906-d396a100-0aea-11e9-9c3c-f4aa608d8c8b.png)

 

```
 $ rostopic pub /turtle1/cmd_vel geometry_msgs/Twist -r 1 -- '[2.0, 0.0, .0.0]' '[0.0, 0.0, -1.8]'
```

![27-2](https://user-images.githubusercontent.com/37301677/50515925-f6c15080-0aea-11e9-98b5-425a3960342e.png)







![27](https://user-images.githubusercontent.com/37301677/50514952-fa51d900-0ae4-11e9-9552-6987ac6327d1.PNG)

새로 생긴 rostopic_3537_1545998424077은 rostopic pub 노드이다. 



![28](https://user-images.githubusercontent.com/37301677/50514956-03db4100-0ae5-11e9-87ec-06762d66530d.PNG)





- rostopic hz
- rqt_plot

![29](https://user-images.githubusercontent.com/37301677/50515936-122c5b80-0aeb-11e9-9389-68a74947a2e1.PNG)

<br>

<br>

-------------

<h2>ROS Services and Parameters 이해하기</h2>
서비스는 노드 간 통신하는 또 다른 방법입니다.  서비스는 노드가 request를 보내고 response를 받게 합니다.



<h5>rosservice 이용하기 (Using rosservice)</h5>
- rosservice list
- rosservice type

<br>

```
 $ rosservice list
 $ rosservice type /clear
 # rosservice type [service]
```

![30](https://user-images.githubusercontent.com/37301677/50515984-57e92400-0aeb-11e9-8c4c-e19303bcd8df.PNG)



![31](https://user-images.githubusercontent.com/37301677/50516039-bf06d880-0aeb-11e9-8bdf-f9447821bcea.PNG)

```
 $ rosservice call /clear
 # rosservicee [service] [args]
```

![32](https://user-images.githubusercontent.com/37301677/50516045-c9c16d80-0aeb-11e9-8a7d-4012cc8875c2.PNG)





```
 $ rosservice type /spawn | rossrv show
 
 $ rosservice cal  /spawn 2 2 0.2 ""
```

![33](https://user-images.githubusercontent.com/37301677/50516081-0725fb00-0aec-11e9-9e1a-bed85e9a81f1.PNG)

<br>



<h5>rosparam 이용하기 (Using rosparam)</h5>
- rosparam list
- rosparam set
- rosparam get
- rosparma dump
- rosparam load



```
 $ rosparam list
```



![35](https://user-images.githubusercontent.com/37301677/50516102-258bf680-0aec-11e9-9929-4e05ff0aa6b2.PNG)



```
 $ rosparam set /background_r 150
 
 # rosparam set [param_name]
 # rosparam get [param_name]
 
 
 $ rosservice call /clear
```



![34](https://user-images.githubusercontent.com/37301677/50516116-3472a900-0aec-11e9-8428-3cd9e5bc8867.PNG)



![36](https://user-images.githubusercontent.com/37301677/50516103-258bf680-0aec-11e9-8362-2962282f9075.PNG)



<br>

<br>

------------------------------

<h2>ROS msg 파일과 srv 파일</h2>
```
 msg : msg 파일은 ROS 메세지 파일의 field를 설명하는 간단한 텍스트 파일이다.
 
 srv : srv 파일은 서비스를 설명한다. request와 response라는 두개의 파트로 구성되어 있다. 
```



<br>

<h4>msg 생성하기</h4>
```
$ roscd beginner_tutorials
$ mkdir msg
$ echo "int64 num" > msg/Num.msg
```

위의 명령어를 입력하고 나면 해당 디렉토리에 Num.msg 파일이 생깁니다.

![37](https://user-images.githubusercontent.com/37301677/50519199-27aa8100-0afd-11e9-9a94-0c29893197c3.PNG)



Num.msg 파일을 확인해보면 위에서 입력했던 int64 num이 적혀져있는 것을 볼 수 있습니다.

![38](https://user-images.githubusercontent.com/37301677/50519200-27aa8100-0afd-11e9-8273-bdb42612af54.PNG)



```
string first_name
string last_name
uint8 age
uint32 score
```

를 Num.msg에 추가로 입력해줍니다.



![39](https://user-images.githubusercontent.com/37301677/50519201-28431780-0afd-11e9-9e7d-65cf968c3857.PNG)

현재 작업하고 있는 beginner_tutorials 패키지의 xml 파일을 편집하여  아래의 텍스트를 추가해줍니다.

```
<build_depend>message_generation</build_depend>
<exec_depend>message_runtime</exec_depend>
```

![46](https://user-images.githubusercontent.com/37301677/50519208-28dbae00-0afd-11e9-9b91-76e88e74be47.PNG)

![47](https://user-images.githubusercontent.com/37301677/50519209-29744480-0afd-11e9-8ef7-affc7436d325.PNG)



마찬가지로 beginner_tutorials 패키지의 CMakeLists.txt 파일을 편집하여 주석을 지우거나 텍스트를 추가해줍니다. 

CMakeLists.txt 는 디폴드로 작성이 되어있기 때문에, 새로 작성하기 보다는 기존에 있는 파일을 수정하여줍니다.  아래에 해당하는 부분만 수정하여줍니다.

```
find_package(catkin REQUIRED COMPONENS
  roscpp
  rospy
  std_msgs
  message_generation
)
...
catkin_package(
  ...
  CATKIN_DEPENDS message_runtime...
  ...
)
...
add_message_files(
  FILES
  Num.msg
)
...
generate_messages(
  DEPENDENCIES
  std_msgs
)

```



![41](https://user-images.githubusercontent.com/37301677/50519203-28431780-0afd-11e9-8cdf-741c41274587.PNG)

![42](https://user-images.githubusercontent.com/37301677/50519204-28431780-0afd-11e9-9f45-92dad31356c0.PNG)





![43](https://user-images.githubusercontent.com/37301677/50519205-28431780-0afd-11e9-9ec4-8d44706f46c8.PNG)



![44](https://user-images.githubusercontent.com/37301677/50519206-28dbae00-0afd-11e9-82e7-ffaa85087b7a.PNG)



```
$ rosmsg show beginner_tutorials/Num
$ rosmsg show Num

# rosmsg show [message type]
```

Num.msg파일의 메세지 타입을 볼 수 있습니다. 

![45](https://user-images.githubusercontent.com/37301677/50519207-28dbae00-0afd-11e9-8e55-bb447aed5e27.PNG)



<br>

<h4>srv 생성하기</h4>
```
$ roscd beginner_tutorials
$ mkdir srv

$ roscp rospy_tutorials AddTwoInts.srv srv/AddTwoInts.srv

# roscp [package_name] [file_to_copy_path] [copy_path]
```



msg 파일을 생성할 때 beginner_tutorials 패키지의 xml 파일을 수정해주었으므로, 다시 확인만 해줍니다. 

![46](https://user-images.githubusercontent.com/37301677/50519305-8ff96280-0afd-11e9-90fb-733c407a1631.PNG)



![47](https://user-images.githubusercontent.com/37301677/50519306-9091f900-0afd-11e9-83b7-ad54d0c6441c.PNG)



beginner_tutorials 파일의 CMakeLists.txt 파일을 수정하여, msg 파일을 생성할 때 수정하였던 부분은 그대로 두고, srv 파일을 생성하기 위해 나머지 부분을 수정하여줍니다. 

```
이 부분도 확인만해주고
find_package(catkin REQUIRED COMPONENS
  roscpp
  rospy
  std_msgs
  message_generation
)

아래의 부분을 찾아서 수정하여 줍니다. 
...
add_service_files(
  FILES
  AddTwoInts.srv
)
...
```

![48](https://user-images.githubusercontent.com/37301677/50519309-9091f900-0afd-11e9-9eea-b4ed6ef6a74e.PNG)



![49](https://user-images.githubusercontent.com/37301677/50519310-912a8f80-0afd-11e9-8c16-dce1a7bf226a.PNG)





```
$ rossrv show beginner_tutorials/AddTwoInts

# rosmsg show <service type>
```



srv 파일의 타입을 확인하였습니다!

![50](https://user-images.githubusercontent.com/37301677/50519312-912a8f80-0afd-11e9-9783-6578ac48932e.PNG)



beginner_tutorials 패키지의 파일들을 수정하였으니, 빌드를 해봅내다.



```
$ roscd beginner_tutorials
$ cd ../..
$ catkin_make install
$ cd -
```



![51](https://user-images.githubusercontent.com/37301677/50520281-cdacba00-0b02-11e9-8204-edf78f8ebcc5.PNG)





![53](https://user-images.githubusercontent.com/37301677/50520283-cdacba00-0b02-11e9-849a-60debd62a157.PNG)







```
아래의 경로에 C++ 헤더파일이 생성된 것을 볼 수 있습니다. 
~/catkin_ws/devel/include/beginner_tutorials/

파이썬 스크립트 파일은 아래의 경로에서 확인할 수 있습니다. 
~/catkin_ws/devel/lib/python2.7/dist-packages/beginner_tutorials/msg
```



![52](https://user-images.githubusercontent.com/37301677/50520282-cdacba00-0b02-11e9-8ad3-366e991c47b0.PNG)



![54](https://user-images.githubusercontent.com/37301677/50520323-05b3fd00-0b03-11e9-8bf1-434d00db0b00.PNG)



<br>

<br>

-------------------------------



<h2>Publisher와 Subscriber 작성/확인하기</h2>
<h4>Publisher 작성하기</h4>
```
$ roscd beginner_tutorials

$ mkdir scripts
$ cd scripts

$ wget https://raw.github.com/ros/ros_tutorials/kinetic-devel/rospy_tutorials/001-talker_listner/talker.py
$ chmot +x talker.py  # make file executable
```

<br>

<h5>talker.py</h5>
```
#!/usr/bin/env python
 

## Simple talker demo that published std_msgs/Strings messages
## to the 'chatter' topic

 
import rospy
from std_msgs.msg import String


def talker():
    pub = rospy.Publisher('chatter', String, queue_size=10)
    rospy.init_node('talker', anonymous=True)
    rate = rospy.Rate(10) # 10hz
    while not rospy.is_shutdown():
        hello_str = "hello world %s" % rospy.get_time()
        rospy.loginfo(hello_str)
        pub.publish(hello_str)
        rate.sleep()

 
if __name__ == '__main__':
    try:
        talker()
    except rospy.ROSInterruptException:
        pass
```



<br>

<h4>Subscriber 작성하기</h4>
```
$ roscd beginner_tutorials/scripts

$ wget https://raw.github.com/ros/ros_tutorials/kinetic-devel/rospy_tutorials/001-talker_listner/listenter.py
$ chmot +x listener.py  # make file executable
```

<br>

<h5>listener.py</h5>
```
#!/usr/bin/env python
 

## Simple talker demo that listens to std_msgs/Strings published 
## to the 'chatter' topic

 
import rospy
from std_msgs.msg import String

 
def callback(data):
    rospy.loginfo(rospy.get_caller_id() + 'I heard %s', data.data)

 
def listener():

    # In ROS, nodes are uniquely named. If two nodes with the same
    # name are launched, the previous one is kicked off. The
    # anonymous=True flag means that rospy will choose a unique
    # name for our 'listener' node so that multiple listeners can
    # run simultaneously.

    rospy.init_node('listener', anonymous=True)


    rospy.Subscriber('chatter', String, callback)
 
    # spin() simply keeps python from exiting until this node is stopped

    rospy.spin()

 
if __name__ == '__main__':
    listener()
```





빌드합시다!

```
$ cd ~/catkin_ws
$ catkin_make
```

<br>

<h4>Publisher 실행하기</h4>
```
$ roscore
```

roscore가 실행되어있다면

rosrun을 통해 실행하여 봅시다!

```
$ rosrun beginner_tutorials talker.py
```



![56](https://user-images.githubusercontent.com/37301677/50521263-33e80b80-0b08-11e9-8102-9e24796b6606.PNG)



```
$ rosrun beginner_tutorials listener.py
```



![57](https://user-images.githubusercontent.com/37301677/50521279-45311800-0b08-11e9-90df-a88a78ae6534.PNG)





<br>

<br>

----------------------------

<h2>Service 와 Client 작성하기</h2>
<h4>Service 작성하기</h4>
```
$ roscd beginner_tutorials

$ mkdir scripts
$ cd scripts

$ wget /////
$ chmot +x add_two_ints_server.py  # make file executable
```



<br>

<h5>add_two_ints_server.py</h5>
```
#!/usr/bin/env python

 
from beginner_tutorials.srv import *
import rospy

 
def handle_add_two_ints(req):
  print("Returning [%s + %s = %s]"%(req.a, req.b, (req.a + req.b)))
  return AddTwoIntsResponse(req.a + req.b)

 
def add_two_ints_server():
  rospy.init_node('add_two_ints_server')
  s = rospy.Service('add_two_ints', AddTwoInts, handle_add_two_ints)
  print("Ready to add two ints.")
  rospy.spin()
 

if __name__ == "__main__":
  add_two_ints_server()
```



<br>

<h4>Client 작성하기</h4>
```
$ roscd beginner_tutorials

$ mkdir scripts
$ cd scripts

$ wget /////
$ chmot +x add_two_ints_client.py  # make file executable

```



<br>

<h5>add_two_ints_client.py</h5>
```
#!/usr/bin/env python

 
import sys
import rospy
from beginner_tutorials.srv import *

 
def add_two_ints_client(x, y):
  rospy.wait_for_service('add_two_ints')
  try:
    add_two_ints = rospy.ServiceProxy('add_two_ints', AddTwoInts)
    resp1 = add_two_ints(x, y)
    return resp1.sum
  except rospy.ServiceException, e:
    print("Service call failed: %s"%e)
 

def usage():
  return "%s [x y]"%sys.argv[0]


if __name__ == "__main__":
  if len(sys.argv) == 3:
    x = int(sys.argv[1])
    y = int(sys.argv[2])
  else:
    print(usage())
    sys.exit(1)
  print("Requesting %s+%s"%(x, y))
  print("%s + %s = %s"%(x, y, add_two_ints_client(x,y)))
```

<br>

				HTML

​					
​				
​				
​						
​				

<h4>Server와 Client 확인하기</h4>

```
$ rosrun beginner_tutorials add_two_int_server.py
```

![58](https://user-images.githubusercontent.com/37301677/50522211-23865f80-0b0d-11e9-9973-c619c2db6758.PNG)





```
$ rosrun beginner_tutorials add_two_int_client.py
```

덧셈을 하고 프로세스가 종료됩니다. 

![59](https://user-images.githubusercontent.com/37301677/50522212-23865f80-0b0d-11e9-9e28-e124aad62de3.PNG)



server에서는 cilent을 실행한 후 

returning이 된 것을 볼 수 있습니다. 

![60](https://user-images.githubusercontent.com/37301677/50522215-241ef600-0b0d-11e9-9177-e0eb53a3cfc6.PNG)



<br>
