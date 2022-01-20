---
layout: post
title: Visualization에 과한 방법들.
category: Machine Vision
tag: Machine Vision
---


### Pointcloud 방법

```c++
sensor_msgs::PointCloud2Ptr NV(new sensor_msgs::PointCloud2);
NVVisualized(input_cloud, normals, NV);

void NVVisualized(const sensor_msgs::PointCloud2Ptr& input, const std::vector<Normal>& normals, sensor_msgs::PointCloud2Ptr& output )
{
output->header = input->header;
output->width = input->width;
output->height = input->height;

output->fields.resize(4);

output->fields[0].name = "x";
output->fields[0].offset = 0;
output->fields[0].datatype = 7;
output->fields[0].count = 1;
output->fields[1].name = "y";
output->fields[1].offset = 4;
output->fields[1].datatype = 7;
output->fields[1].count = 1;
output->fields[2].name = "z";
output->fields[2].offset = 8;
output->fields[2].datatype = 7;
output->fields[2].count = 1;
output->fields[3].name = "rgb";
output->fields[3].offset = 12;
output->fields[3].datatype = 7;
output->fields[3].count = 1;

output->is_bigendian = 0;
output->point_step = sizeof(float) * 4;

output->row_step = output->point_step * output->width;

output->is_dense = 0;

output->data.resize(output->row_step * output->height, 0);

std::size_t size = output->width * output->height;

sensor_msgs::PointCloud2Iterator<float> x_out(*output, "x");
sensor_msgs::PointCloud2Iterator<float> y_out(*output, "y");
sensor_msgs::PointCloud2Iterator<float> z_out(*output, "z");
sensor_msgs::PointCloud2Iterator<float> rgb_out(*output, "rgb");

sensor_msgs::PointCloud2ConstIterator<float> x_in(*input, "x");
sensor_msgs::PointCloud2ConstIterator<float> y_in(*input, "y");
sensor_msgs::PointCloud2ConstIterator<float> z_in(*input, "z");


for (std::size_t i = 0; i < size; ++i, ++x_in, ++y_in, ++z_in, ++x_out, ++y_out, ++z_out, ++rgb_out)
{
//std::copy(&(*x_in), &(*z_in), &(*x_out));
if (!std::isfinite(normals[i].x) || !std::isfinite(*x_in))
{
  *x_out = std::numeric_limits<float>::quiet_NaN();
  *y_out = std::numeric_limits<float>::quiet_NaN();
  *z_out = std::numeric_limits<float>::quiet_NaN();
  *rgb_out = std::numeric_limits<float>::quiet_NaN();
  continue;
}
*x_out = *x_in;
*y_out = *y_in;
*z_out = *z_in;
// assert(*x_in == *x_out);
// // printf("X in copy test\n");
// assert(*y_in == *y_out);
// assert(*z_in == *z_out);
// Normals are assumed to be normalized

uint8_t r = std::floor((normals[i].x * 0.5 + 0.5) * 255.0);
uint8_t g = std::floor((normals[i].y * 0.5 + 0.5) * 255.0);
uint8_t b = std::floor((normals[i].z * 0.5 + 0.5) * 255.0);

uint32_t rgb = ((uint32_t)r << 16 | (uint32_t)g <<8 | (uint32_t)b);
*rgb_out = *reinterpret_cast<float*>(&rgb);



}


}

```

### Eigen 방법


```c++
sensor_msgs::PointCloud2Ptr NV(new sensor_msgs::PointCloud2);
NVVisualizeation(transformed_cloud, cloud_indices, PointTypeVal::UNKNOWN,
                                m_base_name, width, height, NV);
something.publish(*NV);



void NVVisualizeation (const Eigen::MatrixXf& cloud_data, const std::vector<PointType>& indices, const PointTypeVal type,
                                const std::string& frame_name, int width, int height, sensor_msgs::PointCloud2Ptr& output)
{
output->header.frame_id = frame_name;
output->header.stamp = ros::Time::now();
output->width  = 1;
output->height = width * height;

output->fields.resize(3);

output->fields[0].name = "x";
output->fields[0].offset = 0;
output->fields[0].datatype = 7;
output->fields[0].count = 1;
output->fields[1].name = "y";
output->fields[1].offset = 4;
output->fields[1].datatype = 7;
output->fields[1].count = 1;
output->fields[2].name = "z";
output->fields[2].offset = 8;
output->fields[2].datatype = 7;
output->fields[2].count = 1;


output->is_bigendian = 0;
output->point_step = sizeof(float) * 3;

output->row_step = output->point_step ;

output->is_dense = 0;

output->data.resize(output->point_step * output->width * output->height, 0);

std::size_t size = output->width * output->height;

sensor_msgs::PointCloud2Iterator<float> x_out(*output, "x");
sensor_msgs::PointCloud2Iterator<float> y_out(*output, "y");
sensor_msgs::PointCloud2Iterator<float> z_out(*output, "z");

size_t count = 0;
for (int label_idx = indices.size() - 1; label_idx >= 0 ; label_idx--)
{
   if (indices[label_idx] == type || type == -2) // type -2 means include all
   {
      *x_out = cloud_data.col(label_idx).x();
      *y_out = cloud_data.col(label_idx).y();
      *z_out = cloud_data.col(label_idx).z();

      ++x_out;
      ++y_out;
      ++z_out;
      count++;
   }

}

output->height = count;
output->data.resize(output->point_step * count, 0);

}
```
