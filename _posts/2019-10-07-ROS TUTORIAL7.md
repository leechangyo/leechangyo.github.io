---
layout: post
title: 7. URDF
category: ROS
tag: Tutorial
---
# URDF - Overview
- limitation of URDF
  - robot descriptions cannot be changed
  - only tree structures
  - No <sensor></sensor>
  - Low reusability of URDFs
- Limitation 1 - immutability(불변성)
  - result of typical control flow
  - robot description read only once from parameter server
  - no standardized way to notify consumers of changes
  - risk of desynchronization of nodes
- Limitation 2 - no cycles
  - joints have only a single parent and child
  - only acyclic(비순환적인), directed graphs(or:trees) can be modeled
  - Real-world impact: dual-arm manipulation, parallel grippers, etc
- Limitation3 - reusability
  - Only a single <robot> tag in a URDF
  - No support for import of remote files
  - Composite scenes have to be merged manually
  - No way to compose multiple URDFs

- Solution: Xacro(XML Macros)
  - Programmatic URDF generation
  - Templates
  - Parameters

- Xacro:Composability(결합성)?
  - Import macros from other files
  - paramerize templates
  - composite robots & scenes easier:
    - import macro from file
    - invoke macro
# URDF

- Defined the structure of the robot
  - link
  - joint
- Defined basic geometry
  - Box
- Update the dimensions
- specify a color
- place it on the floor
- put it in the right location
- Adjust the size the shape
- Raise the object so that it rests on the floor
- change the color
- update its location
- the default origin for primitive shapes is the center of the shape, which means that the box is half sunk into the floor. to fix this, the z offset the origin needs to be changed to half the height of the object
- to change the location of the object, the offset from the world needs to be redefined. this is done by changing the x and y coordinates of the "origin" element within the joint that connects the object to the world

## object create Step
1. Import the model definition(xacro macro)
2. Add the model to the factory(instantiation)
3. fixating(정착시키다) it in place(joint)
4. updating orientation
5. the import statement consists of a few parts:
  - the xacro:include statement itself
  - the filename attribute specifying the name of the file to import(using a package relative path)
6. Finally, we also need to add the macro cell, which will actually instantiate(예시) the model. to prevent nameclashes, we'll configure the new object to use a unique "prefix"
  - to connect the robot to factory we will connect it to the pedestal rather than the world itself.
  - this is so that if the pedestal ever need to be moved, the robot will move with it, rather than having to update the robot's position manually. this is done by specifying the parent in the joint as the pedestal and the child as the robot
  - finally, the orientation needs to be corrected. this is done by adding an rpy attribute to the origin element that specifies the orientation.
  - As Ros uses radian for angles, we'll make use of the radians convenience function which will do conversion for us

# Checking for errors
- validating URDFs : check_urdf
- checks syntax(ie: legal combinations of URDF keywords)
- not semantics(ie: meaning or real-world correctness)

> running

```
check_urdf tiny_robot.urdf
```
