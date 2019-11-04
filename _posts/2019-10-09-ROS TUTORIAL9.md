---
layout: post
title: 9. ROS Manipulate
category: ROS
tag: Tutorial
---
# ROS Manipulation Overview
- Moveit! - a ROS package for manipulation
- Moveit! setup assistant - configure robot setups to use moveit
- plan and execute motion - the "Move_group" Ros Node
- Implement a simple pick and place pipeline
- interact with the environment and modify it using a robot to perform useful tasks
  - pick and place an object
  - fasten two parts of a car chassis
- An integral part of automation

<a href="https://postimg.cc/4nZNzK45"><img src="https://i.postimg.cc/kMR6P8kk/Capture.png" width="700px" title="source: imgur.com" /><a>

- Moveit : open source software library for manipulation
  - easy integration with ros
  - maintain information consistency
  - integrate robot kinematic information with planning
  - report and request alternative motion plans in case of collisions
  - Account for any hardware limitations such as joint limits
  - keep track of the current state of the robot and its environment while performing a manipulation task
  - talk to the robot hardware/simulation and notify the ros application once a desired manipulation task is complete
- Moveit! - from a user's perspective
  - move_group Ros node
    - several ros Service and actions (APIs)
    - Configuring the move_group node
      - robot description (URDF/XACRO)
      - robot_semantic description(SRDF)
      - joint limit, planner
- where are trajectories executed
  - simulated robot or real hardware
- what controls trajectory execution in simulation or hardware
  - controller on a simulator or a hardware driver
  - simulation package hrwros_gazebo
- how does moveit! talk to the controllers from simulation?
  - ros topics and actions servers

## Basic commands
> command

```
#!/bin/bash
rosrun moveit_commander moveit_commander_cmdline.py /joint_states:=/combined_joint_states joint_states:=/combined_joint_states
```

- list all usable command
  - help
- select the "group" to use
  - use <group_name>
- plan and execute motion form srdf
  - go <up I down I left I right I forward I backward> <distance_in_m>
- get current joint state and pose
  - current
- Execute multiple commands
  - load <path_to_script_file/scrip_file_name>

## MoveGroup Interface
- A collection of APIs to access the various capabilities of Move_group Ros node
- Create Moveit!-based ROS applications
- Setup a simple pick and plcae pipline in code with movegroup APIs
  - using ROS action client

<a href="https://postimg.cc/NyrYF1wk"><img src="https://i.postimg.cc/hG2GCry6/Capture.png" width="700px" title="source: imgur.com" /><a>
