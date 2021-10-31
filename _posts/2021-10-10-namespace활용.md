---
layout: post
title: 나만의 모듈화 및 몇가지 구조화 규칙[C++]
category: Programming
tag: Programming
---

프로젝트를 하다보면 모듈을 담당해서 개발하게 된다. 이때 모듈을 개발을 할때 namespace를 쓴다.

또한 class를 call할때는 explicit으로 하여서 콜을 할때 한가지 방법으로만 콜을 하도록 한다.

virtual destructor를 하는데, 그 이유는 모듈안에 겹치는 class가 존재할 수 있기 때문에,

특정 call class destructor를 하고자 virtual 을 쓴다.

헤더 파일

```c
namespace elevation_mapping {

/*!
 * Elevation map stored as grid map handling elevation height, variance, color etc.
 */
class ElevationMap {
 public:
  /*!
   * Constructor.
   */
  explicit ElevationMap(ros::NodeHandle nodeHandle);

  /*!
   * Destructor.
   */
  virtual ~ElevationMap();
};
```

cpp 파일

```c
namespace elevation_mapping {

ElevationMap::ElevationMap(ros::NodeHandle nodeHandle)
    : nodeHandle_(nodeHandle),
      rawMap_({"elevation", "variance", "horizontal_variance_x", "horizontal_variance_y", "horizontal_variance_xy", "color", "time",
               "dynamic_time", "lowest_scan_point", "sensor_x_at_lowest_scan", "sensor_y_at_lowest_scan", "sensor_z_at_lowest_scan"}),
      fusedMap_({"elevation", "upper_bound", "lower_bound", "color"}),
      postprocessorPool_(nodeHandle.param("postprocessor_num_threads", 1), nodeHandle_),
      hasUnderlyingMap_(false),
      minVariance_(0.000009),
      maxVariance_(0.0009),
      mahalanobisDistanceThreshold_(2.5),
      multiHeightNoise_(0.000009),
      minHorizontalVariance_(0.0001),
      maxHorizontalVariance_(0.05),
      enableVisibilityCleanup_(true),
      enableContinuousCleanup_(false),
      visibilityCleanupDuration_(0.0),
      scanningDuration_(1.0) {
  rawMap_.setBasicLayers({"elevation", "variance"});
  fusedMap_.setBasicLayers({"elevation", "upper_bound", "lower_bound"});
  clear();

  elevationMapFusedPublisher_ = nodeHandle_.advertise<grid_map_msgs::GridMap>("elevation_map", 1);
  if (!underlyingMapTopic_.empty()) {
    underlyingMapSubscriber_ = nodeHandle_.subscribe(underlyingMapTopic_, 1, &ElevationMap::underlyingMapCallback, this);
  }
  // TODO(max): if (enableVisibilityCleanup_) when parameter cleanup is ready.
  visibilityCleanupMapPublisher_ = nodeHandle_.advertise<grid_map_msgs::GridMap>("visibility_cleanup_map", 1);

  initialTime_ = ros::Time::now();
}

ElevationMap::~ElevationMap() = default;
```

이런식으로 모듈을 개발을 하는 것이 가독성이 좋고 효율적이다.
