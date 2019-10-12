---
layout: post
title: Move_base_msgsConfig error
category: ROS
tag: Debug
---

# problem statement
```
CMake Warning at /opt/ros/kinetic/share/catkin/cmake/catkinConfig.cmake:76 (find_package):
  Could not find a package configuration file provided by "move_base_msgs"
  with any of the following names:

    move_base_msgsConfig.cmake
    move_base_msgs-config.cmake

  Add the installation prefix of "move_base_msgs" to CMAKE_PREFIX_PATH or set
  "move_base_msgs_DIR" to a directory containing one of the above files.  If
  "move_base_msgs" provides a separate development package or SDK, be sure it
  has been installed.
Call Stack (most recent call first):
  neo_driver/neo_watchdogs/CMakeLists.txt:7 (find_package)


-- Could not find the required component 'move_base_msgs'. The following CMake error indicates that you either need to install the package with the same name or change your environment so that it can be found.
CMake Error at /opt/ros/kinetic/share/catkin/cmake/catkinConfig.cmake:83 (find_package):
  Could not find a package configuration file provided by "move_base_msgs"
  with any of the following names:

    move_base_msgsConfig.cmake
    move_base_msgs-config.cmake

  Add the installation prefix of "move_base_msgs" to CMAKE_PREFIX_PATH or set
  "move_base_msgs_DIR" to a directory containing one of the above files.  If
  "move_base_msgs" provides a separate development package or SDK, be sure it
  has been installed.
Call Stack (most recent call first):
  neo_driver/neo_watchdogs/CMakeLists.txt:7 (find_package)
```

# Solution
- it is not installed move_base msg, therefore need to install 
```
sudo apt-get ros-kinetic-navigation
```
