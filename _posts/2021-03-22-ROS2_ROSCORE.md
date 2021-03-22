---
layout: post
title: ROS2에서 ROSCORE 안쓰는 이유
category: ROS
tag: ROS
---

ROS1에서는 GAZEBO 혹은 Real Environment에 모바일로봇을 컨트롤 하려면 roscore을 통해 연결을 시켜줘야 했다(rosmaster). 그 이후 다른 터미널을 통해서 컨트롤을 해야 됐다.

즉 Roscore는 마스터 노드이며, 모든 노드는 이 마스터 노드에 종속이 되어 있다. 마스터 노드를 반드시 켜야만 다른 노드들을 이용할 수 있었다.

그러나 ROS2에서는 Roscore(Client/servr(or Slave/master) 형식) 아키텍처가 사라졌고. DDS(Data Distribbution service) 아키텍처가 적용이 되었다. (publish/subscribe 형식)

즉, peer to peer로 실시간과 다양한 데이터 타입을 효과적으로 교환할 수 있다. (ROS1에서는 roscore라는 중간다리가 있기 때문에 불필요한 코스트가 컷고, 노드간 frequency 문제가 있었다.)

또한 모든 노드가 마스터 노드에 종속이 되어 있기 떄문에, 어떤 노드가 문제가 생기게 될경우 모든 시스템에 실패를 불러오게 된다. 그렇지만 로스2에서는 어떤 노드가 문제가 생겼을 경우에도 시스템 실패가 아닌 버그로 간주하여 더 intuitive하게 문제를 해결할 수 있다.

예) 이전 roscore가 없다면

rostopic list 를 하였을 경우 리스트 결과가 안나왔을 것이다.

그러나 ROS2에서는 roscore에 존속되어있지 않기 때문에, 노드 런칭을 자유롭게 할 수있고 토픽이나 echo package 명령어를 통해 런칭된 노드에 있는 변수들을 확일 할 수 있다.


<a href="https://postimg.cc/VSnpvdjh"><img src="https://i.postimg.cc/CKQgv8dw/Screen-Shot-2021-03-22-at-3-40-09-PM.png" width="700px" title="source: imgur.com" /><a>
