- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- Isaac ROS NITROS
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_nitros/index.rst.txt)

* * *

# Isaac ROS NITROS [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/index.html\#repo-name "Link to this heading")

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nitros/image5-1.gif/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nitros/image5-1.gif/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nitros/image5-1.gif/)

## Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/index.html\#overview "Link to this heading")

[Isaac ROS NITROS](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros) contains NVIDIA’s implementation
of type adaptation and negotiation in ROS 2. To learn more about NITROS, see [here](https://nvidia-isaac-ros.github.io/concepts/nitros/index.html).

Isaac ROS NITROS is composed of a number of individual packages, each with either a functional or structural purpose:

`isaac_ros_gxf`:

This package serves as a container for precompiled GXF extensions used by other Isaac ROS packages.
While a number of GXF extensions used by Isaac ROS are provided with source, the extensions contained in `isaac_ros_gxf` are license constrained and are thus shipped as `.so` binaries.

`isaac_ros_managed_nitros`:

This package contains the wrapper classes that enable developers to add NITROS-compatible publishers and subscribers to third-party CUDA-based ROS nodes.
For more information about CUDA with NITROS, see [here](https://nvidia-isaac-ros.github.io/concepts/nitros/cuda_with_nitros.html).

`isaac_ros_nitros`:

This package contains the base `NitrosNode` class and associated core utilities that serve as the foundation for all NITROS-based ROS nodes.

`isaac_ros_nitros_interfaces`:

This package contains the definitions of the custom ROS 2 interfaces that facilitate type negotiation between NITROS nodes.

`isaac_ros_nitros_topic_tools`:

This folder contains a NITROS based implementation of some of the nodes in the [topic\_tools package](https://github.com/ros-tooling/topic_tools).

`isaac_ros_nitros_type`:

This folder contains a number of packages, each defining a specific NITROS type and the associated type adaptation logic to convert to and from a standard ROS type.

`isaac_ros_pynitros`:

This folder contains the implementation of Python NITROS.

## Quickstarts [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/index.html\#quickstarts "Link to this heading")

- [Isaac ROS NITROS](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros/index.html#quickstart)


## Packages [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/index.html\#packages "Link to this heading")

- [`isaac_ros_gxf`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_gxf/index.html)
- [`isaac_ros_managed_nitros`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_managed_nitros/index.html)
- [`isaac_ros_nitros`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros/index.html)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros/index.html#quickstart)
- [Isaac ROS NITROS Bridge](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_bridge/index.html)
  - [Overview](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_bridge/index.html#overview)
  - [Performance](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_bridge/index.html#performance)
  - [Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_bridge/index.html#packages)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_bridge/index.html#quickstart)
  - [Try Another Example](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_bridge/index.html#try-another-example)
  - [Troubleshooting](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_bridge/index.html#troubleshooting)
  - [Updates](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_bridge/index.html#updates)
- [`isaac_ros_nitros_bridge_ros1`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_bridge/isaac_ros_nitros_bridge_ros1/index.html)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_bridge/isaac_ros_nitros_bridge_ros1/index.html#api)
- [`isaac_ros_nitros_bridge_ros2`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_bridge/isaac_ros_nitros_bridge_ros2/index.html)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_bridge/isaac_ros_nitros_bridge_ros2/index.html#api)
- [`isaac_ros_nitros_interfaces`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_interfaces/index.html)
- [`isaac_ros_nitros_topic_tools`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_topic_tools/index.html)
  - [NitrosCameraDrop Node](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_topic_tools/index.html#nitroscameradrop-node)
- [`isaac_ros_nitros_type`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_type/index.html)
- [`isaac_ros_pynitros`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html)
  - [Creating PyNITROS-Accelerated Nodes](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#creating-pynitros-accelerated-nodes)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#quickstart)
  - [Try More Examples](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#try-more-examples)
  - [API Reference](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#api-reference)

## Supported Platforms [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/index.html\#supported-platforms "Link to this heading")

This package is designed and tested to be compatible with ROS 2 Humble running on [Jetson](https://developer.nvidia.com/embedded-computing) or an x86\_64 system with an NVIDIA GPU.

Note

Versions of ROS 2 other than Humble are **not** supported. This package depends on specific ROS 2 implementation features that were introduced beginning with the Humble release. ROS 2 versions after Humble have not yet been tested.

| Platform | Hardware | Software | Notes |
| --- | --- | --- | --- |
| Jetson | [Jetson Orin](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/) | [JetPack 6.1 and 6.2](https://developer.nvidia.com/embedded/jetpack) | For best performance, ensure that [power settings](https://docs.nvidia.com/jetson/archives/r34.1/DeveloperGuide/text/SD/PlatformPowerAndPerformance.html) are configured appropriately.<br>Jetson Orin Nano 4GB may not have enough memory to run many of the Isaac ROS packages and is not recommended. |
| x86\_64 | `Ampere` or higher NVIDIA GPU Architecture with 8 GB RAM or higher | [Ubuntu 22.04+](https://releases.ubuntu.com/22.04/) | [CUDA 12.6+](https://developer.nvidia.com/cuda-downloads) |

## Docker [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/index.html\#docker "Link to this heading")

To simplify development, we strongly recommend leveraging the Isaac ROS Dev Docker images by following [these steps](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html).
This streamlines your development environment setup with the correct versions of dependencies on both Jetson and x86\_64 platforms.

Note

All Isaac ROS Quickstarts, tutorials, and examples have been designed with the Isaac ROS Docker images as a prerequisite.

## Customize your Dev Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/index.html\#customize-your-dev-environment "Link to this heading")

To customize your development environment, reference [this guide](https://nvidia-isaac-ros.github.io/concepts/docker_devenv/index.html#development-environment).

## Updates [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/index.html\#updates "Link to this heading")

| Date | Changes |
| --- | --- |
| 2024-12-10 | Update to be compatible with JetPack 6.1 |
| 2024-09-26 | Update for Isaac ROS 3.1 |
| 2024-05-30 | Add support for NITROS message filters |
| 2023-10-18 | Introducing CUDA with NITROS |
| 2023-05-25 | CPU usage optimizations |
| 2023-04-05 | Update to be compatible with JetPack 5.1.1 |
| 2022-10-19 | Minor updates and bugfixes |
| 2022-08-31 | Update to be compatible with JetPack 5.0.2 |
| 2022-06-30 | Initial release |

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_nitros/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_nitros/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_nitros/index.html)