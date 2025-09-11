- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS NITROS](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/index.html)
- [Isaac ROS NITROS Bridge](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_bridge/index.html)
- Tutorial for NITROS Bridge with Isaac Sim
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/nitros_bridge/tutorial_isaac_sim.rst.txt)

* * *

# Tutorial for NITROS Bridge with Isaac Sim [](https://nvidia-isaac-ros.github.io/concepts/nitros_bridge/tutorial_isaac_sim.html\#tutorial-for-nitros-bridge-with-isaac-sim "Link to this heading")

## Overview [](https://nvidia-isaac-ros.github.io/concepts/nitros_bridge/tutorial_isaac_sim.html\#overview "Link to this heading")

This tutorial will walk you through how to use [isaac\_ros\_nitros\_bridge](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_bridge) to move images on the GPU while avoiding CPU memory copies from Isaac Sim.

## Tutorial Walkthrough [](https://nvidia-isaac-ros.github.io/concepts/nitros_bridge/tutorial_isaac_sim.html\#tutorial-walkthrough "Link to this heading")

1. Clone `isaac_ros_common` repository under a new workspace:


> ```
> mkdir -p ~/isaac_sim_workspaces && \
> cd ~/isaac_sim_workspaces && \
> git clone -b release-3.2 https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common.git isaac_ros_common
>
> ```
>
> Copy to clipboard

2. Install and launch Isaac Sim under the Isaac Sim workspace following the steps in the [Isaac ROS Isaac Sim Setup Guide](https://nvidia-isaac-ros.github.io/getting_started/isaac_sim/index.html)

3. Press **Play** to start publishing data from the Isaac Sim.


> Note
>
> You should be able to see, using the command `ros2 topic list`, two topics under the same main name, one for the standard ROS 2 [sensor\_msgs/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) (e.g.: `/front_stereo_camera/left/image_rect_color`) and another for [isaac\_ros\_nitros\_bridge\_interfaces/NitrosBridgeImage](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common/blob/main/isaac_ros_nitros_bridge_interfaces/msg/NitrosBridgeImage.msg) (e.g.: `/front_stereo_camera/left/image_rect_color/nitros_bridge`).

4. Complete the step 3 & 4 from `Set Up Development Environment` section and `Build from Source` section following the `isaac_ros_nitros_bridge_ros2` quickstart [here](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_bridge/index.html#quickstart).

5. Inside the container, change the `ptrace` scope to be able to run the NITROS Bridge:


> ```
> echo 0 | sudo tee /proc/sys/kernel/yama/ptrace_scope
>
> ```
>
> Copy to clipboard

6. Inside the container, edit the [isaac\_ros\_nitros\_bridge\_image\_converter.launch.py](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_bridge/isaac_ros_nitros_bridge_ros2/launch/isaac_ros_nitros_bridge_image_converter.launch.py?ref_type=heads#L39) ROS 2 launch file to remap the `ros2_input_bridge_image` to the Isaac Sim’s NITROS Bridge topic name (e.g.: `/front_stereo_camera/left/image_rect_color/nitros_bridge`). Then, run the following command to start the NITROS Bridge.


> ```
> cd /workspaces/isaac_ros-dev && \
>   source install/setup.bash && \
>   ros2 launch isaac_ros_nitros_bridge_ros2 isaac_ros_nitros_bridge_image_converter.launch.py pub_image_name:=nitros_pub sub_image_name:=nitros_sub
>
> ```
>
> Copy to clipboard

7. The converted NITROS image will be available under the `nitros_pub` topic.


Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/nitros_bridge/tutorial_isaac_sim.html)[latest](https://nvidia-isaac-ros.github.io/concepts/nitros_bridge/tutorial_isaac_sim.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/nitros_bridge/tutorial_isaac_sim.html)