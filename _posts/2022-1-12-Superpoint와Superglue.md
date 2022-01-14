---
layout: post
title: CMAKE에서 주의해야 될 점
category: ROS
tag: ROS
---

보통 페키지를 관리하고 모듈화 할때 CMAKE를 많이 쓴다.

보통 잘 만들어진 씨메이크를 가져와서 수정하는데 이에 주의해야 될 부분이 있다.


```
# Include libraries
# boost dependency
find_package(Eigen3 REQUIRED)
find_package(Boost REQUIRED COMPONENTS system filesystem thread date_time )
```

위에 내용을 보면 Boost에 대한 Package를 찾고 그에 관련된 libraries들을 가져오게 된다.

```
# catkin dependcy
# Find catkin (the ROS build system)
find_package(catkin REQUIRED COMPONENTS roscpp nav_msgs std_msgs sensor_msgs nav_msgs tf rosbag)
```

마찬가지로 catkin 로스 패키지를 가져오고 그에 관련된 library를 가져오게 된다.

다만 만약 Boost Package가 가지고 있는 라이브러리와 Catkin이 가지고 있는 라이브러리를 헷갈려서 잘못 정의하게 된다면, compile이 잘 안되니, 어디에 디펜던시가 되어있는지 잘 알아보고 해야된다.
