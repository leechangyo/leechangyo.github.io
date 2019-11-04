---
layout: post
title: Controller problem
category: ROS
tag: ROS
---
# Problem
Kin chain provided in model doesn't contain standard UR joint 'shoulder_lift_joint'

# solution

change config in kinematic.yaml from moveit_config

```
robot1:
  kinematics_solver: trac_ik_kinematics_plugin/TRAC_IKKinematicsPlugin
  kinematics_solver_search_resolution: 0.005
  kinematics_solver_timeout: 0.005
  kinematics_solver_attempts: 3
  solve_type : Distance
robot2:
  kinematics_solver: trac_ik_kinematics_plugin/TRAC_IKKinematicsPlugin
  kinematics_solver_search_resolution: 0.005
  kinematics_solver_timeout: 0.005
  kinematics_solver_attempts: 3
  solve_type : Distance
```
