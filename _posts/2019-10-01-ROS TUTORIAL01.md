---
layout: post
title: 0.1 ROS Basic
category: ROS
tag: ROS
---
# Sensor_info

> SensorInformation.msg

```msg
sensor_msgs/Range sensor_data
string maker_name
uint32 part_number
```

> sensor_info_publisher.py

```python
#!/usr/bin/env python
import rospy
from hrwros_msgs.msg import SensorInformation
from hrwros_utilities.sim_sensor_data import distSensorData as getSensorData

def sensorInfoPublisher():
    si_publisher = rospy.Publisher('sensor_info', SensorInformation, queue_size = 10)
    rospy.init_node('sensor_info_publisher', anonymous=False)
    rate = rospy.Rate(10)

    # Create a new SensorInformation object and fill in its contents.
    sensor_info = SensorInformation()

    # Fill in the header information.
    sensor_info.sensor_data.header.stamp = rospy.Time.now()
    sensor_info.sensor_data.header.frame_id = 'distance_sensor_frame'

    # Fill in the sensor data information.
    sensor_info.sensor_data.radiation_type = sensor_info.sensor_data.ULTRASOUND
    sensor_info.sensor_data.field_of_view = 0.5 # Field of view of the sensor in rad.
    sensor_info.sensor_data.min_range = 0.02 # Minimum distance range of the sensor in m.
    sensor_info.sensor_data.max_range = 2.0 # Maximum distance range of the sensor in m.

    # Fill in the manufacturer name and part number.
    sensor_info.maker_name = 'The Ultrasound Company'
    sensor_info.part_number = 123456

    while not rospy.is_shutdown():
        # Read the sensor data from a simulated sensor.
        sensor_info.sensor_data.range = getSensorData(sensor_info.sensor_data.radiation_type,
            sensor_info.sensor_data.min_range, sensor_info.sensor_data.max_range)

        # Publish the sensor information on the /sensor_info topic.
        si_publisher.publish(sensor_info)
        # Print log message if all went well.
        rospy.loginfo("All went well. Publishing topic ")
        rate.sleep()

if __name__ == '__main__':
    try:
        sensorInfoPublisher()
    except rospy.ROSInterruptException:
        pass
```

> sensor_info_subscriber.py

```python
#!/usr/bin/env python
import rospy
from hrwros_msgs.msg import SensorInformation

def sensorInfoCallback(data):
    rospy.loginfo('Distance reading from the sensor is: %f', data.sensor_data.range)

def sensorInfoListener():

    # In ROS, nodes are uniquely named. If two nodes with the same
    # name are launched, the previous one is kicked off. The
    # anonymous=True flag means that rospy will choose a unique
    # name for our 'sensorInfoListener' node so that multiple listeners can
    # run simultaneously.
    rospy.init_node('sensor_info_subscriber', anonymous=False)

    rospy.Subscriber('sensor_info', SensorInformation, sensorInfoCallback)

    # spin() simply keeps python from exiting until this node is stopped
    rospy.spin()

if __name__ == '__main__':
    sensorInfoListener()
```

# Template

>template_publisher.py

```python
#!/usr/bin/env python
import rospy
from std_msgs.msg import String

def simplePublisher():
    simple_publisher = rospy.Publisher('topic_1', String, queue_size = 10)
    rospy.init_node('node_1', anonymous=False)
    rate = rospy.Rate(10)

    topic1_content = "Welcome to Hello (Real) World with ROS!!!"

    while not rospy.is_shutdown():
        simple_publisher.publish(topic1_content)
        rate.sleep()

if __name__ == '__main__':
    try:
        simplePublisher()
    except rospy.ROSInterruptException:
        pass

```

>template_subscriber.py

```python
#!/usr/bin/env python
import rospy
from std_msgs.msg import String

def stringListenerCallback(data):
    rospy.loginfo('%s', data.data)

def stringListener():

    # In ROS, nodes are uniquely named. If two nodes with the same
    # name are launched, the previous one is kicked off. The
    # anonymous=True flag means that rospy will choose a unique
    # name for our 'stringListener' node so that multiple listeners can
    # run simultaneously.
    rospy.init_node('node_2', anonymous=False)

    rospy.Subscriber('topic_1', String, stringListenerCallback)

    # spin() simply keeps python from exiting until this node is stopped
    rospy.spin()

if __name__ == '__main__':
    stringListener()
```

# Metres to feet

> ConvertMetresToFeet.srv

```
float64 distance_metres		# Request message: Distance in (m) to be converted to feet.
---
float64 distance_feet		# Response message: Distance in feet after conversion.
bool success				# Response message: Success or failure of conversion.
```

> metres_to_feet_server.py

```python
#!/usr/bin/env python
# This code has been adapted from the ROS Wiki ROS Service tutorials to the context
# of this course.
# (http://wiki.ros.org/ROS/Tutorials/WritingServiceClient%28python%29)

from hrwros_msgs.srv import ConvertMetresToFeet, ConvertMetresToFeetRequest, ConvertMetresToFeetResponse
import rospy
import numpy as np

_CONVERSION_FACTOR_METRES_TO_FEET = 3.28 # Metres -> Feet conversion factor.

# Service callback function.
def process_service_request(req):

    # Instantiate the response message object.
    res = ConvertMetresToFeetResponse()

    # Perform sanity check. Allow only positive real numbers.
    # Compose the response message accordingly.
    if(req.distance_metres < 0):
        res.success = False
        res.distance_feet = -np.Inf # Default error value.
    else:
        res.distance_feet = _CONVERSION_FACTOR_METRES_TO_FEET * req.distance_metres
        res.success = True

    #Return the response message.
    return res

def metres_to_feet_server():
    # ROS node for the service server.
    rospy.init_node('metres_to_feet_server', anonymous = False)

    # Create a ROS service type.
    service = rospy.Service('metres_to_feet', ConvertMetresToFeet, process_service_request)

    # Log message about service availability.
    rospy.loginfo('Convert metres to feet service is now available.')
    rospy.spin()

if __name__ == "__main__":
    metres_to_feet_server()
```

> metres_to_feet_client.py

```python
#!/usr/bin/env python
# This code has been adapted from the ROS Wiki ROS Service tutorials to the context
# of this course.
# (http://wiki.ros.org/ROS/Tutorials/WritingServiceClient%28python%29)

import sys
import rospy
from hrwros_msgs.srv import ConvertMetresToFeet, ConvertMetresToFeetRequest, ConvertMetresToFeetResponse

def metres_to_feet_client(x):
    # First wait for the service to become available.
    rospy.loginfo("Waiting for service...")
    rospy.wait_for_service('metres_to_feet')
    try:
        # Create a service proxy.
        metres_to_feet = rospy.ServiceProxy('metres_to_feet', ConvertMetresToFeet)

        # Call the service here.
        service_response = metres_to_feet(x)

        print("I only got here AFTER the service call was completed!")

        # Return the response to the calling function.
        return service_response

    except rospy.ServiceException, e:
        print "Service call failed: %s"%e

if __name__ == "__main__":

    # Initialize the client ROS node.
    rospy.init_node("metres_to_feet_client", anonymous = False)

    # The distance to be converted to feet.
    dist_metres = 0.25

    rospy.loginfo("Requesting conversion of %4.2f m to feet"%(dist_metres))

    # Call the service client function.
    service_response = metres_to_feet_client(dist_metres)

    # Process the service response and display log messages accordingly.
    if(not service_response.success):
        rospy.logerr("Conversion unsuccessful! Requested distance in metres should be a positive real number.")
    else:
        rospy.loginfo("%4.2f(m) = %4.2f feet"%(dist_metres, service_response.distance_feet))
        rospy.loginfo("Conversion successful!")

```

# counter_with_delay

> launch file

```xml
<?xml version="1.0"?>
<launch>
  <!-- Argument to the launch file.-->
  <arg name="counter_delay_parameter" default="1.0"/>

  <!-- Start the metres_to_feet service server ROS node.-->
  <node name="metres_to_feet" pkg="hrwros_week1" type="metres_to_feet_server.py"
    output="screen"/>

  <!-- Start the action server ROS node if the start_as argument is true /-->
  <node name="counter_with_delay" pkg="hrwros_week1" type="counter_with_delay_as.py"
    output="screen">
    <param name="counter_delay" type="double" value="$(arg counter_delay_parameter)"/>
  </node>

</launch>
```

> CounterWithDelay.action

```
uint32 num_counts			# Goal message: number of counts to count up to.
---
string result_message		# Result message: simple string message for the result.
---
uint32 counts_elapsed		# Feedback message: number of counts elapsed.
```

> counter_with_delay_as.py

```python
#! /usr/bin/env python

# This code has been adapted from the ROS Wiki actionlib tutorials to the context
# of this course.
# (http://wiki.ros.org/hrwros_msgs/Tutorials)

import rospy

import actionlib

from hrwros_msgs.msg import CounterWithDelayAction, CounterWithDelayFeedback, CounterWithDelayResult

class CounterWithDelayActionClass(object):
    # create messages that are used to publish feedback/result
    _feedback = CounterWithDelayFeedback()
    _result = CounterWithDelayResult()

    def __init__(self, name):
        # This will be the name of the action server which clients can use to connect to.
        self._action_name = name

        # Create a simple action server of the newly defined action type and
        # specify the execute callback where the goal will be processed.
        self._as = actionlib.SimpleActionServer(self._action_name, CounterWithDelayAction, execute_cb=self.execute_cb, auto_start = False)

        # Start the action server.
        self._as.start()
        rospy.loginfo("Action server started...")

    def execute_cb(self, goal):
        # OPTIONAL: NOT GRADED Check if the parameter for the counter delay is available on the parameter server.
        # if not rospy.has_param("<write your code here>"):
        #     rospy.loginfo("Parameter %s not found on the parameter server. Using default value of 1.0s for counter delay.","counter_delay")
        #     counter_delay = 1.0
        # else:
        # Get the parameter for delay between counts.
        counter_delay_value = rospy.get_param("<write your code here>")
        rospy.loginfo("Parameter %s was found on the parameter server. Using %fs for counter delay."%("counter_delay", counter_delay_value))

        # Variable to decide the final state of the action server.
        success = True

        # publish info to the console for the user
        rospy.loginfo('%s action server is counting up to  %i with %fs delay between each count' % (self._action_name, goal.num_counts, counter_delay_value))

        # start executing the action
        for counter_idx in range(0, goal.num_counts):
            # check that preempt has not been requested by the client
            if self._as.is_preempt_requested():
                rospy.loginfo('%s: Preempted' % self._action_name)
                self._as.set_preempted() # Preempted 획득하다
                success = False
                break
            # publish the feedback
            self._feedback.counts_elapsed = counter_idx
            self._as.publish_feedback(self._feedback)
            # Wait for counter_delay s before incrementing the counter.
            <write your code here>

        if success:
            self._result.result_message = "Successfully completed counting."
            rospy.loginfo('%s: Succeeded' % self._action_name)
            self._as.set_succeeded(self._result)

if __name__ == '__main__':
    # Initialize a ROS node for this action server.
    rospy.init_node('counter_with_delay')

    # Create an instance of the action server here.
    server = CounterWithDelayActionClass(rospy.get_name())
    rospy.spin()

```

> counter_with_delay_ac.py

```python
#! /usr/bin/env python
from __future__ import print_function
import rospy

# Brings in the SimpleActionClient
import actionlib

# Brings in the messages used by the CounterWithDelay action, including the
# goal message and the result message.
from hrwros_msgs.msg import CounterWithDelayAction, CounterWithDelayGoal, CounterWithDelayResult

def counter_with_delay_client():
    # Creates the SimpleActionClient, passing the type of the action
    # (CounterWithDelayAction) to the constructor.
    client = actionlib.SimpleActionClient('counter_with_delay', CounterWithDelayAction)

    # Waits until the action server has started up and started
    # listening for goals.
    rospy.loginfo("Waiting for action server to come up...")
    client.wait_for_server()

    num_counts = 10

    # Creates a goal to send to the action server.
    goal = CounterWithDelayGoal(num_counts)

    # Sends the goal to the action server.
    client.send_goal(goal)

    rospy.loginfo("Goal has been sent to the action server.")

    # Waits for the server to finish performing the action.
    client.wait_for_result()

    # Prints out the result of executing the action
    return client.get_result()  # A CounterWithDelayResult

if __name__ == '__main__':
    try:
        # Initializes a rospy node so that the SimpleActionClient can
        # publish and subscribe over ROS.
        rospy.init_node('counter_with_delay_ac')
        result = counter_with_delay_client()
        rospy.loginfo(result.result_message)
    except rospy.ROSInterruptException:
        print("program interrupted before completion", file=sys.stderr)

```
