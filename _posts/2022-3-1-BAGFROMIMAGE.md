---
layout: post
title: VIM useful Command
category: Programming
tag: Programming
---

因为需要自己制作图像集。
首先，我们利用的是现成的包
[ROS_PACKAGE]https://github.com/raulmur/BagFromImage
他的安装流程是老版本的rosbuild，但我们使用catkin_make
安装编译按我的流程走

1. 在自己的 /catkin_ws/src/下

```
git clone https://github.com/raulmur/BagFromImages.git
```

2. 修改cmakelist.txt 使用opencv 2还是 3我不确定，我是3所以此处写3

```yaml
cmake_minimum_required(VERSION 2.4.6)
#include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

project(BagFromImages)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -Wall  -o3 -march=native ")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall  -o3 -march=native")
set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_FLAGS "-std=c++11")
set(CMAKE_CXX_FLAGS_RELEASE "-o3 -Wall -g")

#rosbuild_init()

FIND_PACKAGE(OpenCV 3 REQUIRED)


find_package(catkin REQUIRED COMPONENTS
	roscpp
	std_msgs
	tf
	image_transport
	cv_bridge
	rosbag
)


set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)

catkin_package(CATKIN_DEPENDS roscpp std_msgs tf image_transport cv_bridge)
include_directories(
	${catkin_INCLUDE_DIRS}
	${rosbag_INCLUDE_DIR}
)

#rosbuild_add_executable(${PROJECT_NAME}
#main.cc
#Thirdparty/DLib/FileFunctions.cpp)
add_executable( ${PROJECT_NAME}
main.cc
Thirdparty/DLib/FileFunctions.cpp)

target_link_libraries (${PROJECT_NAME}
${catkin_LIBRARIES}
${OpenCV_LIBS}
${rosbag_LIBRARIES})
```

3. 在终端输入gedit package.xml也就是新建一个package.xml。在里面写入

```yaml
<?xml version="1.0"?>
<package>
  <name>BagFromImages</name>
  <version>0.0.0</version>
  <description>The BagFromImages package</description>

  <!-- One maintainer tag required, multiple allowed, one person per tag -->
  <!-- Example:  -->
  <!-- <maintainer email="jane.doe@example.com">Jane Doe</maintainer> -->
  <maintainer email="whu@gmail.com">whu</maintainer>


  <!-- One license tag required, multiple allowed, one license per tag -->
  <!-- Commonly used license strings: -->
  <!--   BSD, MIT, Boost Software License, GPLv2, GPLv3, LGPLv2.1, LGPLv3 -->
  <license>TODO</license>

  <!-- The *_depend tags are used to specify dependencies -->
  <!-- Dependencies can be catkin packages or system dependencies -->
  <!-- Examples: -->
  <!-- Use build_depend for packages you need at compile time: -->
  <!--   <build_depend>message_generation</build_depend> -->
  <!-- Use buildtool_depend for build tool packages: -->
  <!--   <buildtool_depend>catkin</buildtool_depend> -->
  <!-- Use run_depend for packages you need at runtime: -->
  <!--   <run_depend>message_runtime</run_depend> -->
  <!-- Use test_depend for packages you need only for testing: -->
  <!--   <test_depend>gtest</test_depend> -->
  <buildtool_depend>catkin</buildtool_depend>
  <build_depend>roscpp</build_depend>
  <build_depend>tf</build_depend>
  <build_depend>std_msgs</build_depend>
  <build_depend>sensor_msgs</build_depend>
  <build_depend>image_transport</build_depend>
  <build_depend>cv_bridge</build_depend>
  <build_depend>rosbag</build_depend>


  <run_depend>roscpp</run_depend>
  <run_depend>tf</run_depend>
  <run_depend>std_msgs</run_depend>
  <run_depend>sensor_msgs</run_depend>
  <run_depend>image_transport</run_depend>
  <run_depend>cv_bridge</run_depend>
  <run_depend>rosbag</run_depend>


  <!-- The export tag contains other, unspecified, tags -->
  <export>
    <!-- You can specify that this package is a metapackage here: -->
    <!-- <metapackage/> -->

    <!-- Other tools can request additional information be placed here -->

  </export>
</package>
```

4. 在/catkin_ws目录下去使用catkin_make去编译

5. rosrun bagfromimages BagFromImage <path> <format> <hz> <outputname>
