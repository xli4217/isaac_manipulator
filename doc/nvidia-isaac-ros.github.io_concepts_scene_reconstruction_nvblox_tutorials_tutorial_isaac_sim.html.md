- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Concepts](https://nvidia-isaac-ros.github.io/concepts/index.html)
- [Scene Reconstruction](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/index.html)
- [Nvblox](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/index.html)
- Isaac Sim Examples
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.rst.txt)

* * *

# Isaac Sim Examples [](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.html\#isaac-sim-examples "Link to this heading")

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nvblox/isaac_sim_nvblox_nav2.gif/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nvblox/isaac_sim_nvblox_nav2.gif/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nvblox/isaac_sim_nvblox_nav2.gif/)

This page contains tutorials for running [Isaac ROS Nvblox](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nvblox)
on simulated data streaming out of Isaac Sim.
The tutorial builds a reconstruction from simulated (depth) image data.
The reconstruction is converted to a 2D costmap that is passed to
[Nav2](https://nav2.org/) and used for navigation.
The tutorial describes options for using [Isaac ROS Visual SLAM](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_visual_slam)
and [Isaac ROS DNN Stereo Depth](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_dnn_stereo_depth) for pose and depth
estimation respectively, and for [reconstruction in the presence of people](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.html#reconstruction-with-people)
and [other dynamic objects](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.html#reconstruction-with-dynamic-scene-elements).

## Hardware Requirements [](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.html\#hardware-requirements "Link to this heading")

Simulating the scene and robot sensors requires an RTX-enabled GPU
of sufficient capability and memory capacity. In particular, we recommend an “ideal” machine in the
[Isaac Sim requirements](https://docs.omniverse.nvidia.com/isaacsim/latest/installation/requirements.html).

Note

The sample scene `localhost/NVIDIA/Assets/Isaac/4.2/Isaac/Samples/NvBlox/nvblox_sample_scene.usd`
simulates the output for a 3d lidar and three stereo cameras (depth + color images) by default.
If your system is experiencing compute or memory issue while running the scene, disabling
[Isaac Create Render Product](https://docs.omniverse.nvidia.com/py/isaacsim/source/extensions/omni.isaac.core_nodes/docs/ogn/OgnIsaacCreateRenderProduct.html)
for some of the sensors can reduce the system load.

## Prerequisites [](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.html\#prerequisites "Link to this heading")

These are the steps common to running all Nvblox examples in Isaac Sim. You must:

1. Complete the [Developer Environment Setup](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html).

2. Complete the [Isaac Sim Setup](https://nvidia-isaac-ros.github.io/getting_started/isaac_sim/index.html).


## Install [](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.html\#install "Link to this heading")

1. Complete the [nvblox quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox/index.html#quickstart).


## Isaac Sim Example [](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.html\#isaac-sim-example "Link to this heading")

This example runs a nvblox-based reconstruction with Isaac Sim
supplying images, ground-truth depth, and poses from Isaac Sim.
The reconstruction is used for navigation with [Nav2](https://nav2.org/).

- **Terminal #1:**


> 1. Opening a terminal from the Isaac Sim launcher GUI, as described in
> [Isaac Sim Setup](https://nvidia-isaac-ros.github.io/getting_started/isaac_sim/index.html).
>
> 2. Start the simulation by running:
>
>
>
>
>
> ```
> ./isaac-sim.sh
>
> ```
>
> Copy to clipboard
>
> 3. Open the scene at the path `localhost/NVIDIA/Assets/Isaac/4.2/Isaac/Samples/NvBlox/nvblox_sample_scene.usd`.
>
> 4. Play the scene to start the ROS communication from sim.

- **Terminal #2:**


> 1. Start the Isaac ROS Dev Docker container (if not started in the install step):
>
>
>
>
>
> ```
> cd $ISAAC_ROS_WS && ./src/isaac_ros_common/scripts/run_dev.sh
>
> ```
>
> Copy to clipboard
>
> 2. Navigate (inside the Docker) to the workspace folder and source the workspace:
>
>
>
>
>
> ```
> cd /workspaces/isaac_ros-dev
> source install/setup.bash
>
> ```
>
> Copy to clipboard
>
> 3. Set this flag to ensure DDS communication with Isaac Sim runs over UDP. The file mentioned should be in the Isaac ROS docker image.
>
>
>
>
>
> ```
> export FASTRTPS_DEFAULT_PROFILES_FILE=/usr/local/share/middleware_profiles/rtps_udp_profile.xml
>
> ```
>
> Copy to clipboard
>
> 4. Launch the example:
>
>
>
>
>
> ```
> ros2 launch nvblox_examples_bringup isaac_sim_example.launch.py
>
> ```
>
> Copy to clipboard

- **In RViz**


> 1. Click on the **2D Goal Pose** button. Validate that you see the mesh,
> costmap, and the robot moving towards the goal location, as
> shown at the top of this page.


## Selecting the Sensors [](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.html\#selecting-the-sensors "Link to this heading")

Nvblox can integrate data from 3d lidar and up to 3 cameras simultaneously.

To enable the 3d lidar or more cameras you may use the `lidar` and `num_cameras` arguments by
modifying the following command from [Isaac Sim Example](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.html#isaac-sim-example):

- **Terminal #2:**


> 3. Launch the example:
>
>
>
>
>
> ```
> ros2 launch nvblox_examples_bringup isaac_sim_example.launch.py \
> lidar:=<"lidar"> num_cameras:=<"num_cameras">
>
> ```
>
> Copy to clipboard
>
>
>
> where “lidar” can be set to `True` to enable 3d lidar and
> “num\_cameras” is either 0, 1 or 3.
> Setting “num\_cameras” to 1 will enable the front stereo camera (default)
> and setting it to 3 will enable the side stereo cameras additionally.


## Reconstruction With People [](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.html\#reconstruction-with-people "Link to this heading")

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nvblox/isaac_sim_nvblox_humans.gif/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nvblox/isaac_sim_nvblox_humans.gif/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nvblox/isaac_sim_nvblox_humans.gif/)

This tutorial demonstrates how to perform dynamic people
reconstruction in Nvblox using ground-truth people segmentation from Isaac Sim.
For more information on how people reconstruction works, see
[Technical Details](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/technical_details.html).

To run this example modify the following commands from [Isaac Sim Example](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.html#isaac-sim-example):

- **Terminal #1:**


> 1. Opening a terminal from the Isaac Sim launcher GUI, as described in
> [Isaac Sim Setup](https://nvidia-isaac-ros.github.io/getting_started/isaac_sim/index.html).
>
> 2. Start the simulation by running:
>
>
>
>
>
> ```
> ./isaac-sim.sh
>
> ```
>
> Copy to clipboard
>
> 3. Open the scene at the path `localhost/NVIDIA/Assets/Isaac/4.2/Isaac/Samples/NvBlox/nvblox_sample_scene.usd`.
>
> 4. Toggle /World/Humans to choose to have the scene with people.
>
> 5. Play the scene to start the ROS communication from sim.
>
>
> Note
>
> Because the animation requires execution of Python
> scripts, running the scene with the UI asks you
> to confirm that you want to enable script execution. Click Yes to
> make it possible to start the scene and the people animation.

- **Terminal #2:**


> 3. Launch the example:
>
>
>
>
>
> ```
> ros2 launch nvblox_examples_bringup isaac_sim_example.launch.py \
> mode:=people_segmentation num_cameras:=1
>
> ```
>
> Copy to clipboard
>
>
> Note
>
> Multi-camera and lidar data integration is not supported in `people_segmentation` mode.


## Reconstruction With Dynamic Scene Elements [](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.html\#reconstruction-with-dynamic-scene-elements "Link to this heading")

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nvblox/isaac_sim_dynamic_example.gif/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nvblox/isaac_sim_dynamic_example.gif/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nvblox/isaac_sim_dynamic_example.gif/)

This tutorial demonstrates how to build a reconstruction with dynamic elements in the scene (people and non-people)
using Isaac Sim data. For more information about how dynamic reconstruction works in Nvblox see
[Technical Details](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/technical_details.html).

To run this example modify the following commands from [Isaac Sim Example](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.html#isaac-sim-example):

- **Terminal #1:**


> 1. Opening a terminal from the Isaac Sim launcher GUI, as described in
> [Isaac Sim Setup](https://nvidia-isaac-ros.github.io/getting_started/isaac_sim/index.html).
>
> 2. Start the simulation by running:
>
>
>
>
>
> ```
> ./isaac-sim.sh
>
> ```
>
> Copy to clipboard
>
> 3. Open the scene at the path `localhost/NVIDIA/Assets/Isaac/4.2/Isaac/Samples/NvBlox/nvblox_sample_scene.usd`.
>
> 4. Toggle /World/Dynamics to choose to have the scene with dynamics.
>
> 5. Play the scene to start the ROS communication from sim.

- **Terminal #2:**


> 3. Launch the example:
>
>
>
>
>
> ```
> ros2 launch nvblox_examples_bringup isaac_sim_example.launch.py \
> mode:=dynamic
>
> ```
>
> Copy to clipboard
>
>
> Note
>
> Lidar data integration is not supported in dynamic mode.


### Running on a Custom Scene [](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.html\#running-on-a-custom-scene "Link to this heading")

To test the reconstruction on another scene:

- Make sure you use the same robot USD so that the topic names and Isaac Sim ROS
bridge is correctly set up.

- Make sure that humans you add to the scene have the `person`
semantic segmentation class. To do so, you can use the Semantics
Schema Editor on the top prim of the additional humans.


## Troubleshooting [](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.html\#troubleshooting "Link to this heading")

See
[Isaac Sim Issues](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox/troubleshooting/troubleshooting_nvblox_isaac_sim.html).

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.html)[latest](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/scene_reconstruction/nvblox/tutorials/tutorial_isaac_sim.html)