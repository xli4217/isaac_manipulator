- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Concepts](https://nvidia-isaac-ros.github.io/concepts/index.html)
- [Visual SLAM](https://nvidia-isaac-ros.github.io/concepts/visual_slam/index.html)
- [cuVSLAM](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/index.html)
- Tutorial for Visual SLAM with Isaac Sim
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/visual_slam/cuvslam/tutorial_isaac_sim.rst.txt)

* * *

# Tutorial for Visual SLAM with Isaac Sim [](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_isaac_sim.html\#tutorial-for-visual-slam-with-isaac-sim "Link to this heading")

[![After Localization](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/After_localization.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/After_localization.png/)

## Overview [](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_isaac_sim.html\#overview "Link to this heading")

This tutorial walks you through a graph to estimate 3D pose of the
camera with [Visual SLAM](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_visual_slam)
using images from Isaac Sim.

## Tutorial Walkthrough [](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_isaac_sim.html\#tutorial-walkthrough "Link to this heading")

1. Complete the [quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_visual_slam/isaac_ros_visual_slam/index.html#quickstart).

2. Launch the Docker container using the `run_dev.sh` script:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
     ./scripts/run_dev.sh

```

Copy to clipboard

3. Install and launch Isaac Sim following the steps in the [Isaac ROS Isaac Sim Setup Guide](https://nvidia-isaac-ros.github.io/getting_started/isaac_sim/index.html)

4. Press **Play** to start publishing data from the Isaac Sim.
[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/isaac_sim_sample_scene.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/isaac_sim_sample_scene.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/isaac_sim_sample_scene.png/)
5. In a separate terminal, start `isaac_ros_visual_slam` using the
launch files:





```
ros2 launch isaac_ros_visual_slam isaac_ros_visual_slam_isaac_sim.launch.py

```

Copy to clipboard


7. In a separate terminal, send the signal to move the robot about as follows:





```
ros2 topic pub --once /cmd_vel geometry_msgs/msg/Twist "{linear: {x: 0.2, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 0.2}}"

```

Copy to clipboard


7. In a separate terminal, spin up RViz with default configuration file
to see the rich visualizations as the robot moves.





```
rviz2 -d $(ros2 pkg prefix isaac_ros_visual_slam --share)/rviz/isaac_sim.cfg.rviz

```

Copy to clipboard


[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/Rviz_isaac_sim.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/Rviz_isaac_sim.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/Rviz_isaac_sim.png/)
8. To see the odometry messages, in a separate terminal echo the contents of the `/visual_slam/tracking/odometry` topic with the
following command:





```
ros2 topic echo /visual_slam/tracking/odometry

```

Copy to clipboard


[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/Terminal_output.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/Terminal_output.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/Terminal_output.png/)

## Saving and Using the Map [](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_isaac_sim.html\#saving-and-using-the-map "Link to this heading")

As soon as you start the visual SLAM node, it starts storing the
landmarks and the pose graph.

To save them in a map and store the
map onto a disk:

1\. Make a call to the `SaveMap` ROS 2 service with the
following command:

> Note
>
> `/path/to/save/the/map` must be a new empty directory
> every time you call this service because this service overwrites the
> existing contents.

```
ros2 service call /visual_slam/save_map isaac_ros_visual_slam_interfaces/srv/FilePath "{file_path: /path/to/save/the/map}"

```

Copy to clipboard

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/Save_map.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/Save_map.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/Save_map.png/)[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/Rviz_isaac_sim_mapping.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/Rviz_isaac_sim_mapping.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/Rviz_isaac_sim_mapping.png/)

2\. To verify, try to load and localize in the previously saved map.
Stop the `visual_slam` node launched for creating and saving the map, then relaunch it.

3\. Use the following command to load the map from the disk and provide an
approximate start location (prior). The orientation is currently ignored.

```
ros2 service call /visual_slam/localize_in_map isaac_ros_visual_slam_interfaces/srv/LocalizeInMap "
  map_folder_path: '/path/to/save/the/map'
  pose_hint:
    position:
      x: x-position
      y: y-position
      z: z-position
    orientation:
      x: 0.0
      y: 0.0
      z: 0.0
      w: 1.0"

```

Copy to clipboard

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/Load_and_localize.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/Load_and_localize.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/Load_and_localize.png/)

After the above step returns success, you have successfully loaded and
localized your robot in the map.

If it results in failure, you might have current landmarks from the approximate start
location that are not matching with stored landmarks and you need to provide
another valid value.

[![Before Localization](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/Before_localization.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/Before_localization.png/)[![After Localization](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/After_localization.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_slam/cuvslam/After_localization.png/)

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/visual_slam/cuvslam/tutorial_isaac_sim.html)[latest](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_isaac_sim.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/concepts/visual_slam/cuvslam/tutorial_isaac_sim.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/visual_slam/cuvslam/tutorial_isaac_sim.html)