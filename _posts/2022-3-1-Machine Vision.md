---
layout: post
title: Multi Plane Segmenation Idea(42个PCL Tutorials)
category: Machine Vision
tag: Machine Vision
---

[Identifying ground returns using ProgressiveMorphologicalFilter segmentation](https://pointclouds.org/documentation/tutorials/progressive_morphological_filtering.html)

- Window Slide로 돌면서 Slope(포인터들의 Angle값)을 통해 ground와 Object를 구분한다.

```c++
#include <ros/ros.h>
#include <ros/time.h>
#include <nodelet/nodelet.h>
#include <std_msgs/Float32MultiArray.h>
#include <sensor_msgs/PointCloud2.h>
#include <sensor_msgs/Imu.h>
#include <pluginlib/class_list_macros.h>
#include <message_filters/subscriber.h>
#include <message_filters/sync_policies/approximate_time.h>
#include <message_filters/time_synchronizer.h>

#include <tf/transform_listener.h>
#include <geometry_msgs/Quaternion.h>
#include <tf2_geometry_msgs/tf2_geometry_msgs.h>

#include <pcl/point_types.h>
#include <pcl/point_cloud.h>
#include <pcl/filters/extract_indices.h>
#include <pcl/segmentation/sac_segmentation.h>
#include <pcl/kdtree/kdtree.h>
#include <pcl/search/kdtree.h>
#include <pcl/segmentation/extract_clusters.h>
#include <pcl/exceptions.h>
#include <pcl_ros/transforms.h>
#include <pcl_ros/point_cloud.h>
#include <pcl/features/normal_3d_omp.h>

#include <boost/shared_ptr.hpp>

#include <string>
#include <vector>
#include <math.h>

#define PI 3.14159265359

namespace soma_perception
{
  typedef pcl::PointXYZRGB PointT;
  typedef message_filters::sync_policies::ApproximateTime<sensor_msgs::PointCloud2, sensor_msgs::Imu> MySyncPolicy;

  class PlaneSegmentationNodelet : public nodelet::Nodelet
  {
  public:
    PlaneSegmentationNodelet() {}
    virtual ~PlaneSegmentationNodelet() {}

    virtual void onInit()
    {
      NODELET_INFO("Initializing PlaneSegmentationNodelet");

      nh = getNodeHandle();
      pnh = getPrivateNodeHandle();

      initialize_params();
      //advertise
      ground_pub = nh.advertise<sensor_msgs::PointCloud2>("cloud_ground", 1);
      slope_pub = nh.advertise<sensor_msgs::PointCloud2>("cloud_slope", 1);
      others_pub = nh.advertise<sensor_msgs::PointCloud2>("cloud_others", 1);
      normal_pub = nh.advertise<sensor_msgs::PointCloud2>("cloud_normals",1);
      //sub
      points_sub = new message_filters::Subscriber<sensor_msgs::PointCloud2>(nh, "input_points", 3);
      imu_sub = new message_filters::Subscriber<sensor_msgs::Imu>(nh, "input_imu", 3);
      sync = new message_filters::Synchronizer<MySyncPolicy>(MySyncPolicy(300), *points_sub, *imu_sub);
      sync->registerCallback(boost::bind(&PlaneSegmentationNodelet::cloud_callback, this, _1, _2));
    }

  private:
    /**
   * @brief initialize parameters
   */
    void initialize_params()
    {
      base_link_frame = pnh.param<std::string>("base_link", "soma_link");
      // times_of_rpeats = nh.param<int>("times_of_repeats", 2);
      setted_slope_tilt = pnh.param<float>("setted_slope_tilt", 25.0);
      setted_ground_tilt = pnh.param<float>("setted_ground_tilt", 3.0);
      distance_thres = pnh.param<double>("distance_thres", 0.01);
      cluster_tolerance = pnh.param<double>("cluster_tolerance", 0.1);
      min_clustersize = pnh.param<int>("min_clustersize", 50);
      radius_search= pnh.param<double>("radius_search", 0.1);
    }

    void cloud_callback(const sensor_msgs::PointCloud2ConstPtr &cloud_input,
                        const sensor_msgs::ImuConstPtr &imu_data)
    {
      NODELET_INFO("cloud_callback function");

      if (cloud_input->data.size() == 0)
      {
        NODELET_WARN("empty point cloud");
        return;
      }

      NODELET_INFO("input point cloud size : %d", (int)cloud_input->data.size());

      //--------------------------------------------------
      // (0) point cloud preprocessing
      // conversion and transform
      //--------------------------------------------------
      pcl::PointCloud<PointT>::Ptr cloud_raw;
      cloud_raw.reset(new pcl::PointCloud<PointT>());
      cloud_raw->clear();
      pcl::fromROSMsg(*cloud_input, *cloud_raw); //conversion

      pcl::PointCloud<PointT>::Ptr cloud_transformed;
      cloud_transformed.reset(new pcl::PointCloud<PointT>);
      cloud_transformed->clear();
      transform_pointCloud(cloud_raw, *cloud_transformed); //transform

      //--------------------------------------------------
      // (1) normal vector filtering process
      //--------------------------------------------------
      pcl::PointCloud<pcl::PointXYZRGBNormal>::Ptr normal_output(new pcl::PointCloud<pcl::PointXYZRGBNormal>);
      estimation_normal(cloud_transformed, normal_output);

      //--------------------------------------------------
      // (2) slope detection process
      //--------------------------------------------------
      pcl::PointCloud<PointT>::Ptr pc_ground(new pcl::PointCloud<PointT>());
      pcl::PointCloud<PointT>::Ptr pc_slope(new pcl::PointCloud<PointT>());
      pcl::PointCloud<PointT>::Ptr pc_others(new pcl::PointCloud<PointT>());
      detection_slope(cloud_transformed, imu_data, pc_ground, pc_slope, pc_others);
      // NODELET_INFO("total slope points:%5d", (int)pc_slope->size());
      // NODELET_INFO("total others points:%5d", (int)pc_others->size());

      //--------------------------------------------------
      // (3) euclidean cluster extraction process
      //--------------------------------------------------
      extraction_cluster(pc_slope);
      extraction_cluster(pc_ground);
      // NODELET_INFO("total cluster points:%5d", (int)pc_slope->size());

      //publish of results
      //パブリッシュする前に必ずframe_idとstampを設定すること
      //忘れるとrvizで描画できない
      pc_ground->header.frame_id = base_link_frame;
      pcl_conversions::toPCL(ros::Time::now(), pc_ground->header.stamp);
      ground_pub.publish(pc_ground);

      pc_slope->header.frame_id = base_link_frame;
      pcl_conversions::toPCL(ros::Time::now(), pc_slope->header.stamp);
      slope_pub.publish(pc_slope);

      pc_others->header.frame_id = base_link_frame;
      pcl_conversions::toPCL(ros::Time::now(), pc_others->header.stamp);
      others_pub.publish(pc_others);

      sensor_msgs::PointCloud2 pc_normal;
      pcl::toROSMsg(*normal_output, pc_normal);
      pc_normal.header.frame_id = base_link_frame;
      normal_pub.publish(pc_normal);

    }// finsh callback

    void transform_pointCloud(pcl::PointCloud<PointT>::Ptr input,
                              pcl::PointCloud<PointT> &output)
    {
      if (!base_link_frame.empty())
      {
        if (!tf_listener.canTransform(base_link_frame, input->header.frame_id, ros::Time(0)))
        {
          NODELET_WARN("cannot transform %s->%s", input->header.frame_id.c_str(), base_link_frame.c_str());
          return;
        }

        //get transform
        tf::StampedTransform transform;
        tf_listener.waitForTransform(base_link_frame, input->header.frame_id, ros::Time(0), ros::Duration(2.0));
        tf_listener.lookupTransform(base_link_frame, input->header.frame_id, ros::Time(0), transform);
        //apply transform
        pcl_ros::transformPointCloud(*input, output, transform);
        output.header.frame_id = base_link_frame;
        output.header.stamp = input->header.stamp;
        // NODELET_INFO("transformed point cloud (frame_id=%s)", output.header.frame_id.c_str());
      }
    }

    void estimation_normal(pcl::PointCloud<PointT>::Ptr input,
                           pcl::PointCloud<pcl::PointXYZRGBNormal>::Ptr output)
    {
      pcl::NormalEstimationOMP<PointT, pcl::Normal> impl;
      impl.setInputCloud(input);
      pcl::search::KdTree<PointT>::Ptr tree(new pcl::search::KdTree<PointT>);
      impl.setSearchMethod(tree);
      impl.setRadiusSearch(radius_search);
      pcl::PointCloud<pcl::Normal>::Ptr normal_cloud(new pcl::PointCloud<pcl::Normal>);
      impl.compute(*normal_cloud);

      output->points.resize(input->points.size());
      for (size_t i = 0; i < output->points.size(); i++) {
        pcl::PointXYZRGBNormal p;
        p.x = input->points[i].x;
        p.y = input->points[i].y;
        p.z = input->points[i].z;
        p.rgb = input->points[i].rgb;
        p.normal_x = normal_cloud->points[i].normal_x;
        p.normal_y = normal_cloud->points[i].normal_y;
        p.normal_z = normal_cloud->points[i].normal_z;
        output->points[i] = p;
      }
    }

    void detection_slope(pcl::PointCloud<PointT>::Ptr raw,
                         sensor_msgs::ImuConstPtr imu_data,
                         pcl::PointCloud<PointT>::Ptr &ground,
                         pcl::PointCloud<PointT>::Ptr &slope,
                         pcl::PointCloud<PointT>::Ptr &others)
    {
      pcl::PointIndices::Ptr inliers;
      pcl::ModelCoefficients::Ptr coeffs;
      inliers.reset(new pcl::PointIndices());
      coeffs.reset(new pcl::ModelCoefficients());
      pcl::PointCloud<PointT>::Ptr tmp(new pcl::PointCloud<PointT>());
      pcl::ExtractIndices<PointT> EI;
      pcl::copyPointCloud<PointT>(*raw, *tmp); //copyt to tempolary
      int count = 1;

      int ground_count = 0;
      int slope_count = 0;

      std_msgs::Float32MultiArray absolute_ary;
      std_msgs::Float32MultiArray relative_ary;
      while (1)
      {
        //plane detection
        int ret = segmentation(tmp, *inliers, *coeffs);
        if (ret == -1)
          break;
        // NODELET_INFO("plane size:%d", (int)inliers->indices.size());
        // NODELET_INFO("coeffs(a,b,c,d): %.2f, %.2f, %.2f, %.2f",
        //              coeffs->values[0], coeffs->values[1], coeffs->values[2], coeffs->values[3]);

        if (inliers->indices.size() > 30) //平面検出をやめる条件式はここ
        // if(count < 2) //1回だけ検出回す用
        {
          pcl::PointCloud<PointT>::Ptr tmp2(new pcl::PointCloud<PointT>());
          EI.setInputCloud(tmp);
          EI.setIndices(inliers);
          EI.setNegative(false);
          EI.filter(*tmp2);
          //--------------------------------------------------
          // calc relative tilt for segmented plane
          //--------------------------------------------------
          relative_ary.data.resize(count);
          tf2::Vector3 ex(1, 0, 0), ey(0, 1, 0), ez(0, 0, 1);
          tf2::Vector3 normal(coeffs->values[0], coeffs->values[1], coeffs->values[2]);
          float _relative_tilt = normal.dot(ez) / (normal.length() * ez.length());
          _relative_tilt = acos(_relative_tilt); //pitch angle [rad]
          float relative_tilt = atan2(sin(_relative_tilt), cos(_relative_tilt));
          relative_tilt = RAD2DEG(relative_tilt);
          // NODELET_INFO("relative_tilt: %5.2f", relative_tilt);
          relative_ary.data[count - 1] = relative_tilt;
          //--------------------------------------------------
          // calc absolute tilt for segmented plane
          //--------------------------------------------------
          tf2::Quaternion quat;
          tf2::fromMsg(imu_data->orientation, quat);
          absolute_ary.data.resize(count);
          tf2::Vector3 normal_world = tf2::quatRotate(quat, normal);
          // NODELET_INFO("ezw:%f, %f, %f, %f", normal_world.x(), normal_world.y(), normal_world.z(), normal_world.w());
          float _absolute_tilt = normal_world.dot(ez) / (normal_world.length() * ez.length());
          _absolute_tilt = acos(_absolute_tilt);
          float absolute_tilt = atan2(sin(_absolute_tilt), cos(_absolute_tilt));
          absolute_tilt = RAD2DEG(absolute_tilt);
          // NODELET_INFO("absolute_tilt: %5.2f", absolute_tilt);
          absolute_ary.data[count - 1] = absolute_tilt;
          //--------------------------------------------------
          // store the segmented plane into *ground or *slope
          //--------------------------------------------------
          if (relative_ary.data[count - 1] < setted_ground_tilt)
          {
            *ground += *tmp2;
            ground_count++;
            NODELET_INFO("ground %d times -> relative_ary.data[%d]: %f", ground_count, count, relative_ary.data[count-1]);
            NODELET_INFO("ground %d times -> absolute_ary.data[%d]: %f", ground_count, count, absolute_ary.data[count-1]);
          }
          else if (setted_slope_tilt < absolute_ary.data[count - 1])
          {
            *slope += *tmp2; //append (marge) points to result object
            slope_count++;
            NODELET_INFO("slope %d times -> relative_ary.data[%d]: %f", slope_count, count, relative_ary.data[count-1]);
            NODELET_INFO("slope %d times -> absolute_ary.data[%d]: %f", slope_count, count, absolute_ary.data[count-1]);
          }
          tmp2->clear();
          EI.setNegative(true);
          EI.filter(*tmp2); //extraction other points
          tmp->clear();
          pcl::copyPointCloud<PointT>(*tmp2, *tmp);
        }
        else
        {
          //条件を満たしたら平面検出終わり
          break;
        }
        count++;
      } //end while loop
      pcl::copyPointCloud<PointT>(*tmp, *others);
      return;
    }

    void extraction_cluster(pcl::PointCloud<PointT>::Ptr slope)
    {
      pcl::search::KdTree<PointT>::Ptr tree(new pcl::search::KdTree<PointT>);
      tree->setInputCloud(slope);
      std::vector<pcl::PointIndices> cluster_indices; //clusters
      pcl::EuclideanClusterExtraction<PointT> ec;
      ec.setClusterTolerance(cluster_tolerance); //[m] if two point distance is less than the tlerance, it will be cluster
      ec.setMinClusterSize(min_clustersize);    //number of points in a cluster
      ec.setMaxClusterSize(25000); //about
      ec.setSearchMethod(tree);    //set kd-tree
      ec.setInputCloud(slope);  //set input cloud
      ec.extract(cluster_indices);
      NODELET_INFO("Cluster num: %2d", (int)cluster_indices.size());

      pcl::PointCloud<PointT>::Ptr tmp_slope(new pcl::PointCloud<PointT>());
      for (int i = 0; i < cluster_indices.size(); i++)
      {
        pcl::PointCloud<PointT>::Ptr tmp(new pcl::PointCloud<PointT>);
        pcl::copyPointCloud(*slope, cluster_indices[i].indices, *tmp);
        *tmp_slope += *tmp;
      }
      slope->clear();
      pcl::copyPointCloud(*tmp_slope, *slope);
    }

    int segmentation(pcl::PointCloud<PointT>::Ptr input,
                     pcl::PointIndices &inliers,
                     pcl::ModelCoefficients &coeffs)
    {
      //そもそも入力点群数が少なすぎるとRANSACの計算ができない
      //RANSACのOptimizeCoefficientsをtrueにするときはたぶん10点以上ないとまずい
      if (input->size() < 10)
        return -1;

      //instance of RANSAC segmentation processing object
      pcl::SACSegmentation<PointT> sacseg;

      //set RANSAC parameters
      sacseg.setOptimizeCoefficients(true);
      sacseg.setModelType(pcl::SACMODEL_PLANE);
      sacseg.setMethodType(pcl::SAC_RANSAC);
      sacseg.setMaxIterations(100);
      sacseg.setDistanceThreshold(distance_thres); //[m]
      sacseg.setInputCloud(input);

      try
      {
        sacseg.segment(inliers, coeffs);
      }
      catch (const pcl::PCLException &e)
      {
        //エラーハンドリング (反応しない...)
        NODELET_WARN("Plane Model Detection Error");
        return -1; //failure
      }

      return 0; //success
    }

  private:
    ros::NodeHandle nh;
    ros::NodeHandle pnh;

    tf::TransformListener tf_listener;

    //params
    std::string base_link_frame; //base_link frame id
    float setted_slope_tilt;
    float setted_ground_tilt;
    double distance_thres; //segmentation_plane
    double cluster_tolerance;//extraction_cluster
    int min_clustersize;//extracion_cluster

    double radius_search;

    //subscribers
    message_filters::Subscriber<sensor_msgs::PointCloud2> *points_sub;
    message_filters::Subscriber<sensor_msgs::Imu> *imu_sub;
    message_filters::Synchronizer<MySyncPolicy> *sync;

    //publishers
    ros::Publisher ground_pub;
    ros::Publisher slope_pub;
    ros::Publisher others_pub;
    ros::Publisher normal_pub;
  };
} // namespace soma_perception

PLUGINLIB_EXPORT_CLASS(soma_perception::PlaneSegmentationNodelet, nodelet::Nodelet)
```

[Implemantation SLOPE DETECTION](https://github.com/hayashi-lab-soma/soma_pkg/blob/ad23b331e24853c4a977e559ca197fdff56e7c9e/soma_perception/src/plane_segmentation_nodelet.cpp)


[42个实列](https://blog.csdn.net/weixin_36662031/article/details/107528534)

[PCL Tutorial](https://pcl.readthedocs.io/projects/tutorials/en/latest/#segmentation)

[REFERENCE](https://blog.csdn.net/weixin_36662031?type=blog)

[example code](https://www.freesion.com/article/82121073186/)


### ORGANIZED POINTCLOUD by Integral Image(Covaraince MAtrix -> SVD -> Normal Surface Parameter -> Region Connecting by curvurter, angle, distance of normal surface of covariance matrix -> Ground and Object Segmenet)


### ORGANIZED POINTCLOUD by cross product(gradient -> Cross Prodcut -> Normal Surface Parameter -> Region Connecting by curvurter, angle, distance of normal surface  (Grouping) -> Ground and Object Segementation)

KD TREE를 이용하여서 정확한 그룹힝도 할 수 있을 것 같다.


느낌점 : PCL로 기본적인 것들을 다 할 수 있다. 즉, 오픈 소스를 통해서 웬만한 개발은 다할 수 있고, 이를 응용하여서 업그레이드 시킬 수 있다. 즉, 해당 소스들을 가져와서 포팅할 수있는 그런 능력이 필요하다.

input output만 맞춰주고, 안에 알고리즘을 가져와 쓰는 것이 중요하고, 직접 하나하나 해보면서 이해하는 것이 중요하다.

급하게 하지 말고 한땀 한땀 한다고 생각을하고 장인정신으로 꼼꼼하게 개발을 하고 거기다 customize할 수 있는 능력이 있으면 된다.
