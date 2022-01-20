---
layout: post
title: 효과적으로 Pointcloud 만들기
category: Machine Vision
tag: Machine Vision
---

### Camera instrict parameter

```c++
sensor_msgs::CameraInfoConstPtr cam_info = ros::topic::waitForMessage<sensor_msgs::CameraInfo>(cam_info_topic, *nh, ros::Duration(5.0));
struct CamParam
{
    float fx;
    float fy;
    float cx;
    float cy;

    int width;
    int height;
    CamParam(float _fx, float _fy, float _cx, float _cy, int _width, int _height) :
        fx(_fx), fy(_fy), cx(_cx), cy(_cy), width(_width), height(_height)
    {

    }
};
```


### generate 3d Point from pixel data

```c++

float cur_depth = (float) depth_data[cur_idx] / 1000.0f;
Eigen::Vector3f point_3d_cam;
ProjectPixel(i, j, cur_depth, camera_param_list_[idx], point_3d_cam);

void ProjectPixel(int u, int v, float depth, const CamParam& cam_info, Eigen::Vector3f& point_3d)
{
    point_3d.x() = (u - cam_info.cx) * depth / cam_info.fx;
    point_3d.y() = (v - cam_info.cy) * depth / cam_info.fy;
    point_3d.z() = depth;
}

if (point_3d_cam.y() <  -ROI_MAX_HEIGHT_ || point_3d_cam.y() > -ROI_MIN_HEIGHT_ ||
    point_3d_cam.z() > ROI_MAX_DEPTH_ || point_3d_cam.z() < ROI_MIN_DEPTH_    )
{
    continue;
}
```

### making Empty Pointcloud

```c++
inline void constructEmptyXYZPointCloud(sensor_msgs::PointCloud2Ptr& cloud, std::string frame_name, int width, int height)
{
  cloud->header.frame_id = frame_name;
  cloud->header.stamp = ros::Time::now();

  cloud->width = width;
  cloud->height = height;

  cloud->fields.resize(3);
  cloud->fields[0].name = "x";
  cloud->fields[0].offset = 0;
  cloud->fields[0].datatype = 7;
  cloud->fields[0].count = 1;
  cloud->fields[1].name = "y";
  cloud->fields[1].offset = 4;
  cloud->fields[1].datatype = 7;
  cloud->fields[1].count = 1;
  cloud->fields[2].name = "z";
  cloud->fields[2].offset = 8;
  cloud->fields[2].datatype = 7;
  cloud->fields[2].count = 1;

  cloud->is_bigendian = 0;
  cloud->point_step = sizeof(float) * 3;

  cloud->row_step = cloud->point_step * cloud->width;

  cloud->is_dense = 0;

  cloud->data.resize(cloud->row_step * cloud->height, 0);
}
```

### point cloud data

Pointer pointing data to read

```c++
std::shared_ptr<const Eigen::MatrixXf> m_input_cloud_ptr;
m_input_cloud_ptr = std::make_shared<const Eigen::MatrixXf>(cloud_data);
```


### Pointcloud data 쉽게 관리하기

```c++
class Pointcloud
{
public:
    using Ptr = std::shared_ptr<Pointcloud>;
    using ConstPtr = std::shared_ptr<const Pointcloud>;
};

```

### 쉽게 Multi Sensor Subscribers 하기

``` c++
// number of callback do it one function using boost::bind
sub_list[i]  = nh->subscribe<sensor_msgs::somthing>(topic, 2, boost::bind(&Cb, _1, i));

// image subscribe at heap.
m_img_sub_vec[i] = std::make_shared<image_transport::Subscriber>( it.subscribe(img_topic, 2, boost::bind(&depthCb, _1, i) ) );

```

### 쉽게 Multi Sensor Subscribes2 하기

```c++
std::string img_name = "/depth/image_rect_raw";
std::string cam_info_name = "/depth/camera_info";
for (int i = 0; i < 2; i++)
{
    std::string img_topic = "/camera" + indices[i] + img_name;
    std::string cam_info_topic = "/camera" + indices[i] + cam_info_name;

    // Wait for the camera info topics  
    sensor_msgs::CameraInfoConstPtr cam_info =
        ros::topic::waitForMessage<sensor_msgs::CameraInfo>(cam_info_topic, nh, ros::Duration(10.0));
    if (cam_info)
    {
        cam_params_.emplace_back(cam_info->K[0], cam_info->K[4], cam_info->K[2], cam_info->K[5]);
        printf("Received the camera info for index %s\n", indices[i].c_str());
    }
    else
    {
        printf("Could not received the camera info\n" );
        return -1;
    }

    // register it in heap space.
    std::shared_ptr<image_transport::Subscriber> sub_img =
        std::make_shared<image_transport::Subscriber>( it.subscribe(img_topic, 2, boost::bind(&imgCb, _1, i))); // std::atoi(indices[i].c_str())
    sub_clouds.push_back(sub_img);
}

// register end, it running in here.
ros::Rate r(0.2);

// Check for the cloud
while (current_frame_list_[0] != number_of_frames_ &&
       current_frame_list_[1] != number_of_frames_)
{

    r.sleep();
    ros::spinOnce();
}
```

### Point Type indices

```c++
enum PointTypeVal
{
    INVALID_TYPE = -1,
    UNKNOWN = 0,
    GROUND = 1,
    GRADIENT_EDGE = 2,
    UNCLASSIFIED_OBSTACLES = 3
};

std::vector<PointTypeVal> point_type_indices;
```
