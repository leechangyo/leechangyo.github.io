---
layout: post
title: lidar on autonomous car in C++
category: Sensor fusion
tag: Sensor fusion
---
You would mount a lidar on the roof for the farthest field of view, but you mount a lidar on the roof it's not going to see anything that's happening close to the vehicle down more to the bottom. So to fill out the overall field of view of the senses of the vehicles, you mount lidars in front, in the back, or on the side depending on where you have gaps in the overall sense of coverage. We have a bunch on the roof kind of for maximizing the field of view, and we have some in the front and in the back to really cover the other areas that are rooftop lidars would not see. The relationship between the vertical field of view and the granularity, I think, let's take the velodyne lidar where you have a finite or a very discrete number of layed individual layers like 16 layers, 32 layers, 64 layers, and so on. Usually, all of those lidars have the same vertical field of view, expressing degrees, it's about 30 degrees. If you have 30 degrees and 16 layers, then you have two degrees spacing between the layers in the image. Now, two degrees in about 60 meters, you can hide a pedestrian between two layers. So that limits your actually your resolution, but it also limits in a sense how far you can see because if you have no scan layer on an object, then this object is invisible to the lidar. So the more layers you have in this vertical field of view, the finer the granularity is, the more objects you can see, and in a sense. The farther you can see within the overall power range off the lidar.

- Lidar ray creates

```c++
Lidar(std::vector<Car> setCars, double setGroundSlope)
  : cloud(new pcl::PointCloud<pcl::PointXYZ>()), position(0,0,2.6)
{
  // TODO:: set minDistance to 5 to remove points from roof of ego car
  minDistance = 5;
  maxDistance = 50;
  resoultion = 0.2;
  // TODO:: set sderr to 0.2 to get more interesting pcd files
  sderr = 0.2;
  cars = setCars;
  groundSlope = setGroundSlope;

  // TODO:: increase number of layers to 8 to get higher resoultion pcd
  int numLayers = 8;
  // the steepest vertical angle
  double steepestAngle =  30.0*(-pi/180);
  double angleRange = 26.0*(pi/180);
  // TODO:: set to pi/64 to get higher resoultion pcd
  double horizontalAngleInc = pi/64;

  double angleIncrement = angleRange/numLayers;

  for(double angleVertical = steepestAngle; angleVertical < steepestAngle+angleRange; angleVertical+=angleIncrement)
  {
    for(double angle = 0; angle <= 2*pi; angle+=horizontalAngleInc)
    {
      Ray ray(position,angle,angleVertical,resoultion);
      rays.push_back(ray);
    }
  }
}
```

<a href="https://postimg.cc/Wh6vmKS7"><img src="https://i.postimg.cc/jdBSTY7B/afasfasfs.png" width="700px" title="source: imgur.com" /><a>

- Note :

The syntax of PointCloud with the template is similar to the syntax of vectors or other std container libraries: ContainerName<ObjectName>.
The Ptr type from PointCloud indicates that the object is actually a pointer - a 32 bit integer that contains the memory address of your point cloud object. Many functions in pcl use point cloud pointers as arguments, so it's convenient to return the inputCloud in this form.
The renderRays function is defined in src/render. It contains functions that allow us to render points and shapes to the pcl viewer. You will be using it to render your lidar rays as line segments in the viewer.
The arguments for the renderRays function is viewer, which gets passed in by reference. This means that any changes to the viewer in the body of the renderRays function directly affect the viewer outside the function scope. The lidar position also gets passed in, as well as the point cloud that your scan function generated. The type of point for the PointCloud will be pcl::PointXYZ. We will talk about some other different types of point clouds in a bit.


## Template important(typename)
Templates and Different point cloud data
The lidar scan function used previously produced a pcl PointCloud object with pcl::PointXYZ points. The object uses a template because there are many different types of point clouds: some that are 3D, some that are 2D, some that include color and intensity. Here you are working with plain 3D point clouds so PointXYZ is used. However, later in the course you will have points with an intensity component as well.
Instead of defining two separate functions one with an argument for PointXYZ and the other for PointXYZI, templates can automate this process. With templates, you only have to write the function once and use the template like an argument to specify the point type.

If you haven’t used templates with pointers before, you may have noticed in the code that typename is used whenever a pointer is used that depends on a template. For example in the function signature here: (즉, typename 은 포인터를 사용할때 받아드리는 값을 보통 value로 받아 들이는데, 노 이거는 type이라고 정의를 해주는 것이다. 템플렛을 정의할때 많이 쓰인다)

```c++
typename pcl::PointCloud<PointT>::Ptr ProcessPointClouds<PointT>::FilterCloud(typename pcl::PointCloud<PointT>::Ptr cloud, float filterRes, Eigen::Vector4f minPoint, Eigen::Vector4f maxPoint)
```

The reason for this is the following: Given a piece of code with a type name parameter, like pcl::PointCloud<PointT>::Ptr, the compiler is unable to determine if the code is a value or a type without knowing the value for the type name parameter. The compiler will assume that the code represents a value. If the code actually represents a typename, you will need to specify that.
Test your own intuition with the quiz below. You can use this documentation for help.
