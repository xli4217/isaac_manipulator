- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Concepts](https://nvidia-isaac-ros.github.io/concepts/index.html)
- [Visual SLAM](https://nvidia-isaac-ros.github.io/concepts/visual_slam/index.html)
- [cuVSLAM](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/index.html)
- Tutorial for Visual SLAM Using a HAWK Camera
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/visual_slam/cuvslam/tutorial_hawk.rst.txt)

* * *

# Tutorial for Visual SLAM Using a HAWK Camera [](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_hawk.html\#tutorial-for-visual-slam-using-a-hawk-camera "Link to this heading")

## Overview [](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_hawk.html\#overview "Link to this heading")

This tutorial walks you through setting up
[Isaac ROS Visual SLAM](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_visual_slam) with
a [Hawk camera](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_argus_camera).

## Tutorial Walkthrough - VSLAM Execution [](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_hawk.html\#tutorial-walkthrough-vslam-execution "Link to this heading")

1. Complete the [Hawk setup tutorial](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/hawk_setup.html).

2. Complete the VSLAM quickstart [here](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_visual_slam/isaac_ros_visual_slam/index.html#quickstart).

3. \[Terminal 1\] Inside the container, install the dependencies:


> ```
> sudo apt-get update
>
> ```
>
> Copy to clipboard






```
sudo apt-get install -y \
      ros-humble-isaac-ros-correlated-timestamp-driver \
      ros-humble-isaac-ros-hawk \
      ros-humble-isaac-ros-image-proc \
      ros-humble-isaac-ros-imu-bmi088

```

Copy to clipboard

4. \[Terminal 1\] Run the launch file inside the container and wait for 5 seconds:





```
ros2 launch isaac_ros_visual_slam isaac_ros_visual_slam_hawk.launch.py

```

Copy to clipboard

5. \[Terminal 2\] In a second terminal check that the VSLAM node is publishing the
odometry messages.

Attach another terminal to the running container for issuing other ROS 2 commands.





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common
./scripts/run_dev.sh

```

Copy to clipboard



Verify that you are getting the output from the `visual_slam` node at the
same rate as the input.





```
ros2 topic hz /visual_slam/tracking/odometry --window 20

```

Copy to clipboard


Typically, if you are running `visual_slam` on Jetson, it is
recommended that you **NOT** evaluate with live visualization.

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/visual_slam/cuvslam/tutorial_hawk.html)[latest](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_hawk.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/visual_slam/cuvslam/tutorial_hawk.html)