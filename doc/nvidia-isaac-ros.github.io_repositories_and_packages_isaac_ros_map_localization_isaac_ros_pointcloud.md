- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Map Localization](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/index.html)
- `isaac_ros_pointcloud_utils`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.rst.txt)

* * *

# `isaac_ros_pointcloud_utils` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_map_localization/blob/main/isaac_ros_pointcloud_utils).

## Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html\#overview "Link to this heading")

The ROS nodes in this package allows for [sensor\_msgs::msg::PointCloud2](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/PointCloud2.msg) and [sensor\_msgs::msg::LaserScan](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/LaserScan.msg) to be converted to [isaac\_ros\_pointcloud\_interfaces::msg::FlatScan](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common/blob/main/isaac_ros_pointcloud_interfaces/msg/FlatScan.msg) which can then be used with the [isaac\_ros\_occupancy\_grid\_localizer](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html).

## API [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html\#api "Link to this heading")

### Isaac ROS PointCloud to FlatScan [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html\#isaac-ros-pointcloud-to-flatscan "Link to this heading")

#### Usage [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html\#usage "Link to this heading")

```
ros2 launch isaac_ros_pointcloud_utils isaac_ros_pointcloud_to_flatscan.launch.py

```

Copy to clipboard

#### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html\#ros-parameters "Link to this heading")

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `threshold_x_axis` | bool | `false` | Enable X Axis Threshold |
| `threshold_y_axis` | bool | `false` | Enable Y Axis Threshold |
| `min_x` | double | `-1.0` | Min X axis threshold |
| `max_x` | double | `1.0` | Max X axis threshold |
| `min_y` | double | `-1.0` | Min Y axis threshold |
| `max_y` | double | `1.0` | Max Y axis threshold |
| `min_z` | double | `-0.1` | Min Z axis threshold |
| `max_z` | double | `0.1` | Max Z axis threshold |
| `max_points` | int | `150000` | Maximum number of 3D points in input Point Cloud used to pre allocate GPU memory |

#### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html\#ros-topics-subscribed "Link to this heading")

| ROS Topic | Type | Description |
| --- | --- | --- |
| `pointcloud` | [sensor\_msgs::msg::PointCloud2](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/PointCloud2.msg) | Input Point Cloud |

#### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html\#ros-topics-published "Link to this heading")

| ROS Topic | Type | Description |
| --- | --- | --- |
| `flatscan` | [isaac\_ros\_pointcloud\_interfaces::msg::FlatScan](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common/blob/main/isaac_ros_pointcloud_interfaces/msg/FlatScan.msg) | Output Flat Scan |

### Isaac ROS LaserScan to FlatScan [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html\#isaac-ros-laserscan-to-flatscan "Link to this heading")

#### Usage [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html\#id1 "Link to this heading")

```
ros2 launch isaac_ros_pointcloud_utils isaac_ros_laserscan_to_flatscan.launch.py

```

Copy to clipboard

#### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html\#id2 "Link to this heading")

| ROS Topic | Type | Description |
| --- | --- | --- |
| `scan` | [sensor\_msgs::msg::LaserScan](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/LaserScan.msg) | Input LaserScan |

#### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html\#id3 "Link to this heading")

| ROS Topic | Type | Description |
| --- | --- | --- |
| `flatscan` | [isaac\_ros\_pointcloud\_interfaces::msg::FlatScan](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common/blob/main/isaac_ros_pointcloud_interfaces/msg/FlatScan.msg) | Output Flat Scan |

### Isaac ROS FlatScan to LaserScan [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html\#isaac-ros-flatscan-to-laserscan "Link to this heading")

#### Usage [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html\#id4 "Link to this heading")

```
ros2 launch isaac_ros_pointcloud_utils isaac_ros_flatscan_to_laserscan.launch.py

```

Copy to clipboard

#### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html\#id5 "Link to this heading")

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `angle_min` | double | `0.0` | The starting angle of the generated LaserScan |
| `angle_max` | double | `2 * M_PI` | The ending angle of the generated LaserScan |
| `angle_increment` | double | `M_PI / 180` | The angle increment per LaserScan reading |
| `time_increment` | double | `0.0001` | The time increment per LaserScan reading |
| `max_range_fallback` | double | `200.0` | If the Max Range of the input FlatScan == 0, then this parameter is used to populate ‘Max Range’ field of the output LaserScan. |

#### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html\#id6 "Link to this heading")

|     |     |     |
| --- | --- | --- |
| `flatscan` | [isaac\_ros\_pointcloud\_interfaces::msg::FlatScan](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common/blob/main/isaac_ros_pointcloud_interfaces/msg/FlatScan.msg) | Input Flat Scan |

#### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html\#id7 "Link to this heading")

| ROS Topic | Type | Description |
| --- | --- | --- |
| `scan` | [sensor\_msgs::msg::LaserScan](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/LaserScan.msg) | Input LaserScan |

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_map_localization/isaac_ros_pointcloud_utils/index.html)