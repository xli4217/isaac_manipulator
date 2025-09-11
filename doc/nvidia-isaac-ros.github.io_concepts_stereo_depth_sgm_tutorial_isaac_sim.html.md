- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Image Pipeline](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/index.html)
- [`isaac_ros_stereo_image_proc`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_stereo_image_proc/index.html)
- Tutorial with Isaac Sim
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/stereo_depth/sgm/tutorial_isaac_sim.rst.txt)

* * *

# Tutorial with Isaac Sim [](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/sgm/tutorial_isaac_sim.html\#tutorial-with-isaac-sim "Link to this heading")

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/stereo_depth/sgm/isaac_sim_image_pipeline.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/stereo_depth/sgm/isaac_sim_image_pipeline.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/stereo_depth/sgm/isaac_sim_image_pipeline.png/)

## Overview [](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/sgm/tutorial_isaac_sim.html\#overview "Link to this heading")

This tutorial demonstrates how to perform depth-camera based
reconstruction using the `disparity_node` and stereo image pairs
streamed from Isaac Sim.

## Tutorial Walkthrough [](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/sgm/tutorial_isaac_sim.html\#tutorial-walkthrough "Link to this heading")

1. Complete the `isaac_ros_stereo_image_proc` [quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_stereo_image_proc/index.html#quickstart).

2. Install and launch Isaac Sim following the steps in the [Isaac ROS Isaac Sim Setup Guide](https://nvidia-isaac-ros.github.io/getting_started/isaac_sim/index.html).

3. Press **Play** to start publishing data from the Isaac Sim.
[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/isaac_sim_sample_scene.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/isaac_sim_sample_scene.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/isaac_sim_sample_scene.png/)
4. In a separate terminal, start the `isaac_ros_stereo_image_proc`
graph using the launch files:





```
ros2 launch isaac_ros_stereo_image_proc isaac_ros_stereo_image_pipeline_isaac_sim.launch.py

```

Copy to clipboard



You should see a RViz window, as shown below:
[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/stereo_depth/sgm/isaac_sim_image_pipeline.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/stereo_depth/sgm/isaac_sim_image_pipeline.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/stereo_depth/sgm/isaac_sim_image_pipeline.png/)

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/stereo_depth/sgm/tutorial_isaac_sim.html)[latest](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/sgm/tutorial_isaac_sim.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/concepts/stereo_depth/sgm/tutorial_isaac_sim.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/stereo_depth/sgm/tutorial_isaac_sim.html)