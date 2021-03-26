---
layout: post
title: How to create a ROS2 Package for C++
category: ROS2
tag: ROS2
---

COMMANDS TO USE:
- ros2 pkg create ros2_cpp_pkg --build-type ament_cmake --dependencies rclcpp
- colcon build --symlink-install

ROS1과 비슷하게 create 된 C++ 코드는 CMakeLists에 executes 스크립트를 써서 아래와 같이 execute할 수 있다

- rosrun run ros2_cpp_pkg cpp_code


### ament_cmake
ament_cmake is the build system for CMake based packages in ROS 2 (in particular, it will be used for most if not all C/C++ projects). It is a set of scripts enhancing CMake and adding convenience functionality for package authors.

the main role of ament_cmake is to provide CMake macros that will make your job easier when developing multiple packages that depend on each other. None of it is required to build a CMake based ROS package.

An example of such macro is ament_target_dependencies, it condenses the following cmake logic into a single call:

- target_compile_definitions()
- target_include_directories()
- target_link_libraries()
- setting link flags on the target

Another (the most?) important macro it provides is ament_package. This is what will handle the registration of your package in the ament index, generate the CMake config files for the project to be find_package'able, install the extra CMake config files you provide, install the package.xml of your package etc

You can find some information about ament_cmakein the section "Build type: ament_cmake" of https://design.ros2.org/articles/amen...
