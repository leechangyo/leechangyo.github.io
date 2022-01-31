---
layout: post
title: Visual Odometery[VINS, ORBSLAM] in lidar map
category: Visual SLAM
tag: Visual SLAM
---

<a href="https://postimg.cc/D8DJPYq1"><img src="https://i.postimg.cc/CxMGY35r/Screen-Shot-2022-01-31-at-6-42-37-PM.png" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/wyL8nMP6"><img src="https://i.postimg.cc/50k4YFSF/Screen-Shot-2022-01-31-at-6-50-28-PM.png" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/K4j6NjR8"><img src="https://i.postimg.cc/SQfmmz4C/Screen-Shot-2022-01-31-at-6-43-04-PM.png" width="700px" title="source: imgur.com" /><a>


### Align pcd pointcloud and visual pointcloud(Main Algorithm here)

```c++
// world coordiante and camera pose
Eigen::Matrix4f vllm_camera = T_align * offset_camera;
Eigen::Matrix4f last_camera = vllm_camera;
// Align pointclouds beween pcd and visual slam data
optimize::Aligner aligner(config.gain.scale, config.gain.latitude, config.gain.altitude, config.gain.smooth);
// relative pose to world cooridnate, visual data, pcd data, pointcloud correspondency, camera pose, T_world pose history, scale, pcd normal pointcloud
T_align = aligner.estimate7DoF(
T_align, vslam_data, map_ptr->getTargetCloud(), correspondences,
offset_camera, vllm_history, config.ref_scale, map_ptr->getTargetNormals());

// Integrate
vllm_camera = T_align * offset_camera;
// Get Inovation
float scale = util::getScale(vllm_camera);
float update_transform = (last_camera - vllm_camera).topRightCorner(3, 1).norm(); // called "Euclid distance"
float update_rotation = (last_camera - vllm_camera).topLeftCorner(3, 3).norm() / scale; // called "chordal distance"
std::cout << "update= \033[33m" << update_transform << " \033[m,\033[33m " << update_rotation << "\033[m" << std::endl;
std::cout << "T_align\n\033[4;36m"
<< T_align << "\033[m" << std::endl;
if (config.threshold_translation > update_transform
&& config.threshold_rotation > update_rotation)
break;
}
Outcome outcome;
// 从pcl::correspondence中访问点云中对应点的坐标
// mathicng points
outcome.correspondences = correspondences; // pcl correspondences
outcome.T_align = T_align; // pose
return outcome;

```
### REFERENCE

[GPS/INS/视觉 融合 、 自己采集数据测试](https://blog.csdn.net/hltt3838/article/details/110148851)
