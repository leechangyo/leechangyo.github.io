---
layout: post
title: PointCloud Segmentation in C++
category: Sensor fusion
tag: Sensor fusion
---

##	Segmentation

We want to be able to locate obstacles in the scene. However, some objects in our scene our not obstacles. What would be objects that appear in the pcd but are not obstacles? For the most part, any free space on the road is not an obstacle, and if the road is flat it’s fairly straightforward to pick out road points from non-road points. **To do this we will use a method called Planar Segmentation which uses the RANSAC (random sample consensus) algorithm.**


## Why Segmentation

Engineers use LiDAR really how we mentioned it before by just recording the LiDAR signal driving around. We record all the data that comes back into the LiDAR. We create objects out of it or not, depending on the particular use case where we could do stuff directly with the point cloud. We can then use the objects to track those objects, make predictions, what they do. We can use it to classify objects. We have certain classes that the LiDAR using Neural Network can detect and distinguish. The segmentation is saying okay, this particular thing belongs to one object. This belongs to the ground plane, this belongs to a tree or so. So you associate certain points with objects and then say okay, this is one object, that set of points and this is another object, a set of objects or another object. This is how this basically works.


## Point Processing

The first thing you are going to do is create a processPointClouds object. This is defined by the src/processPointClouds.cpp and src/processPointClouds.h files. This object is going to contain all the methods that you will be using in this module for processing lidar data. The process object also has helper methods for loading and saving PCD files. By the time you compete this exercise, the code should compile but you will still need to complete the next few concepts before getting results.

<a href="https://postimg.cc/YvYrh3y5"><img src="https://i.postimg.cc/ht3fZZ8t/312312-Picture1.png" width="700px" title="source: imgur.com" /><a>
