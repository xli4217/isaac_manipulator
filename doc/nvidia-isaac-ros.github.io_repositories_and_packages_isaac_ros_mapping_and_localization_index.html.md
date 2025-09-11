- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- Isaac ROS Mapping And Localization
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_mapping_and_localization/index.rst.txt)

* * *

# Isaac ROS Mapping And Localization [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/index.html\#repo-name "Link to this heading")

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_mapping_and_localization/localize_in_hubble_lab.gif/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_mapping_and_localization/localize_in_hubble_lab.gif/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_mapping_and_localization/localize_in_hubble_lab.gif/)

## Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/index.html\#overview "Link to this heading")

[Isaac ROS Mapping And Localization](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_mapping_and_localization) contains a list of ROS 2 packages for mapping and localization.

- [Isaac ROS Visual Global Localization](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_ros_visual_global_localization/index.html) provides a robust and accurate visual global localization solution as a ROS 2 package for robots. Built on the cuVGL library, this package uses stereo cameras to estimate a device’s or robot’s position in an environment without prior knowledge of its location.

- [Isaac Mapping ROS](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/index.html) includes the core implementations of cuVGL and tools for converting rosbags to cuVGL format.


To learn more about cuVGL, refer to the
[Visual Global Localization](https://nvidia-isaac-ros.github.io/concepts/visual_global_localization/index.html) documentation.

## Quickstarts [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/index.html\#quickstarts "Link to this heading")

- [Isaac ROS Visual Global Localization](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_ros_visual_global_localization/index.html#quickstart)

- [Isaac Mapping ROS](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/index.html#installation)


## Accuracy [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/index.html\#accuracy "Link to this heading")

Please see the [Visual Global Localization Metrics](https://nvidia-isaac-ros.github.io/concepts/visual_global_localization/index.html#metrics-of-cuvgl).

## Packages [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/index.html\#packages "Link to this heading")

- [Isaac Mapping ROS](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/index.html)
  - [Overview](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/index.html#overview)
  - [Installation](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/index.html#installation)
  - [Tools](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/index.html#tools)
- [Isaac Mapping](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/isaac_mapping/index.html)
  - [Overview](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/isaac_mapping/index.html#overview)
  - [cuVGL](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/isaac_mapping/index.html#cuvgl)
  - [Tools](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/isaac_mapping/index.html#tools)
- [Isaac ROS Visual Global Localization](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_ros_visual_global_localization/index.html)
  - [Overview](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_ros_visual_global_localization/index.html#overview)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_ros_visual_global_localization/index.html#quickstart)
  - [Try More Examples](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_ros_visual_global_localization/index.html#try-more-examples)
  - [Coordinate Frames](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_ros_visual_global_localization/index.html#coordinate-frames)
  - [Troubleshooting](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_ros_visual_global_localization/index.html#troubleshooting)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_ros_visual_global_localization/index.html#api)

## Camera System Requirements [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/index.html\#camera-system-requirements "Link to this heading")

Please see the [Camera System Requirements](https://nvidia-isaac-ros.github.io/concepts/visual_global_localization/index.html#camera-system-requirements).

## Supported Platforms [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/index.html\#supported-platforms "Link to this heading")

This package is designed and tested to be compatible with ROS 2 Humble running on [Jetson](https://developer.nvidia.com/embedded-computing) or an x86\_64 system with an NVIDIA GPU.

Note

Versions of ROS 2 other than Humble are **not** supported. This package depends on specific ROS 2 implementation features that were introduced beginning with the Humble release. ROS 2 versions after Humble have not yet been tested.

| Platform | Hardware | Software | Notes |
| --- | --- | --- | --- |
| Jetson | [Jetson Orin](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/) | [JetPack 6.1 and 6.2](https://developer.nvidia.com/embedded/jetpack) | For best performance, ensure that [power settings](https://docs.nvidia.com/jetson/archives/r34.1/DeveloperGuide/text/SD/PlatformPowerAndPerformance.html) are configured appropriately.<br>Jetson Orin Nano 4GB may not have enough memory to run many of the Isaac ROS packages and is not recommended. |
| x86\_64 | `Ampere` or higher NVIDIA GPU Architecture with 8 GB RAM or higher | [Ubuntu 22.04+](https://releases.ubuntu.com/22.04/) | [CUDA 12.6+](https://developer.nvidia.com/cuda-downloads) |

## Docker [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/index.html\#docker "Link to this heading")

To simplify development, we strongly recommend leveraging the Isaac ROS Dev Docker images by following [these steps](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html).
This streamlines your development environment setup with the correct versions of dependencies on both Jetson and x86\_64 platforms.

Note

All Isaac ROS Quickstarts, tutorials, and examples have been designed with the Isaac ROS Docker images as a prerequisite.

## Customize your Dev Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/index.html\#customize-your-dev-environment "Link to this heading")

To customize your development environment, reference [this guide](https://nvidia-isaac-ros.github.io/concepts/docker_devenv/index.html#development-environment).

## Commercial Support & Source Access [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/index.html\#commercial-support-source-access "Link to this heading")

For commercial support and access to source code, please [contact\\
NVIDIA](https://developer.nvidia.com/isaac-platform-contact-us-form).

## Updates [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/index.html\#updates "Link to this heading")

| Date | Changes |
| --- | --- |
| 2024-12-10 | Initial release |

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_mapping_and_localization/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/index.html)