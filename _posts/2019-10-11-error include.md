---
layout: post
title: Include error
category: ROS
tag: debug
---

# 进入代码目录并编译

记得source一下

cd simulation_structure/

source /opt/ros/kinetic/setup.bash

catkin_make


有错误

详情如下：

CMake Error at /opt/ros/kinetic/share/catkin/cmake/catkin_package.cmake:302 (message):
  catkin_package() include dir 'include' does not exist relative to
  '/home/dyz/simulation_structure/src/sensor_cmd_communication'
Call Stack (most recent call first):
  /opt/ros/kinetic/share/catkin/cmake/catkin_package.cmake:102 (_catkin_package)
  sensor_cmd_communication/CMakeLists.txt:108 (catkin_package)

cd src/

 

-- Configuring incomplete, errors occurred!
See also "/home/dyz/simulation_structure/build/CMakeFiles/CMakeOutput.log".
See also "/home/dyz/simulation_structure/build/CMakeFiles/CMakeError.log".
Invoking "cmake" failed


解决办法，删除sensor_cmd_communication文件夹，进入src并重新creat：

rm -rf src/sensor_cmd_communication/

cd src/

catkin_create_pkg sensor_cmd_communication rospy roscpp


详情如下：

dyz@dyz-X550CC:~/simulation_structurerm−rfsensorcmdcommunication/dyz@dyz−X550CC: /simulationstructurerm−rfsensorcmdcommunication/dyz@dyz−X550CC: /simulationstructure cd src/
dyz@dyz-X550CC:~/simulation_structure/src$ catkin_create_pkg sensor_cmd_communication rospy roscpp
Created file sensor_cmd_communication/CMakeLists.txt
Created file sensor_cmd_communication/package.xml
Created folder sensor_cmd_communication/include/sensor_cmd_communication
Created folder sensor_cmd_communication/src
Successfully created files in /home/dyz/simulation_structure/src/sensor_cmd_communication. Please adjust the values in package.xml.

退出src并编译

cd ..

catkin_make


详情如下：

dyz@dyz-X550CC:~/simulation_structure/srccd..dyz@dyz−X550CC: /simulationstructurecd..dyz@dyz−X550CC: /simulationstructure catkin_make
Base path: /home/dyz/simulation_structure
Source space: /home/dyz/simulation_structure/src
Build space: /home/dyz/simulation_structure/build
Devel space: /home/dyz/simulation_structure/devel
Install space: /home/dyz/simulation_structure/install

...

编译成功。
————————————————
版权声明：本文为CSDN博主「ad33332222」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/ad33332222/article/details/91421436
