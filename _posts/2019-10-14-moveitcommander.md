---
layout: post
title: Moveit Commander problem
category: ROS
tag: ROS
---

when we get this error

> import moveit_commander
Failed to import pyassimp, see https://github.com/ros-planning/moveit/issues/86 for more info

the pyassimp problem

when single import pyassimp problem

>Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python2.7/dist-packages/pyassimp/__init__.py", line 1, in <module>

we got this error

# first solution
- install pyssimp library

# second

- go to /usr/lib/python2.7/dist-packages/pyassimp
- and open core.py
- and change it

```python
--- old/core.py	2016-06-07 14:13:11.948880646 +0200
+++ new/core.py	2016-06-07 14:12:59.357306268 +0200
@@ -30,7 +30,7 @@
     """
     Assimp-Singleton
     """
-    load, load_mem, release, dll = helper.search_library()
+    load_mem, release, dll = helper.search_library()
 _assimp_lib = AssimpLib()

 def make_tuple(ai_obj, type = None):
 ```

and run. it will work

# Reference
https://github.com/shadow-robot/sr_interface/issues/234
