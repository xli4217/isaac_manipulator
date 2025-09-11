- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- Isaac ROS Nvblox
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_nvblox/index.rst.txt)

* * *

# Isaac ROS Nvblox [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/index.html\#repo-name "Link to this heading")

Nvblox ROS 2 integration for local 3D scene reconstruction and mapping.

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nvblox/isaac_sim_nvblox_humans.gif/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nvblox/isaac_sim_nvblox_humans.gif/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nvblox/isaac_sim_nvblox_humans.gif/)

## Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/index.html\#overview "Link to this heading")

[Isaac ROS Nvblox](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nvblox) contains ROS 2 packages for 3D reconstruction and cost
maps for navigation. `isaac_ros_nvblox` processes depth and pose to
reconstruct a 3D scene in real-time and outputs a 2D costmap for
[Nav2](https://github.com/ros-planning/navigation2). The costmap is
used in planning during navigation as a vision-based solution to avoid
obstacles.

`isaac_ros_nvblox` is designed to work with depth-cameras and/or 3D LiDAR.
The package uses GPU acceleration to compute a 3D reconstruction and 2D costmaps using
[nvblox](https://github.com/nvidia-isaac/nvblox), the underlying
framework-independent C++ library.

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox_nodegraph.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox_nodegraph.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox_nodegraph.png/)

Above is a typical graph that uses `isaac_ros_nvblox`.
Nvblox takes a depth image, a color image, and a pose as input, with
which it computes a 3D scene reconstruction on the GPU. In this graph
the pose is computed using `visual_slam`, or some other pose estimation
node. The reconstruction
is sliced into an output cost map which is provided through a cost map plugin
into [Nav2](https://github.com/ros-planning/navigation2).
An optional colorized 3D reconstruction is delivered into `rviz`
using the mesh visualization plugin. Nvblox streams mesh updates
to RViz to update the reconstruction in real-time as it is built.

`isaac_ros_nvblox` offers several modes of operation. In its default mode
the environment is assumed to be static. Two additional modes of operation are provided
to support mapping scenes which contain dynamic elements: people reconstruction, for
mapping scenes containing people, and dynamic reconstruction, for mapping
scenes containing more general dynamic objects.
The graph above shows `isaac_ros_nvblox` operating in people reconstruction
mode. The color image corresponding to the depth image is processed with `unet`, using
the PeopleSemSegNet DNN model to estimate a segmentation mask for
persons in the color image. Nvblox uses this mask to separate reconstructed persons into a
separate people-only part of the reconstruction. The [Technical Details](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/technical_details.html)
provide more information on these three types of mapping.

## Quickstarts [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/index.html\#quickstarts "Link to this heading")

- [Isaac ROS Nvblox](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox/index.html#quickstart)


## Performance [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/index.html\#performance "Link to this heading")

The following tables provides timings for various functions of
[nvblox core](https://github.com/nvidia-isaac/nvblox) on various platforms.

| Dataset | Voxel Size (m) | Component | x86\_64 w/ 3090 (Desktop) | x86\_64 w/ RTX A3000 (Laptop) | AGX Orin | Orin Nano |
| --- | --- | --- | --- | --- | --- | --- |
| Replica | 0.05 | TSDF | 0.5 ms | 0.3 ms | 0.8 ms | 2.1 ms |
| Color | 0.7 ms | 0.7 ms | 1.1 ms | 3.6 ms |
| Meshing | 0.7 ms | 1.3 ms | 2.3 ms | 13 ms |
| ESDF | 0.8 ms | 1.2 ms | 1.7 ms | 6.2 ms |
| Dynamics | 1.7 ms | 1.4 ms | 2.0 ms | N/A(\*) |
| Redwood | 0.05 | TSDF | 0.2 ms | 0.2 ms | 0.5 ms | 1.2 ms |
| Color | 0.5 ms | 0.5 ms | 0.8 ms | 2.6 ms |
| Meshing | 0.3 ms | 0.5 ms | 0.9 ms | 4.2 ms |
| ESDF | 0.8 ms | 1.0 ms | 1.5 ms | 5.1 ms |
| Dynamics | 1.0 ms | 0.7 ms | 1.2 ms | N/A(\*) |

(\*): Dynamics not supported on Jetson Nano.

## Packages [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/index.html\#packages "Link to this heading")

- [`isaac_ros_nvblox`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox/index.html)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox/index.html#quickstart)
    - [Set Up Development Environment](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox/index.html#set-up-development-environment)
    - [Download Quickstart Assets](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox/index.html#download-quickstart-assets)
    - [Set Up `isaac_ros_nvblox`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox/index.html#set-up-package-name)
    - [Run Example Launch File](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox/index.html#run-example-launch-file)
  - [Try More Examples](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox/index.html#try-more-examples)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox/index.html#api)
    - [ROS Parameters](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox/api/parameters.html)
    - [ROS Topics and Services](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox/api/topics_and_services.html)
  - [Troubleshooting](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox/index.html#troubleshooting)
    - [Isaac Sim Issues](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox/troubleshooting/troubleshooting_nvblox_isaac_sim.html)
    - [RealSense Issues](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox/troubleshooting/troubleshooting_nvblox_realsense.html)
    - [ROS Communication Issues](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox/troubleshooting/troubleshooting_nvblox_ros_communication.html)
- [`nvblox_examples_bringup`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/nvblox_examples_bringup/index.html)
- [`nvblox_image_padding`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/nvblox_image_padding/index.html)
- [`nvblox_isaac_sim`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/nvblox_isaac_sim/index.html)
- [`nvblox_msgs`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/nvblox_msgs/index.html)
- [`nvblox_nav2`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/nvblox_nav2/index.html)
- [`nvblox_performance_measurement`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/nvblox_performance_measurement/index.html)
- [`nvblox_ros`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/nvblox_ros/index.html)
- [`nvblox_ros_common`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/nvblox_ros_common/index.html)
- [`nvblox_rviz_plugin`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/nvblox_rviz_plugin/index.html)
- [`realsense_splitter`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/realsense_splitter/index.html)
- [`semantic_label_conversion`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/semantic_label_conversion/index.html)

## Camera System Requirements [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/index.html\#camera-system-requirements "Link to this heading")

The camera system providing data to this package must adhere to the specifications outlined below:

| Specification | Required Specification |
| --- | --- |
| Minimum target imager framerate | 30 Hertz |
| Maximum permissible jitter in imager framerate | \+/\- 2 milliseconds |
| Maximum expected offset between imagers within stereo camera | \+/\- 100 microseconds |
| Maximum expected offset between imagers across stereo camera | \+/\- 100 microseconds |

## Supported Platforms [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/index.html\#supported-platforms "Link to this heading")

This package is designed and tested to be compatible with ROS 2 Humble running on [Jetson](https://developer.nvidia.com/embedded-computing) or an x86\_64 system with an NVIDIA GPU.

Note

Versions of ROS 2 other than Humble are **not** supported. This package depends on specific ROS 2 implementation features that were introduced beginning with the Humble release. ROS 2 versions after Humble have not yet been tested.

| Platform | Hardware | Software | Notes |
| --- | --- | --- | --- |
| Jetson | [Jetson Orin](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/) | [JetPack 6.1 and 6.2](https://developer.nvidia.com/embedded/jetpack) | For best performance, ensure that [power settings](https://docs.nvidia.com/jetson/archives/r34.1/DeveloperGuide/text/SD/PlatformPowerAndPerformance.html) are configured appropriately.<br>Jetson Orin Nano 4GB may not have enough memory to run many of the Isaac ROS packages and is not recommended. |
| x86\_64 | `Ampere` or higher NVIDIA GPU Architecture with 8 GB RAM or higher | [Ubuntu 22.04+](https://releases.ubuntu.com/22.04/) | [CUDA 12.6+](https://developer.nvidia.com/cuda-downloads) |

## Docker [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/index.html\#docker "Link to this heading")

To simplify development, we strongly recommend leveraging the Isaac ROS Dev Docker images by following [these steps](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html).
This streamlines your development environment setup with the correct versions of dependencies on both Jetson and x86\_64 platforms.

Note

All Isaac ROS Quickstarts, tutorials, and examples have been designed with the Isaac ROS Docker images as a prerequisite.

## Customize your Dev Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/index.html\#customize-your-dev-environment "Link to this heading")

To customize your development environment, reference [this guide](https://nvidia-isaac-ros.github.io/concepts/docker_devenv/index.html#development-environment).

## Updates [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/index.html\#updates "Link to this heading")

| Date | Changes |
| --- | --- |
| 2024-12-10 | Optimized performance for always-on dynamic obstacle detection and 1 cm voxels |
| 2024-09-26 | Update for ZED compatibility |
| 2024-05-30 | Multi-camera support, NITROS integration and performance improvements. |
| 2023-10-18 | General dynamic reconstruction. |
| 2023-04-05 | People reconstruction and new weighting functions. |
| 2022-12-10 | Updated documentation. |
| 2022-10-19 | Updated OSS licensing. |
| 2022-08-31 | Update to be compatible with JetPack 5.0.2. Serialization of Nvblox maps to file. Support for 3D LIDAR input and performance improvements. |
| 2022-06-30 | Support for ROS 2 Humble and miscellaneous bug fixes. |
| 2022-03-21 | Initial version. |

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_nvblox/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_nvblox/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_nvblox/index.html)