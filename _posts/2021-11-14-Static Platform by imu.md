---
layout: post
title: Static Platform IMU 활용
category: Machine Vision
tag: Machine Vision
---

### IMU로 Static Platform IMU 활용

```c++
// number of callback do it one function using boost::bind
m_gyro_sub_list[i]  = nh->subscribe<sensor_msgs::Imu>(gyro_topic, 2, boost::bind(&ObstacleExtractor::gyroCb, this, _1, i));

// Process the imu pose
const Eigen::Vector3d& theta_estimated = m_imu_pose_estimator_list[idx]->getTheta();
// const Eigen::Vector3d theta_estimated(0,0,0);
Eigen::AngleAxisd rollAngle(-theta_estimated.x(), Eigen::Vector3d::UnitX());
Eigen::AngleAxisd yawAngle(-theta_estimated.z(), Eigen::Vector3d::UnitZ());
Eigen::AngleAxisd pitchAngle(-(theta_estimated.y() + M_PI / 2), Eigen::Vector3d::UnitY());
// std::cout<<"Eigen angle\n" << theta_estimated * 180 / M_PI << std::endl;
// Eigen::Quaterniond q_optical_to_robot_link(0.5, 0.5, -0.5, 0.5);
Eigen::Quaterniond q_optical_to_robot_link(0.5, -0.5, 0.5, -0.5);

Eigen::Quaterniond q = rollAngle * yawAngle * pitchAngle;

geometry_msgs::PoseStamped pose_stamped;
geometry_msgs::Pose pose;
pose.orientation.w = q.w();
pose.orientation.x = q.x();
pose.orientation.y = q.y();
pose.orientation.z = q.z();
pose.position.x = 0.0;
pose.position.y = 0.0;
pose.position.z = 0.0;

pose_stamped.pose = pose;
pose_stamped.header.frame_id = m_base_name; // m_camera_extrinsics[idx].header.frame_id;
m_pub_imu_pose_list[idx].publish(pose_stamped);


// q = q.inverse();
// q = q * q_optical_to_robot_link;
q = q * q_optical_to_robot_link;
m_camera_extrinsics[idx].transform.rotation.w = q.w();
m_camera_extrinsics[idx].transform.rotation.x = q.x();
m_camera_extrinsics[idx].transform.rotation.y = q.y();
m_camera_extrinsics[idx].transform.rotation.z = q.z();
```
