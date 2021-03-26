---
layout: post
title: How to create a ROS2 Package for python
category: ROS2
tag: ROS2
---

COMMANDS TO USE:
- source /opt/ros/crystal/setup.bash
- ros2 pkg create ros2_msg --build-type ament_cmake --dependencies rclcpp std_msgs
- create msg folder
- create msg file as Mymsg.msg
- then write messages

```
int32 day
string month
int year
```

- CMakeLists에다 find_package(rosidl_default_generators REQQUIRED), find_package(builtin_interface REQUIRED) 추가
- 그리고 rosidl_generate_interface(new_msg "msg/Mymsg.msg" DEPENDENCIES builtin_interface) 추가
- 마지막으로 package.xml 파일도 추가
  - <build_depend>builtin_interface</build_depend>
  - <build_depend>rosidl_default_generators</build_depend>
  - <exec_depend>builtin_interface</exec_depend>
  - <exec_depend>rosidl_default_runtime</exec_depend>
  - <member_of_group>rosidl_interface_package</member_of_group>

- 그리고 concon build --symlink-install
- source install/setup.bash

Test
- ros2 topic pub  /test_topic ros2_msg/MyMsg "{day : '21', month : 'August', year : '2021' }"


로스2 에서는 msg, serve, action등의 메세지들 중복에 대한 충볼을 프리하게 하였다.


참고 자료:
https://www.youtube.com/watch?v=nG3IOkEOPps&list=PLK0b4e05LnzYNBzqXNm9vFD9YXWp6honJ&index=3
