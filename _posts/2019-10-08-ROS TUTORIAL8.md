---
layout: post
title: 8. ROS Navigation stack
category: ROS
tag: ROS
---
# ROS Navigation stack
- 2D Navigation
- inputs:
  - Odometry
  - Sensor data
  - Goal Pose
- Output:
  - safe velocity commands
- only for differential drive or holonomic wheeled robots
- requires a planar laser
- performs best on square or circular robots
- ROS topics : Transport information between nodes

## Move_base
- provides an implementation of an action
  - Actions are used for long term tasks
- Uses a global and local planner to accomplish its global navigation goal
- Manages communication between the components of the navigation stack

## Rviz
1. Under Global options on the left side panel for fixed frame, change base_link or base_footprint to Odom
2. click on Add and select the by topic tab
3. choose odometry and click on OK

## Navigation necessary
1. Map of the environment
  - use a pre-existing map
  - built by the robot itself during a mapping process
    - this process is called SLAM
      - "gmapping"
    - simultaneous Localization and mapping
    - it gathers information about the environment using laser scanning
2. Localization
  - Global methods
    - GPS accuracy ~3.5m
    - Wi-Fi hotspot accuracy ~40m
    - cell towers accuracy ~600m
  - Local methods
    - laser can accuracy ~5cm
    - cameras accuracy ~10cm
3. Path planning
4. Obstacle avoidance

# Exercise

> Create unknown_obstacle.py

```python
#!/usr/bin/env python
import rospy
import os
# Import the spawn urdf model service from Gazebo.
from gazebo_msgs.srv import SpawnModel, SpawnModelRequest, GetModelProperties, GetModelPropertiesRequest
from geometry_msgs.msg import Pose

def spawn_unknown_obstacle(obstacle_name, obstacle_model_xml, obstacle_pose):
  # Service proxy to spawn urdf object in Gazebo.
  spawn_obstacle_service = rospy.ServiceProxy('/gazebo/spawn_urdf_model', SpawnModel)
  # Create service request.
  spawn_obstacle_request = SpawnModelRequest()
  spawn_obstacle_request.model_name = obstacle_name
  spawn_obstacle_request.model_xml = obstacle_model_xml
  spawn_obstacle_request.robot_namespace = ''
  spawn_obstacle_request.initial_pose = obstacle_pose
  spawn_obstacle_request.reference_frame = 'map'
  # Call spawn model service.
  spawn_obstacle_response = spawn_obstacle_service(spawn_obstacle_request)
  if(not spawn_obstacle_response.success):
    rospy.logerr('Could not spawn unknown obstacle')
    rospy.logerr(spawn_obstacle_response.status_message)
  else:
    rospy.loginfo(spawn_obstacle_response.status_message)

def check_obstacle_existence(obstacle_name):
  # Service proxy to spawn urdf object in Gazebo.
  check_obstacle_service = rospy.ServiceProxy('/gazebo/get_model_properties', GetModelProperties)

  check_obstacle_response = check_obstacle_service(obstacle_name)
  return check_obstacle_response.success


if __name__ == '__main__':
  try:
    # Initialize a ROS Node
    rospy.init_node('spawn_unknown_obstacle')
    #Get file name of the object to be spawned.
    with open(os.path.join(os.environ["HOME"], "hrwros_ws/src/hrwros_support/urdf/unknown_obstacle/unknown_obstacle.urdf"), "r") as box_file:
      box_xml = box_file.read()
    # Wait for a couple of seconds to prevent the unknown obstacle from being
    # considered as a part of the map.
    rospy.sleep(4)

    if not check_obstacle_existence('unknown_obstacle1'):
      # First obstacle.
      obstacle1_pose = Pose()
      obstacle1_pose.position.x = -1.0
      obstacle1_pose.position.y = 1.2
      obstacle1_pose.position.z = 0.0
      obstacle1_pose.orientation.x = 0.0
      obstacle1_pose.orientation.y = 0.0
      obstacle1_pose.orientation.z = 0.0
      obstacle1_pose.orientation.w = 1.0
      spawn_unknown_obstacle('unknown_obstacle1', box_xml, obstacle1_pose)

    if not check_obstacle_existence('unknown_obstacle2'):
      # Second obstacle.
      obstacle2_pose = Pose()
      obstacle2_pose.position.x = -1.0
      obstacle2_pose.position.y = 3.0
      obstacle2_pose.position.z = 0.0
      obstacle2_pose.orientation.x = 0.0
      obstacle2_pose.orientation.y = 0.0
      obstacle2_pose.orientation.z = 0.0
      obstacle2_pose.orientation.w = 1.0
      spawn_unknown_obstacle('unknown_obstacle2', box_xml, obstacle2_pose)

  except rospy.ROSInterruptException:
    rospy.loginfo("Interrupt received to stop ROS node.")

```

> move.py

```python
#!/usr/bin/env python
import rospy
import sys
# Brings in the SimpleActionClient
import actionlib
from actionlib_msgs.msg import GoalStatus

# Brings in the messages used by the MoveBase action, including the
# goal message and the result message.
from move_base_msgs.msg import MoveBaseAction, MoveBaseGoal, MoveBaseResult, MoveBaseActionResult

if __name__ == '__main__':
    try:
        # Initialize a ROS Node
        rospy.init_node('move_turtlebot')
        # Create a SimpleActionClient for the move_base action server.
        turtlebot_navigation_client = actionlib.SimpleActionClient('move_base', MoveBaseAction)
        # Wait until the move_base action server becomes available.
        rospy.loginfo("Waiting for move_base action server to come up...")
        turtlebot_navigation_client.wait_for_server()
        rospy.loginfo("move_base action server is available...")
        # Creates a goal to send to the action server.
        turtlebot_robot1_goal = MoveBaseGoal()
        # Construct the target pose for the turtlebot in the "map" frame.
        turtlebot_robot1_goal.target_pose.header.stamp = rospy.Time.now()
        turtlebot_robot1_goal.target_pose.header.frame_id = "map"
        turtlebot_robot1_goal.target_pose.header.seq = 1
        turtlebot_robot1_goal.target_pose.pose.position.x = 2
        turtlebot_robot1_goal.target_pose.pose.position.y = 1
        turtlebot_robot1_goal.target_pose.pose.position.z = 0.0
        turtlebot_robot1_goal.target_pose.pose.orientation.x = 0.0
        turtlebot_robot1_goal.target_pose.pose.orientation.y = 0.0
        turtlebot_robot1_goal.target_pose.pose.orientation.z = 0.0
        turtlebot_robot1_goal.target_pose.pose.orientation.w = 1.0
        # Send the goal to the action server.
        turtlebot_navigation_client.send_goal(turtlebot_robot1_goal)
        rospy.loginfo("Goal sent to move_base action server.")
        # Wait for the server to finish performing the action.
        turtlebot_navigation_client.wait_for_result()
        # Display a log message depending on the navigation result.
        navigation_result_status = turtlebot_navigation_client.get_state()
        if GoalStatus.SUCCEEDED != navigation_result_status:
            rospy.logerr('Navigation to the desired goal failed :(. Sorry, try again!)')
        else:
            rospy.loginfo('Hooray! Successfully reached the desired goal!')
    except rospy.ROSInterruptException:
        rospy.loginfo("Interrupt received to stop ROS node.")

```
