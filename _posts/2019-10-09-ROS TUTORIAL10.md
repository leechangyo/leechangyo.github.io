---
layout: post
title: 10. TF2 ROS
category: ROS
tag: Tutorial
---
# TF2 ROS Introduction
- command line tool
  - tf_echo(static)

> example

```
tf tf_echo world robot2_pedestal_link
```

- tf2_ros package
  - implement functional aspects, and actively maintain relations
  - Transform Pose: allows for "time travel" - look up the spatiotemporal時空 relation
- was there a TF(1) package?
  - yes
  - used in many applications
  - command line tools
  - All in one (vs clear separation in TF2)
- All TF functionalities are migrated to tf2_ros
  - availability of a transform buffer in tf2_ros
- Information representation
  - Quantification
    - 3D Transfromation
    - ROS topic: /tf (tf2_msgs/TFMessage)
- 
