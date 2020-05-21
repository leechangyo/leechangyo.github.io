---
layout: post
title: 1. Make Msg file and scripts
category: ROS
tag: ROS
---

# 1. Create Msg
- folder inisde project name "msg"
- create file "Twoints.msg"
- put it the data format

> TwoInts.msg

```pyhton
int16 a
int16 b
```

- we can use publish or script from the Twoints.msg(int16 a/int16 b)
- and go to CMakeList.txt, add it msg file in the "add_message_files "

> CMakeList.txt

```python
# Generate messages in the 'msg' folder
add_message_files(
  FILES
  TwoInts.msg
)
```
- and roscd /cd.. catkin_make

# 2. use it (1)

> two_int_talker.py

```python
#!/usr/bin/env python  
import random

import rospy
from std_msgs.msg import Int16
from project1_solution.msg import TwoInts

def talker():
    pub = rospy.Publisher('two_ints', TwoInts, queue_size=10)
    rospy.init_node('two_int_talker', anonymous=True)
    rate = rospy.Rate(0.5)  
    random.seed()

    while not rospy.is_shutdown():

        msg = TwoInts()

        msg.a = random.randint(1,20)
        msg.b = random.randint(1,20)
        pub.publish(msg)
        rate.sleep()


if __name__ == '__main__':
    try:
        talker()
    except rospy.ROSInterruptException:
        pass
```

# 2. use it (1)

> sum.py

```python
#!/usr/bin/env python  
import rospy

from std_msgs.msg import Int16
from project1_solution.msg import TwoInts

def cb(msg):
    global SumMsg,addPub
    SumMsg = msg.a + msg.b
    addPub.publish(SumMsg) # publish in the addpub topic("/sum")

rospy.init_node('solution')

SumMsg = Int16()
addPub = rospy.Publisher('/sum',Int16,queue_size=1)
Sub = rospy.Subscriber('/two_ints',TwoInts,cb) #cb is callback the data read it in the function

rospy.spin()

```
