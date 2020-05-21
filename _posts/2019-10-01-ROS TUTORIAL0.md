---
layout: post
title: 0. ROS Basic
category: ROS
tag: ROS
---
# Fundamental Concepts
- ROS file system -nomenclature
  - ROS workspace -catkin workspace
    - src space, devel space, build space, install space
  - ROS packages
    - organize different functional modules
- ROS workspace is a folder to organize ROS project files
- catkin - a build tool that compiles source files to binaries
- catkin_workspace - ROS workspace where catkin is used as the build tool

<a href="https://postimg.cc/MX9q5xjJ"><img src="https://i.postimg.cc/NfcFgjsF/Capture.png" width="500px" title="source: imgur.com" /><a>

- install dependencies of a ROS package(listed in package.xml)

```
cd <path_to_folder_with_ROS_packages(s)>
rosdep install <package_name>
```

- install dependencies of all packages in our source space

```
cd <path_to_ros_workspace/src>
rosdep install --from-paths . --ignore-src -y
```

- we dont need the source files of all dependencies of our ROS package

- Devel space
  - Binary executable files for tesing our development
  - files for setting up project specific ROS environment
  - source ROS environment for project in catkin workspace

```
source <path_to_workspace/devel/setup.bash
```

# ROS launch
- Group Multiple Ros nodes in one file
  - launch file in xml format
- start up all nodes in the launch file
  - arguments, node specific paramters, namespaces
- also possible to include launch files from other packages in the same launch file

> example

```xml
<launch>
  <node name="counter_with_delay" pkg="hrwros" type="counter_with_delay_as.py" output="screen">
    <param name="counter_delay" type="double" vale="$(arg counter_delay_parameter)"/>

```

# ROS Actions
- No waiting until an execution is complete(non-blocking)
  - waiting is an option if required
- A generalized request-response system - **the client-sever** infrastructure in ROS
- Actions are defined by the three message types: Goal(request), Result(response) and Feedback
- Requirement: counter with a 1s delay between each count
  - Goal Message - Number of count up to (uint32)
  - Result message - state message(string)
  - feedback message - number of counts completed(unit32)

> CounterWithDelay.action

```msg
unit32 num_counts <- Goal Field
---
string result_message <- result field
---
unit32 counts_elapsed <- feedback field
```

- Ros action definitions reside in the ROS package with project specific message definitions
  - <package name>/action folder
- generate action messages manually
  - rosrun actionlib_msgs genaction.py <path_to_action_file>
- show the contents of an action message
  - rosmsg show <stack_name> _ msgs/<ActionMessage>

## Processing a goal request
- **goalCallbcak function** processes a goal request
  - a goal can be pre-empted(and cancelled) before completion
- goal statuses: ACTIVE, SUCCEEDED, ABORTED(실패)

```python
client = actionlb.SimpleActionClient(counter_with_delay,CounterWithDelayAction)

client.wait_for_server()

goal = CounterWithDelayGoal(num_counts=10)
client.send_goal(goal)
```

# ROS Service
- Event-Based execution for ROS applications

```srv
float64 measurement_metres <- request field
---
float64 measurement_feet <- response field
Bool success
```
