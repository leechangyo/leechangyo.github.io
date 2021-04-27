---
layout: post
title: what is Pointcloud
category: Sensor fusion
tag: Sensor fusion
---

## What is pointcloud

What is a point cloud? A point cloud is the set of all the Lidar reflections that is measured. So each Lidar, each point is one laser beam going to the object being reflected, and that creates a point because the laser beam is relatively tight in diameter. Then you collect all of the reflections of those laser beams from the environment, and this creates this point cloud. The data Lidar generates depends on the principle of the Lidar field of you, and you know how dense the beams are. But let's call it a 100 megabyte per second roughly, can be the more can be less. If you have 64 lasers, obviously that thing generates way more data than if you have only two layers, or 10 or 30 layers. So it depends a little bit on the Lidar itself.


let’s dive into how lidar data is stored. Lidar data is stored in a format called Point Cloud Data (PCD for short). A .pcd file is a list of (x,y,z) cartesian coordinates along with intensity values, it’s a single snapshot of the environment, so after a single scan. That means with a VLP 64 lidar, a pcd file would have around 256,000 (x,y,z,i) values.

<a href="https://postimg.cc/Jt4LMKQ6"><img src="https://i.postimg.cc/7PSY1WJk/Picture1.png" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/yD9qvGVd"><img src="https://i.postimg.cc/zG2JXm6n/Picture12png.png" width="700px" title="source: imgur.com" /><a>

PCD of a city block with parked cars, and a passing van. Intensity values are being shown as different colors. The big black spot is where the car with the lidar sensor is located.

The coordinate system for point cloud data is the same as the car’s local coordinate system. In this coordinate system the x axis is pointing towards the front of the car, and the y axis is pointing to the left of the car. Also since this coordinate system is right-handed the z axis points up above the car

<a href="https://postimg.cc/XGKdL4P8"><img src="https://i.postimg.cc/63gc9QLD/Picture2.png" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/vDBZ3GCZ"><img src="https://i.postimg.cc/jq6nq5XP/Screen-Shot-2021-04-27-at-7-58-58-PM.png" width="700px" title="source: imgur.com" /><a>

## Pointcloud Tool

The tools engineers use depends obviously on the engineers. There are tools that are proprietary to each company or organization that have been developed over time. Ours come originally from working with the stereo cameras. There's also a very well-known library called a point cloud library, that deals with the main things are really segmentation, and separation of the ground plane, and things like that. Those kinds of tools where you do segmentation of the point cloud to get the images out of all the objects out of the point cloud, clustering segmentation, those things. There's also an open CV, are tools to work with point clouds.

In this module you will be working on processing point cloud data to find obstacles. All the code will be done in a C++ environment, so some familiarity with C++ will definitely be helpful. PCL is an open source C++ library for working with point clouds. You will be using it to visualize data, render shapes, and become familiar with some of its built in processing functions. Some documentation for PCL can be found [here](http://pointclouds.org/).

<a href="https://postimg.cc/p9bBJGTP"><img src="https://i.postimg.cc/k5Dh9LPR/11313123.png" width="700px" title="source: imgur.com" /><a>


PCL is widely used in the robotics community for working with point cloud data, and there are many tutorials available online for using it. There are a lot of built in functions in PCL that can help to detect obstacles. Built in PCL functions that will be used later in this module are Segmentation, Extraction, and Clustering.
