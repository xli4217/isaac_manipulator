- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS cuMotion](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/index.html)
- `isaac_ros_cumotion`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.rst.txt)

* * *

# `isaac_ros_cumotion` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_cumotion/blob/main/isaac_ros_cumotion).

## Motion Generation [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#motion-generation "Link to this heading")

The motion generation capabilities provided by the cuMotion planner node are exposed via
a plugin for MoveIt 2. Please see the corresponding
[quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion_moveit/index.html#quickstart)
to get started.

## Robot Segmentation [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#robot-segmentation "Link to this heading")

### Quickstart [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#quickstart "Link to this heading")

#### Set Up Development Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#set-up-development-environment "Link to this heading")

1. Set up your development environment by following the instructions in [getting started](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html).

2. Clone `isaac_ros_common` under `${ISAAC_ROS_WS}/src`.





```
cd ${ISAAC_ROS_WS}/src && \
      git clone -b release-3.2 https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common.git isaac_ros_common

```

Copy to clipboard

3. (Optional) Install dependencies for any sensors you want to use by following the [sensor-specific guides](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/index.html).



Note



We strongly recommend installing all sensor dependencies **before** starting any quickstarts.
Some sensor dependencies require restarting the Isaac ROS Dev container during installation, which will interrupt the quickstart process.


#### Download Quickstart Assets for Robot Segmentation [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#download-quickstart-assets-for-robot-segmentation "Link to this heading")

1. Download the `r2b_robotarm` dataset from the [r2b 2024 dataset on NGC](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/isaac/resources/r2bdataset2024)

2. Place the dataset at `${ISAAC_ROS_WS}/isaac_ros_assets/r2b_2024/r2b_robotarm`.


##### Build `isaac_ros_cumotion` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#build-package-name "Link to this heading")

Binary PackageBuild from Source

1. Launch the Docker container using the `run_dev.sh` script:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
./scripts/run_dev.sh

```

Copy to clipboard

2. Install the prebuilt Debian package:


> ```
> sudo apt-get update
>
> ```
>
> Copy to clipboard






```
sudo apt-get install -y ros-humble-isaac-ros-cumotion

```

Copy to clipboard


#### Run Launch File for Robot Segmentation [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#run-launch-file-for-robot-segmentation "Link to this heading")

Rosbag

1. Continuing inside the docker container, run the following launch file to spin up a demonstration of robot
segmentation for a single camera using the rosbag:





```
ros2 launch isaac_ros_cumotion robot_segmentation.launch.py

```

Copy to clipboard

2. Open a **second** terminal inside the Docker container:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
./scripts/run_dev.sh

```

Copy to clipboard

3. Run the rosbag file to simulate an image stream:





```
ros2 bag play --clock -l ${ISAAC_ROS_WS}/isaac_ros_assets/r2b_2024/r2b_robotarm \
   --remap /camera_1/aligned_depth_to_color/image_raw:=/depth_image \
camera_1/color/camera_info:=rgb/camera_info

```

Copy to clipboard

4. Open a **third** terminal inside the Docker container:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
./scripts/run_dev.sh

```

Copy to clipboard

5. Run a static transform publisher, used by the segmentation node to determine the relative pose of the robot base:





```
ros2 run tf2_ros static_transform_publisher --frame-id ar_tag --child-frame-id  base_link --x -0.30 --y 0.47 --z 0.0 --qx 0 --qy 0 --qz 0.707 --qw 0.707

```

Copy to clipboard


#### Visualize Results of Robot Segmentation [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#visualize-results-of-robot-segmentation "Link to this heading")

1. Open a **new** terminal inside the Docker container:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
      ./scripts/run_dev.sh

```

Copy to clipboard

2. Visualize the robot mask in `RViz`:





```
rviz2

```

Copy to clipboard



Then click on the `Add` button and select `By topic`. In the `By topic` window, select the topic
`/cumotion/camera_1/robot_mask`.

Optionally, you can also add the camera image to the RViz window by selecting the topic
`/camera_1/color/image_raw`. This will allow you to confirm that the robot mask correctly matches the
camera image.



Note



Due to initial warm-up time, the visualization may take up to 1 minute to appear.
During this time, the depth mask may stutter or lag behind the camera image.


[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/cumotion_rviz.gif/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/cumotion_rviz.gif/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/cumotion_rviz.gif/)

## Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#troubleshooting "Link to this heading")

### Isaac ROS Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#isaac-ros-troubleshooting "Link to this heading")

For solutions to problems with Isaac ROS, see [troubleshooting](https://nvidia-isaac-ros.github.io/troubleshooting/index.html).

## API [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#api "Link to this heading")

### CumotionActionServer [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#cumotionactionserver "Link to this heading")

#### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#ros-parameters "Link to this heading")

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `robot` | `string` | `ur5e.xrdf` | Path to the XRDF file for the robot |
| `urdf_path` | `string` | `rclpy.Parameter.Type.STRING` | Path to the robot’s URDF |
| `time_dilation_factor` | `float` | `0.5` | Speed scaling factor for the planner |
| `max_attempts` | `int` | `10` | Maximum number of attempts to solve the motion generation problem |
| `num_graph_seeds` | `int` | `6` | Number of seeds to use for the graph-based planner |
| `num_trajopt_seeds` | `int` | `6` | Number of seeds to use for trajectory optimization |
| `include_trajopt_retract_seed` | `bool` | `true` | When true, each trajectory optimization seed that linearly interpolates from start to goal is augmented by a second seed trajectory that interpolates from start to the default c-space configuration of the robot and then to the goal. The default c-space configuration is user-defined (specified in the XRDF file) but typically corresponds to a natural “resting” or retracted state. |
| `num_trajopt_time_steps` | `int` | `32` | Number of waypoints to use for trajectory optimization |
| `interpolation_dt` | `float` | `0.025` | Fixed time step (in seconds) used for interpolating the optimized trajectory |
| `collision_cache_cuboid` | `int` | `20` | Number of cuboid objects to pre-allocate in the cuMotion world |
| `collision_cache_mesh` | `int` | `20` | Number of mesh objects to pre-allocate in the cuMotion world |
| `voxel_size` | `float` | `0.05` | Size of a voxel in meters |
| `read_esdf_world` | `bool` | `false` | When true, indicates that cuMotion should read a Euclidean signed distance field (ESDF) as part of its world |
| `publish_curobo_world_as_voxels` | `bool` | `false` | When true, indicates that cuMotion should publish its world representation |
| `add_ground_plane` | `bool` | `false` | When true, indicates that cuMotion should add an implicit ground plane |
| `publish_voxel_size` | `float` | `0.05` | Voxel size to use when publishing cuMotion’s world |
| `max_publish_voxels` | `int` | `50000` | Maximum number of voxels that may be used to publish cuMotion’s world |
| `joint_states_topic` | `string` | `/joint_states` | Topic for reading the robot’s joint state |
| `tool_frame` | `string` | `rclpy.Parameter.Type.STRING` | Tool frame of the robot that should be used for planning |
| `workspace_file_path` | `string` | `''` | Path to a workspace file that defines the grid boundaries. See `isaac_manipulator_bringup/config/nvblox/workspace_bounds/zurich_test_bench.yaml` for an example. |
| `grid_size_m` | `float array` | `[2.0, 2.0, 2.0]` | Size of the grid in meters. Only used when `workspace_file_path` is not set. |
| `grid_center_m` | `float array` | `[0.0, 0.0, 0.0]` | Grid center position at which to place cuMotion’s world. Only used when `workspace_file_path` is not set. |
| `esdf_service_name` | `string` | `/nvblox_node/get_esdf_and_gradient` | Service to call when querying the ESDF world |
| `enable_curobo_debug_mode` | `bool` | `false` | When true, cuMotion’s backend planning library (currently cuRobo) will log additional debug messages |
| `override_moveit_scaling_factors` | `bool` | `false` | When true, the planner is allowed to override MoveIt’s scaling factors |

#### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#ros-topics-subscribed "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `joint_states_topic` | [sensor\_msgs/JointState](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/JointState.msg) | Joint states of the robot |

#### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#ros-topics-published "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `/curobo/voxels` | [visualization\_msgs/Marker](https://github.com/ros2/common_interfaces/blob/humble/visualization_msgs/msg/Marker.msg) | cuMotion’s world represented as voxels for visualization |

#### ROS Services Requested [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#ros-services-requested "Link to this heading")

| ROS Service | Interface | Description |
| --- | --- | --- |
| `esdf_service_name` | [nvblox\_msgs/EsdfAndGradients](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nvblox/blob/main/nvblox_msgs/srv/EsdfAndGradients.srv) | Service that takes an axis-aligned bounding box (AABB) for the planning region and returns a dense ESDF and gradient field for that region. |

#### ROS Actions Advertised [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#ros-actions-advertised "Link to this heading")

| ROS Action | Interface | Description |
| --- | --- | --- |
| `cumotion/move_group` | [moveit\_msgs/MoveGroup](https://github.com/moveit/moveit_msgs/blob/humble/action/MoveGroup.action) | Creates a motion plan and forwards it to MoveIt |

### CumotionRobotSegmenter [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#cumotionrobotsegmenter "Link to this heading")

#### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#id1 "Link to this heading")

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `robot` | `string` | `ur5e.xrdf` | Path to the XRDF file for the robot |
| `urdf_path` | `string` | `rclpy.Parameter.Type.STRING` | Path to the robot’s URDF |
| `cuda_device` | `int` | `0` | CUDA device index to use |
| `distance_threshold` | `float` | `0.1` | Maximum distance from a given collision sphere (in meters) at which to mask points |
| `time_sync_slop` | `float` | `0.1` | Maximum allowed delay (in seconds) for which depth image and joint state messages are considered synchronized |
| `tf_lookup_duration` | `float` | `5.0` | Maximum duration (in seconds) for which to block while waiting for a transform to become available |
| `joint_states_topic` | `string` | `/joint_states` | Topic to subscribe to for the robot’s joint states |
| `debug_robot_topic` | `string` | `/cumotion/robot_segmenter/robot_spheres` | Topic on which to publish the spheres representing the robot |
| `depth_image_topics` | `string array` | `['/cumotion/depth_1/image_raw']` | List of topics to subscribe to for input depth images |
| `depth_camera_infos` | `string array` | `['/cumotion/depth_1/camera_info']` | List of topics to subscribe to for camera info corresponding to the input depth image streams |
| `robot_mask_publish_topics` | `string array` | `['/cumotion/depth_1/robot_mask']` | List of topics on which to publish the robot segmentation masks for the corresponding depth image streams |
| `world_depth_publish_topics` | `string array` | `['/cumotion/depth_1/world_depth']` | List of topics on which to publish depth images with the robot segmented out |
| `log_debug` | `bool` | `False` | When true, increases logging verbosity to help with debugging |

#### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#id2 "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `joint_states_topic` | [sensor\_msgs/JointState](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/JointState.msg) | Joint states of the robot |
| `depth_image_topics` | [sensor\_msgs/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | Input depth images, including the robot to be segmented out |
| `depth_camera_infos` | [sensor\_msgs/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Topics to subscribe to for the camera info corresponding to the input depth image streams |

#### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html\#id3 "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `debug_robot_topic` | [visualization\_msgs/MarkerArray](https://github.com/ros2/common_interfaces/blob/humble/visualization_msgs/msg/MarkerArray.msg) | Robot represented by spheres for debugging and visualization |
| `robot_mask_publish_topics` | [sensor\_msgs/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | Robot segmentation masks for the corresponding depth image streams |
| `world_depth_publish_topics` | [sensor\_msgs/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | Depth images with the robot segmented out |

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html)