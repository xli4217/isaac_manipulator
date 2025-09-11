- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Reference Workflows](https://nvidia-isaac-ros.github.io/reference_workflows/index.html)
- [Isaac Perceptor](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/index.html)
- Technical Details
- [View page source](https://nvidia-isaac-ros.github.io/_sources/reference_workflows/isaac_perceptor/technical_details.rst.txt)

* * *

# Technical Details [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/technical_details.html\#technical-details "Link to this heading")

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_perceptor/perceptor_diagram.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_perceptor/perceptor_diagram.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_perceptor/perceptor_diagram.png/)

A high-level description of the data-flow in the Isaac Perceptor system. Images flow from
either the Hawk cameras on the Nova Orin Developer Kit, a (compressed) rosbag,
or Isaac Sim into a series of
GPU-accelerated modules which work together to build a 3D reconstruction of the world.
The reconstruction is converted to 2D/3D costmap which can be used by a downstream
application, for example a path-planner. [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/technical_details.html#id1 "Link to this image")

Isaac Perceptor leverages multiple Isaac ROS modules:

- [Isaac ROS Nova](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html) for time-synchronized multi-cam data.

- [Isaac ROS Visual SLAM](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_visual_slam/isaac_ros_visual_slam/index.html) for GPU-accelerated camera-based odometry.

- [Isaac ROS Depth Estimation](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html) for learning-based stereo-depth estimation.

- [Isaac ROS Nvblox](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/index.html) for GPU-accelerated local 3D reconstruction.

- [Isaac ROS Image Pipeline](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/index.html) for GPU-accelerated image processing.


Isaac Perceptor provides a 3D map of the world around the robot
using the Nova sensor suite, as well as providing access to the raw sensor output.

The input to the Isaac Perceptor system is several time-synchronized image streams from the
[HAWK stereo cameras](https://leopardimaging.com/leopard-imaging-hawk-stereo-camera/)
which are part of the Nova sensor suite on the Nova Orin Developer Kit,
from recorded data stored in a (compressed) rosbag, or from Isaac Sim.
The NITROS images coming from these cameras are passed through the Hawk processing pipeline
which is a combination of GPU-accelerated image operations, notably rectification and
undistortion.
The left and right side-camera image pairs are also throttled in order to reduce GPU load.

Several Isaac ROS components are involved in building a 3D map of the world.
The rectified stereo image streams are passed through
[ESS Depth Estimation](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html)
in order to produce depth-image streams for the front, left, and right cameras.
Concurrently, the stereo image streams are passed to
[Isaac ROS Visual SLAM](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_visual_slam/isaac_ros_visual_slam/index.html)
in order to estimate the motion of the system through the world.
The depth images plus the visual SLAM poses are passed to nvblox to compute a voxelized
map of the world.

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_perceptor/perceptor_warehouse.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_perceptor/perceptor_warehouse.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_perceptor/perceptor_warehouse.png/)

This map is then converted to a distance map for downstream applications, and
a visualization mesh transmitting over the Foxglove bridge to a base station for
visualization.

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/reference_workflows/isaac_perceptor/technical_details.html)[latest](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/technical_details.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/reference_workflows/isaac_perceptor/technical_details.html)