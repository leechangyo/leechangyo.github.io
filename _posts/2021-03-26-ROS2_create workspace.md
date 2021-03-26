---
layout: post
title: How to create a ROS2 Workspace
category: ROS2
tag: ROS2
---

COMMANDS TO USE:
- mkdir -p my_ros2_ws/src
- colcon build
- source ~/my_ros2_ws/install/local_setup.bash

in, ros1 kind of
- catkin_make
- source ~devel/setup.bash.


## Cocon build
colcon 는 OSRF(Open Source Robotics Foundation )에서 빌드한 명령줄 도구입니다. 이 서비스는 ROS 및 ROS2 애플리케이션의 빌드 및 번들링을 자동화하고, 을 즉시 대체할 수 있습니다.

colcon는 중첩된 폴더 구조를 지원하고 패키지를 자동으로 찾습니다.

예를 들어 intermediate_directory의 CMakeLists.txt는 필요하지 않습니다.

```
src/
├── package_1/
│ ├── package.xml
| └── CMakeLists.txt # okay
└── intermediate_directory/
    ├── package_2
    | ├── package.xml
    | └── CMakeLists.txt # okay
    ├── package_3
    | ├── package.xml
    | └── CMakeLists.txt  # okay
    └── CMakeLists.txt  # !!! remove !!!
```

colcon is a command line tool to improve the workflow of building, testing and using multiple software packages. It automates the process, handles the ordering and sets up the environment to use the packages.

## Background
colcon is an iteration on the ROS build tools catkin_make, catkin_make_isolated, catkin_tools and ament_tools.

## Basics

A ROS workspace is a directory with a particular structure. Commonly there is a src subdirectory. Inside that subdirectory is where the source code of ROS packages will be located. Typically the directory starts otherwise empty.

colcon does out of source builds. By default it will create the following directories as peers of the src directory:

- The build directory will be where intermediate files are stored. For each package a subfolder will be created in which e.g. CMake is being invoked.

- The install directory is where each package will be installed to. By default each package will be installed into a separate subdirectory.

- The log directory contains various logging information about each colcon invocation.
