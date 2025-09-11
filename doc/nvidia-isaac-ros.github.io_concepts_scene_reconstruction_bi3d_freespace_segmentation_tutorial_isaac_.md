- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Freespace Segmentation](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_freespace_segmentation/index.html)
- [`isaac_ros_bi3d_freespace`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_freespace_segmentation/isaac_ros_bi3d_freespace/index.html)
- Tutorial for Freespace Segmentation with Isaac Sim
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/scene_reconstruction/bi3d_freespace_segmentation/tutorial_isaac_sim.rst.txt)

* * *

# Tutorial for Freespace Segmentation with Isaac Sim [](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/bi3d_freespace_segmentation/tutorial_isaac_sim.html\#tutorial-for-freespace-segmentation-with-isaac-sim "Link to this heading")

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_freespace_segmentation/Isaac_sim_tutorial.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_freespace_segmentation/Isaac_sim_tutorial.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_freespace_segmentation/Isaac_sim_tutorial.png/)

## Overview [](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/bi3d_freespace_segmentation/tutorial_isaac_sim.html\#overview "Link to this heading")

This tutorial demonstrates how to use a [Isaac\\
Sim](https://developer.nvidia.com/isaac-sim) and
[isaac\_ros\_bi3d\_freespace](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_freespace_segmentation/blob/main/isaac_ros_bi3d_freespace)
to create a local occupancy grid.

## Tutorial Walkthrough [](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/bi3d_freespace_segmentation/tutorial_isaac_sim.html\#tutorial-walkthrough "Link to this heading")

1. Complete the
[Isaac ROS Freespace Segmentation quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_freespace_segmentation/isaac_ros_bi3d_freespace/index.html#quickstart).

2. Install and launch Isaac Sim following the steps in the [Isaac ROS Isaac Sim Setup Guide](https://nvidia-isaac-ros.github.io/getting_started/isaac_sim/index.html)

3. Press **Play** to start publishing data from Isaac Sim.
[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/isaac_sim_sample_scene.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/isaac_sim_sample_scene.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/isaac_sim_sample_scene.png/)
4. Open a second terminal and attach to the container:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
./scripts/run_dev.sh

```

Copy to clipboard

5. In the second terminal, start the `isaac_ros_bi3d` node using the
launch files:





```
ros2 launch isaac_ros_bi3d_freespace isaac_ros_bi3d_freespace_isaac_sim.launch.py \
featnet_engine_file_path:=${ISAAC_ROS_WS}/isaac_ros_assets/models/bi3d_proximity_segmentation/featnet.plan \
segnet_engine_file_path:=${ISAAC_ROS_WS}/isaac_ros_assets/models/bi3d_proximity_segmentation/segnet.plan \
max_disparity_values:=32

```

Copy to clipboard



You should see a RViz window, as shown below:
[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_freespace_segmentation/Isaac_sim_rviz.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_freespace_segmentation/Isaac_sim_rviz.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_freespace_segmentation/Isaac_sim_rviz.png/)
6. Optionally, you can run the visualizer script to visualize the
disparity image:





```
ros2 run isaac_ros_bi3d isaac_ros_bi3d_visualizer.py --disparity_topic bi3d_mask

```

Copy to clipboard


[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_freespace_segmentation/Visualizer_isaac_sim.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_freespace_segmentation/Visualizer_isaac_sim.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_freespace_segmentation/Visualizer_isaac_sim.png/)

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/scene_reconstruction/bi3d_freespace_segmentation/tutorial_isaac_sim.html)[latest](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/bi3d_freespace_segmentation/tutorial_isaac_sim.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/concepts/scene_reconstruction/bi3d_freespace_segmentation/tutorial_isaac_sim.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/scene_reconstruction/bi3d_freespace_segmentation/tutorial_isaac_sim.html)