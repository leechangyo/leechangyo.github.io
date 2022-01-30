---
layout: post
title: Debug 쉽게하는 법[C++, ROS]
category: ROS
tag: ROS
---

### C++ DEBUG

1. firstly, created .o file for source code.(Cmake or using some compiler)

2. GO TO RUN & BUG
- create json
  - choose type of debug type(in this case, C++, and RECOMMAND LAUNCH)

3. change execute path

4. run launch.json.


### ROS DEBUG

1. debug mode compile
  - catkin_make -DCMAKE_BUILD_TYPE=Debug

2. GO TO RUN & BUG
- create json
  - choose type of debug type(in this case : ROS launch(roslaunch), ROS attach(for rosrun))

```
{
    "name": "ROS: Launch",
    "type": "ros",
    "request": "launch",
    "target": "absolute path to launch file"
},
{
    "name": "ROS: Attach",
    "type": "ros",
    "request": "attach"
},
```

3. change path.

4. run roscore

5. run debug
  - if attach mode, run rosrun file and run debug.


reference
https://github.com/ms-iot/vscode-ros/blob/master/doc/debug-support.md
