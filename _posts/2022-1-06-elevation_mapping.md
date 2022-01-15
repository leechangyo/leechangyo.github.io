---
layout: post
title: Robot-Centric Elevation Mapping with uncertainty estimate
category: SLAM
tag: SLAM
---

- Goal : find a representation of the terrain that takes the uncertainties of the range measurements and the state estimation into account

- Approach : mapping approach fully incorporates the distance sensor measurement uncertainties and the six-dimensional pose covariance of the robot.

- Contribution : the uncertainty of the robot's position and orientation is reflected in the map by linearly growing the variance of the height estimate based on the accumulated distance and angle.

- Method : distance measurements are taken relative to the robot and when the robot moves, the entire elevation map is updated with information about the motion of robot. at any time, the robot-centric elevation map is an estimate which the system has about the shape of the terrian from a local perspective. the region ahead of the robot has typically the highest precision as it is constatnly updated with new measurements from the forward-looking distance sensor. on the other hand, regions which are out of the sensor's field of view(below or behind the robot) have decreased certainty due to drift the robot's relative pose estimation

- Specific : in this project, we can decide whether applying motion prediction or not.(we can update measurement variance update too).

### Measurement Update

to Obtain the variance of the height measurement, we derive the jacobians for the sensor measurement J_s and the sensor frame rotation J_q

- Note
  - Map : odom
  - base : base_link
  - sensor : camera_depth_optical_frame


#### 1. find Tranformation matrix


- Tranformation Matrix : [sensor to map], [base to sensor], [map to base]

```c++
bool SensorProcessorBase::updateTransformations(const ros::Time& timeStamp) {
  try {
    transformListener_.waitForTransform(sensorFrameId_, generalParameters_.mapFrameId_, timeStamp, ros::Duration(1.0));

    tf::StampedTransform transformTf;
    transformListener_.lookupTransform(generalParameters_.mapFrameId_, sensorFrameId_, timeStamp, transformTf);
    poseTFToEigen(transformTf, transformationSensorToMap_);

    transformListener_.lookupTransform(generalParameters_.robotBaseFrameId_, sensorFrameId_, timeStamp,
                                       transformTf);  // TODO(max): Why wrong direction?
    Eigen::Affine3d transform;
    poseTFToEigen(transformTf, transform);
    rotationBaseToSensor_.setMatrix(transform.rotation().matrix());
    translationBaseToSensorInBaseFrame_.toImplementation() = transform.translation();

    transformListener_.lookupTransform(generalParameters_.mapFrameId_, generalParameters_.robotBaseFrameId_, timeStamp,
                                       transformTf);  // TODO(max): Why wrong direction?
    poseTFToEigen(transformTf, transform);
    rotationMapToBase_.setMatrix(transform.rotation().matrix());
    translationMapToBaseInMapFrame_.toImplementation() = transform.translation();

    if (!firstTfAvailable_) {
      firstTfAvailable_ = true;
    }

    return true;
  } catch (tf::TransformException& ex) {
    if (!firstTfAvailable_) {
      return false;
    }
    ROS_ERROR("%s", ex.what());
    return false;
  }
}

```

- Transformed Pointcloud into sensorframe and map_frame

- and remove points out of range

```c++
bool SensorProcessorBase::process(const PointCloudType::ConstPtr pointCloudInput, const Eigen::Matrix<double, 6, 6>& robotPoseCovariance,
                                  const PointCloudType::Ptr pointCloudMapFrame, Eigen::VectorXf& variances, std::string sensorFrame) {
  sensorFrameId_ = sensorFrame;
  ROS_DEBUG("Sensor Processor processing for frame %s", sensorFrameId_.c_str());

  // Update transformation at timestamp of pointcloud
  ros::Time timeStamp;
  timeStamp.fromNSec(1000 * pointCloudInput->header.stamp);
  if (!updateTransformations(timeStamp)) {
    return false;
  }

  // Transform into sensor frame.
  PointCloudType::Ptr pointCloudSensorFrame(new PointCloudType);

  // pointcloudinput frame id can be same as sensorframeid_
  transformPointCloud(pointCloudInput, pointCloudSensorFrame, sensorFrameId_);

  // Remove Nans (optional voxel grid filter)
  filterPointCloud(pointCloudSensorFrame);

  // Specific filtering per sensor type
  filterPointCloudSensorType(pointCloudSensorFrame);

  // Remove outside limits in map frame
  if (!transformPointCloud(pointCloudSensorFrame, pointCloudMapFrame, generalParameters_.mapFrameId_)) {
    return false;
  }
  std::vector<PointCloudType::Ptr> pointClouds({pointCloudMapFrame, pointCloudSensorFrame});
  removePointsOutsideLimits(pointCloudMapFrame, pointClouds);

  // Compute variances
  return computeVariances(pointCloudSensorFrame, robotPoseCovariance, variances);
}

```

#### 2. Measurement Update(Variance Update)

- projection vector : xyz 공간상에 포인트들을 xy로 투영하는 것

<a href="https://postimg.cc/RW0Hghv2"><img src="https://i.postimg.cc/K8PDv3wc/download.png" width="700px" title="source: imgur.com" /><a>

- SensorJacobian(RotationMapToBased^T * RotationBaseToSensor) : Sensor Measurement

- rotation variance(pose covariance)

- Sessor frame rotation(skewmatrix is converting vector to rotation matrix)


```
bool StereoSensorProcessor::computeVariances(const PointCloudType::ConstPtr pointCloud,
                                             const Eigen::Matrix<double, 6, 6>& robotPoseCovariance, Eigen::VectorXf& variances) {
  variances.resize(pointCloud->size());

  // Projection vector (P).
  const Eigen::RowVector3f projectionVector = Eigen::RowVector3f::UnitZ();

  // Sensor Jacobian (J_s).
  const Eigen::RowVector3f sensorJacobian =
      projectionVector * (rotationMapToBase_.transposed() * rotationBaseToSensor_.transposed()).toImplementation().cast<float>();

  // Robot rotation covariance matrix (Sigma_q).
  Eigen::Matrix3f rotationVariance = robotPoseCovariance.bottomRightCorner(3, 3).cast<float>();

  // Preparations for#include <pcl/common/transforms.h> robot rotation Jacobian (J_q) to minimize computation for every point in point
  // cloud.
  const Eigen::Matrix3f C_BM_transpose = rotationMapToBase_.transposed().toImplementation().cast<float>();
  const Eigen::RowVector3f P_mul_C_BM_transpose = projectionVector * C_BM_transpose;
  const Eigen::Matrix3f C_SB_transpose = rotationBaseToSensor_.transposed().toImplementation().cast<float>();
  const Eigen::Matrix3f B_r_BS_skew =
      kindr::getSkewMatrixFromVector(Eigen::Vector3f(translationBaseToSensorInBaseFrame_.toImplementation().cast<float>()));

  for (unsigned int i = 0; i < pointCloud->size(); ++i) {
    // For every point in point cloud.

    // Preparation.
    pcl::PointXYZRGBConfidenceRatio point = pointCloud->points[i];
    double disparity = sensorParameters_.at("depth_to_disparity_factor") / point.z;  // NOLINT(cppcoreguidelines-pro-type-union-access)
    Eigen::Vector3f pointVector(point.x, point.y, point.z);  // S_r_SP // NOLINT(cppcoreguidelines-pro-type-union-access)
    float heightVariance = 0.0;                              // sigma_p

    // Measurement distance.
    float measurementDistance = pointVector.norm();

    // Compute sensor covariance matrix (Sigma_S) with sensor model.
    float varianceNormal =
        pow(sensorParameters_.at("depth_to_disparity_factor") / pow(disparity, 2), 2) *
        ((sensorParameters_.at("p_5") * disparity + sensorParameters_.at("p_2")) *
             sqrt(pow(sensorParameters_.at("p_3") * disparity + sensorParameters_.at("p_4") - getJ(i), 2) + pow(240 - getI(i), 2)) +
         sensorParameters_.at("p_1"));
    float varianceLateral = pow(sensorParameters_.at("lateral_factor") * measurementDistance, 2);
    Eigen::Matrix3f sensorVariance = Eigen::Matrix3f::Zero();
    sensorVariance.diagonal() << varianceLateral, varianceLateral, varianceNormal;

    // Robot rotation Jacobian (J_q).
    const Eigen::Matrix3f C_SB_transpose_times_S_r_SP_skew = kindr::getSkewMatrixFromVector(Eigen::Vector3f(C_SB_transpose * pointVector));
    Eigen::RowVector3f rotationJacobian = P_mul_C_BM_transpose * (C_SB_transpose_times_S_r_SP_skew + B_r_BS_skew);

    // Measurement variance for map (error propagation law).
    heightVariance = rotationJacobian * rotationVariance * rotationJacobian.transpose();
    heightVariance += sensorJacobian * sensorVariance * sensorJacobian.transpose();

    // Copy to list.
    variances(i) = heightVariance;
  }

```

<a href="https://postimg.cc/njKW00kF"><img src="https://i.postimg.cc/TwBX2s5D/Kakao-Talk-Image-2022-01-15-12-00-39.jpg" width="700px" title="source: imgur.com" /><a>

update varaince to existing heightmap

#### 3. Motion Model Update

- elevation map에다가 robo pose에 대한 motion update를 맵에다 해준다.

- data reading

```
// Get robot pose at requested time.
boost::shared_ptr<geometry_msgs::PoseWithCovarianceStamped const> poseMessage = robotPoseCache_.getElemBeforeTime(time);
if (!poseMessage) {
  // Tell the user that either for the timestamp no pose is available or that the buffer is possibly empty
  if (robotPoseCache_.getOldestTime().toSec() > lastPointCloudUpdateTime_.toSec()) {
    ROS_ERROR("The oldest pose available is at %f, requested pose at %f", robotPoseCache_.getOldestTime().toSec(),
              lastPointCloudUpdateTime_.toSec());
  } else {
    ROS_ERROR("Could not get pose information from robot for time %f. Buffer empty?", lastPointCloudUpdateTime_.toSec());
  }
  return false;
}

kindr::HomTransformQuatD robotPose;
kindr_ros::convertFromRosGeometryMsg(poseMessage->pose.pose, robotPose);
// Covariance is stored in row-major in ROS: http://docs.ros.org/api/geometry_msgs/html/msg/PoseWithCovariance.html
Eigen::Matrix<double, 6, 6> robotPoseCovariance =
    Eigen::Map<const Eigen::Matrix<double, 6, 6, Eigen::RowMajor>>(poseMessage->pose.covariance.data(), 6, 6);

// Compute map variance update from motion prediction.
robotMotionMapUpdater_.update(map_, robotPose, robotPoseCovariance, time);

return true;
```

- reduce covariance matrix between two robot poses

- tangent pitch meaning : update yawJacobian
  - it is super useful to find covariance matrix of vehicle about rotation motion


<a href="https://postimg.cc/4YmNqhjk"><img src="https://i.postimg.cc/2SxbqQw6/image005.png" width="700px" title="source: imgur.com" /><a>

```c

bool RobotMotionMapUpdater::update(ElevationMap& map, const Pose& robotPose, const PoseCovariance& robotPoseCovariance,
                                   const ros::Time& time) {
  const PoseCovariance robotPoseCovarianceScaled = covarianceScale_ * robotPoseCovariance;

  // Check if update necessary.
  if (previousUpdateTime_ == time) {
    return false;
  }

  // Initialize update data.
  grid_map::Size size = map.getRawGridMap().getSize();
  grid_map::Matrix varianceUpdate(size(0), size(1));  // TODO(max): Make as grid map?
  grid_map::Matrix horizontalVarianceUpdateX(size(0), size(1));
  grid_map::Matrix horizontalVarianceUpdateY(size(0), size(1));
  grid_map::Matrix horizontalVarianceUpdateXY(size(0), size(1));

  // Relative convariance matrix between two robot poses.
  ReducedCovariance reducedCovariance;
  computeReducedCovariance(robotPose, robotPoseCovarianceScaled, reducedCovariance);
  ReducedCovariance relativeCovariance;
  computeRelativeCovariance(robotPose, reducedCovariance, relativeCovariance);

  // Retrieve covariances for (24).
  Covariance positionCovariance = relativeCovariance.topLeftCorner<3, 3>();
  Covariance rotationCovariance(Covariance::Zero());
  rotationCovariance(2, 2) = relativeCovariance(3, 3);

  // Map to robot pose rotation (R_B_M = R_I_B^T * R_I_M).
  kindr::RotationMatrixPD mapToRobotRotation = kindr::RotationMatrixPD(robotPose.getRotation().inverted() * map.getPose().getRotation());
  kindr::RotationMatrixPD mapToPreviousRobotRotationInverted =
      kindr::RotationMatrixPD(previousRobotPose_.getRotation().inverted() * map.getPose().getRotation()).inverted();

  // Translation Jacobian (J_r) (25).
  Eigen::Matrix3d translationJacobian = -mapToRobotRotation.matrix().transpose();

  // Translation variance update (for all points the same).
  Eigen::Vector3f translationVarianceUpdate =
      (translationJacobian * positionCovariance * translationJacobian.transpose()).diagonal().cast<float>();

  // Map-robot relative position (M_r_Bk_M, for all points the same).
  // Preparation for (25): M_r_BP = R_I_M^T (I_r_I_M - I_r_I_B) + M_r_M_P
  // R_I_M^T (I_r_I_M - I_r_I_B):
  const kindr::Position3D positionRobotToMap =
      map.getPose().getRotation().inverseRotate(map.getPose().getPosition() - previousRobotPose_.getPosition());

  auto& heightLayer = map.getRawGridMap()["elevation"];

  // For each cell in map. // TODO(max): Change to new iterator.
  for (unsigned int i = 0; i < static_cast<unsigned int>(size(0)); ++i) {
    for (unsigned int j = 0; j < static_cast<unsigned int>(size(1)); ++j) {
      kindr::Position3D cellPosition;  // M_r_MP

      const auto height = heightLayer(i, j);
      if (std::isfinite(height)) {
        grid_map::Position position;
        map.getRawGridMap().getPosition({i, j}, position);
        cellPosition = {position.x(), position.y(), height};

        // Rotation Jacobian J_R (25)
        const Eigen::Matrix3d rotationJacobian =
            -kindr::getSkewMatrixFromVector((positionRobotToMap + cellPosition).vector()) * mapToPreviousRobotRotationInverted.matrix();

        // Rotation variance update.
        const Eigen::Matrix2f rotationVarianceUpdate =
            (rotationJacobian * rotationCovariance * rotationJacobian.transpose()).topLeftCorner<2, 2>().cast<float>();

        // Variance update.
        varianceUpdate(i, j) = translationVarianceUpdate.z();
        horizontalVarianceUpdateX(i, j) = translationVarianceUpdate.x() + rotationVarianceUpdate(0, 0);
        horizontalVarianceUpdateY(i, j) = translationVarianceUpdate.y() + rotationVarianceUpdate(1, 1);
        horizontalVarianceUpdateXY(i, j) = rotationVarianceUpdate(0, 1);
      } else {
        // Cell invalid. // TODO(max): Change to new functions
        varianceUpdate(i, j) = std::numeric_limits<float>::infinity();
        horizontalVarianceUpdateX(i, j) = std::numeric_limits<float>::infinity();
        horizontalVarianceUpdateY(i, j) = std::numeric_limits<float>::infinity();
        horizontalVarianceUpdateXY(i, j) = std::numeric_limits<float>::infinity();
      }
    }
  }

  map.update(varianceUpdate, horizontalVarianceUpdateX, horizontalVarianceUpdateY, horizontalVarianceUpdateXY, time);
  previousReducedCovariance_ = reducedCovariance;
  previousRobotPose_ = robotPose;
  return true;
}

bool RobotMotionMapUpdater::computeReducedCovariance(const Pose& robotPose, const PoseCovariance& robotPoseCovariance,
                                                     ReducedCovariance& reducedCovariance) {
  // Get augmented Jacobian (A.4).
  // 오일러 각은 강체가 놓인 방향을 3차원 공간에 표시하기 위해 레온하르트 오일러가 도입한 세 개의 각도이다. 즉, 3차원 회전군 SO의 한 좌표계다. 3차원 공간에 놓인 강체의 방향은 오일러 각도를 사용하여 세 번의 회전을 통해 얻을 수 있다.
  kindr::EulerAnglesZyxPD eulerAngles(robotPose.getRotation()); // get eulerangle(오일러각)
  double tanOfPitch = tan(eulerAngles.pitch());
  // (A.5)
  Eigen::Matrix<double, 1, 3> yawJacobian(cos(eulerAngles.yaw()) * tanOfPitch, sin(eulerAngles.yaw()) * tanOfPitch, 1.0);
  Eigen::Matrix<double, 4, 6> jacobian;
  jacobian.setZero();
  jacobian.topLeftCorner(3, 3).setIdentity();
  jacobian.bottomRightCorner(1, 3) = yawJacobian;

  // (A.3)
  reducedCovariance = jacobian * robotPoseCovariance * jacobian.transpose();
  return true;
}

bool RobotMotionMapUpdater::computeRelativeCovariance(const Pose& robotPose, const ReducedCovariance& reducedCovariance,
                                                      ReducedCovariance& relativeCovariance) {
  // Rotation matrix of z-align frame R_I_tilde_B.
  const kindr::RotationVectorPD rotationVector_I_B(robotPose.getRotation());
  const kindr::RotationVectorPD rotationVector_I_tilde_B(0.0, 0.0, rotationVector_I_B.vector().z());
  const kindr::RotationMatrixPD R_I_tilde_B(rotationVector_I_tilde_B);

  // Compute translational velocity from finite differences.
  kindr::Position3D positionInRobotFrame =
      previousRobotPose_.getRotation().inverseRotate(robotPose.getPosition() - previousRobotPose_.getPosition());
  kindr::Velocity3D v_Delta_t(positionInRobotFrame);  // (A.8)

  // Jacobian F (A.8).
  Jacobian F;
  F.setIdentity();
  // TODO(max): Why does Eigen::Vector3d::UnitZ() not work?
  F.topRightCorner(3, 1) = kindr::getSkewMatrixFromVector(Eigen::Vector3d(0.0, 0.0, 1.0)) * R_I_tilde_B.matrix() * v_Delta_t.vector();

  // Jacobian inv(G) * Delta t (A.14).
  Jacobian inv_G_Delta_t;
  inv_G_Delta_t.setZero();
  inv_G_Delta_t(3, 3) = 1.0;
  Jacobian inv_G_transpose_Delta_t(inv_G_Delta_t);
  inv_G_Delta_t.topLeftCorner(3, 3) = R_I_tilde_B.matrix().transpose();
  inv_G_transpose_Delta_t.topLeftCorner(3, 3) = R_I_tilde_B.matrix();

  // Relative (reduced) robot covariance (A.13).
  relativeCovariance = inv_G_Delta_t * (reducedCovariance - F * previousReducedCovariance_ * F.transpose()) * inv_G_transpose_Delta_t;

  return true;
}
```

- 즉 sensor Measurement로 map을 업데이트하고 이전 모션과 현재 모션시 생기는 Update(Jacobian * (preivous covraince - current covartiance) * Jacobian ^T)된 Varaince를 맵에다 업데이트를 하여서 센서와 모션 퓨전으로 맵들을 업데이트한다.(locally하데 )
