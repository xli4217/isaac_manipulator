- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- Isaac ROS Benchmark
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_benchmark/index.rst.txt)

* * *

# Isaac ROS Benchmark [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/index.html\#repo-name "Link to this heading")

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_benchmark/r2b_turtlebot_takeoff.gif/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_benchmark/r2b_turtlebot_takeoff.gif/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_benchmark/r2b_turtlebot_takeoff.gif/)

## Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/index.html\#overview "Link to this heading")

[Isaac ROS Benchmark](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_benchmark) builds upon the
[ros2\_benchmark](https://github.com/NVIDIA-ISAAC-ROS/ros2_benchmark)
to provide configurations to benchmark Isaac ROS graphs. Performance
results that measure Isaac ROS for throughput, latency, and utilization
enable robotics developers to make informed decisions when designing
real-time robotics applications. The Isaac ROS performance results can
be independently verified, as the method, configuration, and data input
used for benchmarking are provided.

The `ros2_benchmark` playback node plug-in, for type adaptation and
negotiation, is provided for
[NITROS](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros), which
optimizes the performance of message transport costs through
[RCL](https://github.com/ros2/rclcpp) with GPU accelerated graphs of
nodes.

The datasets for benchmarking are explicitly not downloaded by default.
To pull down the standardized benchmark datasets, refer to the
[ros2\_benchmark Dataset](https://github.com/NVIDIA-ISAAC-ROS/ros2_benchmark#datasets)
section.

## Quickstarts [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/index.html\#quickstarts "Link to this heading")

- [Isaac ROS Benchmark](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/isaac_ros_benchmark/index.html#quickstart)


## Benchmarking [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/index.html\#benchmarking "Link to this heading")

Please consult [here](https://nvidia-isaac-ros.github.io/concepts/benchmarking/index.html) for more details about using Isaac ROS Benchmark.

## Packages [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/index.html\#packages "Link to this heading")

- [`isaac_ros_benchmark`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/isaac_ros_benchmark/index.html)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/isaac_ros_benchmark/index.html#quickstart)
  - [Troubleshooting](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/isaac_ros_benchmark/index.html#troubleshooting)
- [`isaac_ros_moveit_benchmark`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/benchmarks/isaac_ros_moveit_benchmark/index.html)
  - [Overview](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/benchmarks/isaac_ros_moveit_benchmark/index.html#overview)
  - [Setup](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/benchmarks/isaac_ros_moveit_benchmark/index.html#setup)
  - [Running the Benchmark](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/benchmarks/isaac_ros_moveit_benchmark/index.html#running-the-benchmark)
  - [Available Metrics](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/benchmarks/isaac_ros_moveit_benchmark/index.html#available-metrics)

## Supported Platforms [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/index.html\#supported-platforms "Link to this heading")

This package is designed and tested to be compatible with ROS 2 Humble running on [Jetson](https://developer.nvidia.com/embedded-computing) or an x86\_64 system with an NVIDIA GPU.

Note

Versions of ROS 2 other than Humble are **not** supported. This package depends on specific ROS 2 implementation features that were introduced beginning with the Humble release. ROS 2 versions after Humble have not yet been tested.

| Platform | Hardware | Software | Notes |
| --- | --- | --- | --- |
| Jetson | [Jetson Orin](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/) | [JetPack 6.1 and 6.2](https://developer.nvidia.com/embedded/jetpack) | For best performance, ensure that [power settings](https://docs.nvidia.com/jetson/archives/r34.1/DeveloperGuide/text/SD/PlatformPowerAndPerformance.html) are configured appropriately.<br>Jetson Orin Nano 4GB may not have enough memory to run many of the Isaac ROS packages and is not recommended. |
| x86\_64 | `Ampere` or higher NVIDIA GPU Architecture with 8 GB RAM or higher | [Ubuntu 22.04+](https://releases.ubuntu.com/22.04/) | [CUDA 12.6+](https://developer.nvidia.com/cuda-downloads) |

## Docker [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/index.html\#docker "Link to this heading")

To simplify development, we strongly recommend leveraging the Isaac ROS Dev Docker images by following [these steps](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html).
This streamlines your development environment setup with the correct versions of dependencies on both Jetson and x86\_64 platforms.

Note

All Isaac ROS Quickstarts, tutorials, and examples have been designed with the Isaac ROS Docker images as a prerequisite.

## Customize your Dev Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/index.html\#customize-your-dev-environment "Link to this heading")

To customize your development environment, reference [this guide](https://nvidia-isaac-ros.github.io/concepts/docker_devenv/index.html#development-environment).

## Updates [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/index.html\#updates "Link to this heading")

| Date | Changes |
| --- | --- |
| 2024-12-10 | Added new benchmarks |
| 2024-09-26 | Updated for Isaac ROS 3.1 |
| 2024-05-30 | Restructure benchmark scripts into packages |
| 2023-10-18 | Added new benchmarks |
| 2023-04-05 | Initial release |

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_benchmark/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_benchmark/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_benchmark/index.html)