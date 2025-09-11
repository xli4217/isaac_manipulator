- [Home](https://nvidia-isaac-ros.github.io/index.html)
- Getting Started
- [View page source](https://nvidia-isaac-ros.github.io/_sources/getting_started/index.rst.txt)

* * *

# Getting Started [](https://nvidia-isaac-ros.github.io/getting_started/index.html\#getting-started "Link to this heading")

The Isaac ROS suite has been developed and released by NVIDIA to
leverage the power of NVIDIA acceleration on NVIDIA Jetson and
discrete GPUs for standard robotics applications.

Isaac ROS uses standard ROS interfaces on input and output topics, making
it extremely easy to use as a drop-in replacement for commonly-used, CPU-based ROS
implementations familiar to robotics developers.

## System Requirements [](https://nvidia-isaac-ros.github.io/getting_started/index.html\#system-requirements "Link to this heading")

| Platform | Hardware | Software | Notes |
| --- | --- | --- | --- |
| Jetson | [Jetson Orin](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/) | [JetPack 6.1 and 6.2](https://developer.nvidia.com/embedded/jetpack) | For best performance, ensure that [power settings](https://docs.nvidia.com/jetson/archives/r34.1/DeveloperGuide/text/SD/PlatformPowerAndPerformance.html) are configured appropriately.<br>Jetson Orin Nano 4GB may not have enough memory to run many of the Isaac ROS packages and is not recommended. |
| x86\_64 | `Ampere` or higher NVIDIA GPU Architecture with 8 GB RAM or higher | [Ubuntu 22.04+](https://releases.ubuntu.com/22.04/) | [CUDA 12.6+](https://developer.nvidia.com/cuda-downloads) |

### ROS Support [](https://nvidia-isaac-ros.github.io/getting_started/index.html\#ros-support "Link to this heading")

All Isaac ROS packages are designed and tested to be compatible with [ROS 2\\
Humble](https://docs.ros.org/en/humble/index.html).

If your application is build with [ROS 1 Noetic](https://wiki.ros.org/noetic), you can integrate Isaac ROS packages with
accelerated performance using the [Isaac ROS NITROS Bridge](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros_bridge/index.html).

Warning

ROS 1 Noetic is not supported in the same OS environment as ROS 2 Humble.

Warning

Isaac ROS packages have **ONLY** been tested against ROS 2 Humble. Other ROS 2
versions are **NOT YET** supported.

Follow steps below to set up a ROS 2 developer environment with Isaac ROS Dev Docker images. Alternatively, you can also install pre-built ROS 2 Humble packages through the [Isaac Apt Repository](https://nvidia-isaac-ros.github.io/getting_started/isaac_apt_repository.html).

Note

We **strongly** recommend that you set up your [developer environment](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html) with Isaac ROS Dev Docker images.
This will streamline your development environment setup with the correct versions of dependencies on both Jetson and x86\_64 platforms.
Working within the Isaac ROS Dev Docker containers will setup ROS and automatically configure the [Isaac Apt Repository](https://nvidia-isaac-ros.github.io/getting_started/isaac_apt_repository.html).

## Setup [](https://nvidia-isaac-ros.github.io/getting_started/index.html\#setup "Link to this heading")

1. Set up the hardware to run Isaac ROS:


- [Compute Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/index.html)

- [Sensors Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/index.html)


Note

If you do not have access to physical sensors but still want to try out Isaac ROS packages, you can check out
our [Isaac Sim guide](https://nvidia-isaac-ros.github.io/getting_started/isaac_sim/index.html) for setting up the simulation environment.

2. Set up the developer environment for Isaac ROS:


- [Developer Environment Setup](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html)


Once you are set up, check out the [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html) to start running
Isaac ROS packages!

## Isaac Sim Tutorials [](https://nvidia-isaac-ros.github.io/getting_started/index.html\#isaac-sim-tutorials "Link to this heading")

Isaac ROS packages are also designed to work with [Isaac Sim](https://developer.nvidia.com/isaac-sim), which is NVIDIA’s
robotics simulation platform powered by Omniverse. A number of tutorials are provided to learn how to use Isaac Sim
with Isaac ROS.

Note

Last validated with [Isaac Sim 4.5.0](https://docs.omniverse.nvidia.com/isaacsim/latest/release_notes.html#id1) and [Isaac Sim 4.2.0](https://docs.omniverse.nvidia.com/isaacsim/latest/archived_release_notes.html#)

- [Tutorial for AprilTag Detection with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/fiducials/apriltag/tutorial_isaac_sim.html)

- [Tutorial for Visual SLAM with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_isaac_sim.html)

- [Tutorial for Nvblox with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.html)

- [Tutorial for DNN Object Detection with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/object_detection/detectnet/tutorial_isaac_sim.html)

- [Tutorial for SGM Stereo Disparity with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/sgm/tutorial_isaac_sim.html)

- [Tutorial for DNN Stereo Depth Estimation with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/ess/tutorial_isaac_sim.html)

- [Tutorial for Bi3D with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/bi3d/tutorial_isaac_sim.html)

- [Tutorial for Freespace Segmentation with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/bi3d_freespace_segmentation/tutorial_isaac_sim.html)

- [Tutorial for Occupancy Grid Localizer with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/localization/lidar/tutorial_isaac_sim.html)

- [Tutorial for DNN Image Segmentation with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/segmentation/unet/tutorial_isaac_sim.html)

- [Tutorial for Isaac ROS Mission Client](https://nvidia-isaac-ros.github.io/concepts/missions/isaac_ros_mission_client.html)

- [Tutorial for RT-DETR with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/object_detection/rtdetr/tutorial_isaac_sim.html)

- [Tutorial for FoundationPose with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/pose_estimation/foundationpose/tutorial_isaac_sim.html)

- [Tutorial for NITROS Bridge with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/nitros_bridge/tutorial_isaac_sim.html)

- [Tutorial for cuMotion MoveIt Plugin with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/manipulation/cumotion_moveit/tutorial_isaac_sim.html)

- [Tutorial for Isaac Manipulator Reference Workflows with Isaac Sim](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_isaac_sim.html)


Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/getting_started/index.html)[latest](https://nvidia-isaac-ros.github.io/getting_started/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/getting_started/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/getting_started/index.html)