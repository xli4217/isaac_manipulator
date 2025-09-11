- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- Isaac ROS Nova
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_nova/index.rst.txt)

* * *

# Isaac ROS Nova [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html\#repo-name "Link to this heading")

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/Nova_Carter_Isaac_KV_540p_01_v002_DM.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/Nova_Carter_Isaac_KV_540p_01_v002_DM.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/Nova_Carter_Isaac_KV_540p_01_v002_DM.png/)

## Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html\#overview "Link to this heading")

[Isaac ROS Nova](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova) provides a set of optimized packages and tools to interface with the Nova sensor suite.
These packages integrate with hardware timestamp synchronization on Jetson Orin platforms to enable high-quality sensor fusion.
Sensor data streams through Isaac ROS graphs using [NITROS](https://nvidia-isaac-ros.github.io/concepts/nitros/index.html) for NVIDIA-accelerated processing.

Note

Isaac ROS Nova requires having run `nova-orin-init` [here](https://nvidia-isaac-ros.github.io/nova/nova_init/index.html#install) on your compute node before using any of the included packages.

- [Hesai Pandar XT32 3D LiDAR](https://www.hesaitech.com/product/xt32/)

- [Leopard Imaging HAWK stereo camera](https://leopardimaging.com/leopard-imaging-hawk-stereo-camera/)

- [Leopard Imaging OWL monocular camera](https://leopardimaging.com/product/automotive-cameras/cameras-by-interface/maxim-gmsl-2-cameras/li-ar0234cs-gmsl2-owl/li-ar0234cs-gmsl2-owl/)

- [Bosch BMI088 IMU](https://www.bosch-sensortec.com/products/motion-sensors/imus/bmi088/)


## Quickstart [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html\#quickstart "Link to this heading")

It is recommended to use Nova through the [Isaac ROS Nova meta-package](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova/blob/main/isaac_ros_nova). This package will launch all the sensor drivers for your defined Nova system. Guidelines and instructions on its use can be found [here](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova/index.html).

## Multi-camera ROS performance [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html\#multi-camera-ros-performance "Link to this heading")

Autonomy in robotics requires perception around a robot. AI based perception functions depend on time synchronized acquisition of sensor data for an understanding of the robot and its environment to plan the robots actions to perform its task.

Nova provides simultaneous start of image capture on multiple cameras to simplify perception functions, as objects and the environment are in a coherent capture at the same time across all cameras; this avoids the complexity of compensating for motion of the robot, and moving objects when cameras capture independently.

![Image above contains a composite of 8 camera image captures from four Hawk cameras with Nova Orin, time synchronized as observed by the external high precision LED visual timer.](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/hawk_sync.png/)

Performance of simultaneous image capture is measured as the jitter between start of frame across all cameras in the system, using hardware timestamps, and verified with an independent high speed precision LED timer. Nova measures a typical jitter on start of frame across all cameras to less than ±20 us (microseconds). Any jitter higher than ±100 us is considered not synchronized and a failure. Failure rates are measured over a specific time interval, using [Sigma Levels](https://en.wikipedia.org/wiki/Six_Sigma#Sigma_Levels).

| Mode | 1x Hawk | 2x Hawk | 3x Hawk | 4x Hawk | 4x Owl | 4x Hawk + 4x Owl |
| --- | --- | --- | --- | --- | --- | --- |
| 1900x1200 color RGGB @ 30fps | 6-sigma+ [\[Note\]](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html#note) | 6-sigma+ [\[Note\]](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html#note) | 6-sigma+ [\[Note\]](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html#note) | 6-sigma | 6-sigma+ [\[Note\]](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html#note) | 1-sigma |
| megapixels / second | 132 | 264 | 396 | 527 | 264 | 791 |

\[Note\]( [1](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html#id1), [2](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html#id2), [3](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html#id3), [4](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html#id4))

For entries labeled 6-sigma+, no drops were detected during the 8 hour test recordings.

This table shows the sigma-levels and megapixel processing rates for multi-camera system in ROS 2 with a data recording application, which includes image capture, image processing, auto exposure adjustments, image compression to H.264, and writes to NvME disk with Nova Orin for 8 hours. `isaac_ros_data_validation` is used to verify the recording as written to disk. Validation confirms there are no frame drops, de-synchronization or image capture jitter greater than ±100 us. The scene includes a strobe light, which induces auto exposure updates, and a high precision LED timer to verify ground truth synchronization.

## Isaac ROS NITROS Acceleration [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html\#isaac-ros-nitros-acceleration "Link to this heading")

This package is powered by [NVIDIA Isaac Transport for ROS (NITROS)](https://developer.nvidia.com/blog/improve-perception-performance-for-ros-2-applications-with-nvidia-isaac-transport-for-ros/), which leverages type adaptation and negotiation to optimize message formats and dramatically accelerate communication between participating nodes.

## Packages [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html\#packages "Link to this heading")

- [`isaac_ros_correlated_timestamp_driver`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html)
  - [Overview](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html#overview)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html#quickstart)
  - [Troubleshooting](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html#troubleshooting)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html#api)
- [`isaac_ros_data_recorder`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/index.html)
  - [Overview](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/index.html#overview)
  - [Tutorials](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/index.html#tutorials)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/index.html#api)
- [`isaac_ros_data_replayer`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html)
  - [Overview](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html#overview)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html#quickstart)
  - [Troubleshooting](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html#troubleshooting)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html#api)
- [`isaac_ros_data_validation`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_validation/index.html)
  - [Overview](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_validation/index.html#overview)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_validation/index.html#quickstart)
  - [Troubleshooting](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_validation/index.html#troubleshooting)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_validation/index.html#api)
- [`isaac_ros_ground_calibration`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_ground_calibration/index.html)
  - [Overview](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_ground_calibration/index.html#overview)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_ground_calibration/index.html#quickstart)
  - [Calibration Target](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_ground_calibration/index.html#calibration-target)
  - [Data Recording](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_ground_calibration/index.html#data-recording)
  - [Troubleshooting](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_ground_calibration/index.html#troubleshooting)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_ground_calibration/index.html#api)
- [`isaac_ros_hawk`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html)
  - [Overview](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html#overview)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html#quickstart)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html#api)
  - [Troubleshooting](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html#troubleshooting)
- [`isaac_ros_hesai`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hesai/index.html)
  - [Overview](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hesai/index.html#overview)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hesai/index.html#quickstart)
  - [Troubleshooting](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hesai/index.html#troubleshooting)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hesai/index.html#api)
- [`isaac_ros_imu_bmi088`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html)
  - [Overview](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html#overview)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html#quickstart)
  - [Troubleshooting](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html#troubleshooting)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html#api)
- [`isaac_ros_nova`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova/index.html)
  - [Overview](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova/index.html#overview)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova/index.html#quickstart)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova/index.html#api)
- [`isaac_ros_nova_recorder`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html)
  - [Overview](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html#overview)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html#quickstart)
  - [Try More Examples](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html#try-more-examples)
  - [Troubleshooting](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html#troubleshooting)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html#api)
- [`isaac_ros_owl`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_owl/index.html)
  - [Overview](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_owl/index.html#overview)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_owl/index.html#quickstart)
  - [Troubleshooting](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_owl/index.html#troubleshooting)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_owl/index.html#api)

## Supported Platforms [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html\#supported-platforms "Link to this heading")

The packages in this repository are only tested to work on the [Isaac Nova Orin](https://developer.nvidia.com/isaac/nova-orin) platform based on [Jetson AGX Orin](https://developer.nvidia.com/embedded/learn/jetson-agx-orin-devkit-user-guide/index.html)

Run [Nova Orin Init](https://nvidia-isaac-ros.github.io/nova/nova_init/index.html) to enable running configurations of the Nova Orin sensor suite correctly.

| Platform | Hardware | Software |
| --- | --- | --- |
| Jetson | [Jetson Orin](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/) | [JetPack 6.1 and 6.2](https://developer.nvidia.com/embedded/jetpack) |

## Updates [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html\#updates "Link to this heading")

| Date | Changes |
| --- | --- |
| 2024-12-10 | Added ground calibration support |
| 2024-09-26 | Update for Isaac ROS 3.1 |
| 2024-05-30 | Introduced Data Recorder and other utilities |
| 2023-10-18 | Initial release |

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_nova/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_nova/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_nova/index.html)