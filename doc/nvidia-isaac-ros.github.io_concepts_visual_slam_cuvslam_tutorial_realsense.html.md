- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Concepts](https://nvidia-isaac-ros.github.io/concepts/index.html)
- [Visual SLAM](https://nvidia-isaac-ros.github.io/concepts/visual_slam/index.html)
- [cuVSLAM](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/index.html)
- Tutorial for Visual SLAM Using a RealSense Camera with Integrated IMU
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/visual_slam/cuvslam/tutorial_realsense.rst.txt)

* * *

# Tutorial for Visual SLAM Using a RealSense Camera with Integrated IMU [](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_realsense.html\#tutorial-for-visual-slam-using-a-realsense-camera-with-integrated-imu "Link to this heading")

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/realsense.gif/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/realsense.gif/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/realsense.gif/)

## Overview [](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_realsense.html\#overview "Link to this heading")

This tutorial walks you through setting up
[Isaac ROS Visual SLAM](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_visual_slam) with
a [Realsense camera](https://www.intel.com/content/www/us/en/architecture-and-technology/realsense-overview.html).

Note

The [launch file](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_visual_slam/blob/main/isaac_ros_visual_slam/launch/isaac_ros_visual_slam_realsense.launch.py)
provided in this tutorial is designed for a RealSense camera with
integrated IMU. If you want to run this tutorial with a RealSense
camera without an IMU (like RealSense D435), then change the
`enable_imu_fusion` parameter in the launch file to `False`.

Note

This tutorial requires a compatible RealSense camera from
the list of available
[cameras](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/realsense_setup.html#camera-compatibility).

## Tutorial Walkthrough - VSLAM Execution [](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_realsense.html\#tutorial-walkthrough-vslam-execution "Link to this heading")

1. Complete the [RealSense setup tutorial](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/realsense_setup.html).

2. Complete the quickstart [here](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_visual_slam/isaac_ros_visual_slam/index.html#quickstart).

3. Follow the
[IMU page](https://github.com/ethz-asl/kalibr/wiki/IMU-Noise-Model#how-to-obtain-the-parameters-for-your-imu)
to obtain the [IMU Noise\\
Model](https://github.com/ethz-asl/kalibr/wiki/IMU-Noise-Model)
parameters. Parameters can be obtained through the datasheet for the IMU
or from a ROS package such as
[this](https://github.com/CruxDevStuff/allan_ros2).

4. \[Terminal 1\] Run `realsense-camera` node and `visual_slam` node.

Make sure you have your RealSense camera attached to the system, and
then start the Isaac ROS container.





```
    cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
./scripts/run_dev.sh

```

Copy to clipboard

5. \[Terminal 1\] Inside the container, build and source the workspace:





```
cd /workspaces/isaac_ros-dev && \
     colcon build --symlink-install && \
     source install/setup.bash

```

Copy to clipboard

6. \[Terminal 1\] Run the launch file, which launches the example and waits
for 5 seconds:





```
ros2 launch isaac_ros_visual_slam isaac_ros_visual_slam_realsense.launch.py

```

Copy to clipboard

7. \[Terminal 2\] Attach a second terminal to check the operation.

Attach another terminal to the running container for issuing other ROS commands.





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
      ./scripts/run_dev.sh

```

Copy to clipboard



Verify that you can see all the ROS topics expected.





```
ros2 topic list

```

Copy to clipboard




> Output example:
>
> ```
> /camera/accel/imu_info
> /camera/accel/metadata
> /camera/accel/sample
> /camera/extrinsics/depth_to_accel
> /camera/extrinsics/depth_to_gyro
> /camera/extrinsics/depth_to_infra1
> /camera/extrinsics/depth_to_infra2
> /camera/gyro/imu_info
> /camera/gyro/metadata
> /camera/gyro/sample
> /camera/imu
> /camera/infra1/camera_info
> /camera/infra1/image_rect_raw
> /camera/infra1/image_rect_raw/compressed
> /camera/infra1/image_rect_raw/compressedDepth
> /camera/infra1/image_rect_raw/theora
> /camera/infra1/metadata
> /camera/infra2/camera_info
> /camera/infra2/image_rect_raw
> /camera/infra2/image_rect_raw/compressed
> /camera/infra2/image_rect_raw/compressedDepth
> /camera/infra2/image_rect_raw/theora
> /camera/infra2/metadata
> /parameter_events
> /rosout
> /tf
> /tf_static
> /visual_slam/imu
> /visual_slam/status
> /visual_slam/tracking/odometry
> /visual_slam/tracking/slam_path
> /visual_slam/tracking/vo_path
> /visual_slam/tracking/vo_pose
> /visual_slam/tracking/vo_pose_covariance
> /visual_slam/vis/gravity
> /visual_slam/vis/landmarks_cloud
> /visual_slam/vis/localizer
> /visual_slam/vis/localizer_loop_closure_cloud
> /visual_slam/vis/localizer_map_cloud
> /visual_slam/vis/localizer_observations_cloud
> /visual_slam/vis/loop_closure_cloud
> /visual_slam/vis/observations_cloud
> /visual_slam/vis/pose_graph_edges
> /visual_slam/vis/pose_graph_edges2
> /visual_slam/vis/pose_graph_nodes
> /visual_slam/vis/velocity
>
> ```
>
> Copy to clipboard


Check the frequency of the `realsense-camera` node’s output
frequency.





```
ros2 topic hz /camera/infra1/image_rect_raw --window 20

```

Copy to clipboard




> Example output:
>
> ```
> average rate: 89.714
>         min: 0.011s max: 0.011s std dev: 0.00025s window: 20
> average rate: 90.139
>         min: 0.010s max: 0.012s std dev: 0.00038s window: 20
> average rate: 89.955
>         min: 0.011s max: 0.011s std dev: 0.00020s window: 20
> average rate: 89.761
>         min: 0.009s max: 0.013s std dev: 0.00074s window: 20
>
> ```
>
> Copy to clipboard
>
> `Ctrl` \+ `c` to stop the output.


You can also check the frequency of IMU topic.





```
ros2 topic hz /camera/imu --window 20

```

Copy to clipboard




> Example output:
>
> ```
> average rate: 199.411
>         min: 0.004s max: 0.006s std dev: 0.00022s window: 20
> average rate: 199.312
>         min: 0.004s max: 0.006s std dev: 0.00053s window: 20
> average rate: 200.409
>         min: 0.005s max: 0.005s std dev: 0.00007s window: 20
> average rate: 200.173
>         min: 0.004s max: 0.006s std dev: 0.00028s window: 20
>
> ```
>
> Copy to clipboard


Verify that you are getting the output from the `visual_slam`
node at the same rate as the input.





```
ros2 topic hz /visual_slam/tracking/odometry --window 20

```

Copy to clipboard




> Example output:
>
> ```
> average rate: 58.086
>         min: 0.002s max: 0.107s std dev: 0.03099s window: 20
> average rate: 62.370
>         min: 0.001s max: 0.109s std dev: 0.03158s window: 20
> average rate: 90.559
>         min: 0.009s max: 0.013s std dev: 0.00066s window: 20
> average rate: 85.612
>         min: 0.002s max: 0.100s std dev: 0.02079s window: 20
> average rate: 90.032
>         min: 0.010s max: 0.013s std dev: 0.00059s window: 20
>
> ```
>
> Copy to clipboard


## Tutorial Walkthrough - Visualization [](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_realsense.html\#tutorial-walkthrough-visualization "Link to this heading")

You have two options for checking the `visual_slam`
output:

- **Live visualization**: Run RViz2 live while running
`realsense-camera` node and `visual_slam` nodes.

- **Offline visualization**: Record rosbag file and check the recorded
data offline (possibly on a different machine).


Running `RViz2` on a remote PC over the network can be challenging and can be
difficult especially when you have image message topics to subscribe because
of the added burden on the ROS 2 network transport.

Working on RViz2 in a X11-forwarded window can also be difficult because of
the network speed limitation.

Typically, if you are running `visual_slam` on Jetson, it is generally
recommended that you **NOT** evaluate with live visualization (1).

### Live Visualization [](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_realsense.html\#live-visualization "Link to this heading")

1. \[Terminal 2\] Open RViz2 from the second terminal:





```
rviz2 -d $(ros2 pkg prefix isaac_ros_visual_slam --share)/rviz/realsense.cfg.rviz

```

Copy to clipboard



As you move the camera, verify that the position and orientation of the frames
corresponds to how the camera moved relative to its starting
pose.
[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/realsense.gif/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/realsense.gif/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/realsense.gif/)

### Offline Visualization [](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_realsense.html\#offline-visualization "Link to this heading")

1. \[Terminal 2\] Save a rosbag file.

Record the output in your rosbag file, along with the input data for
later visual inspection.





```
export ROSBAG_NAME=courtyard-d435i
ros2 bag record -o ${ROSBAG_NAME} \
     /camera/imu /camera/accel/metadata /camera/gyro/metadata \
     /camera/infra1/camera_info /camera/infra1/image_rect_raw \
     /camera/infra1/metadata \
     /camera/infra2/camera_info /camera/infra2/image_rect_raw \
     /camera/infra2/metadata \
     /tf_static /tf \
     /visual_slam/status \
     /visual_slam/tracking/odometry \
     /visual_slam/tracking/slam_path /visual_slam/tracking/vo_path \
     /visual_slam/tracking/vo_pose /visual_slam/tracking/vo_pose_covariance \
     /visual_slam/vis/landmarks_cloud /visual_slam/vis/loop_closure_cloud \
     /visual_slam/vis/observations_cloud \
     /visual_slam/vis/pose_graph_edges /visual_slam/vis/pose_graph_edges2 \
     /visual_slam/vis/pose_graph_nodes
ros2 bag info ${ROSBAG_NAME}

```

Copy to clipboard



If you plan to run the rosbag on a remote machine (PC) for
evaluation, you can send the rosbag file to your remote machine.





```
export IP_PC=192.168.1.100
scp -r ${ROSBAG_NAME} ${PC_USER}@${IP_PC}:/home/${PC_USER}/workspaces/isaac_ros-dev/

```

Copy to clipboard

2. \[Terminal 1\] Launch RViz2.


> If you are SSHing into Jetson from your PC, make sure you enabled
> X forwarding by adding `-X` option with SSH command:
>
> ```
> ssh -X ${USERNAME_ON_JETSON}@${IP_JETSON}
>
> ```
>
> Copy to clipboard


Launch the Isaac ROS container:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
      ./scripts/run_dev.sh

```

Copy to clipboard



Run RViz with a configuration file for visualizing a set of messages
from Visual SLAM node.





```
cd /workspaces/isaac_ros-dev
rviz2 -d $(ros2 pkg prefix isaac_ros_visual_slam --share)/rviz/vslam_keepall.cfg.rviz

```

Copy to clipboard

3. \[Terminal 2\] Playback the recorded rosbag.

Attach another terminal to the running container.





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
      ./scripts/run_dev.sh

```

Copy to clipboard



Play the recorded rosbag file.





```
ros2 bag play ${ROSBAG_NAME}

```

Copy to clipboard



RViz starts showing a visualization similar to the following:


> [![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/RViz_0217-cube_vslam-keepall.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/RViz_0217-cube_vslam-keepall.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/RViz_0217-cube_vslam-keepall.png/)


Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/visual_slam/cuvslam/tutorial_realsense.html)[latest](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_realsense.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/concepts/visual_slam/cuvslam/tutorial_realsense.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/visual_slam/cuvslam/tutorial_realsense.html)