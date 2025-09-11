- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Reference Workflows](https://nvidia-isaac-ros.github.io/reference_workflows/index.html)
- [Isaac Manipulator](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/index.html)
- Tutorial for Pick and Place using cuMotion with Perception
- [View page source](https://nvidia-isaac-ros.github.io/_sources/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place.rst.txt)

* * *

# Tutorial for Pick and Place using cuMotion with Perception [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place.html\#tutorial-for-pick-and-place-using-cumotion-with-perception "Link to this heading")

[![RViz visualization of Isaac Manipulator pick and place for a robot simulated in Isaac Sim](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/isaac_manipulator_rviz_scene.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/isaac_manipulator_rviz_scene.png/)

## Overview [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place.html\#overview "Link to this heading")

This tutorial walks through the process of picking and placing of an object using the following packages.

- [Isaac ROS RT-DETR](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_object_detection/isaac_ros_rtdetr/index.html) for object detection

- [Isaac ROS FoundationPose](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_pose_estimation/isaac_ros_foundationpose/index.html) for object 3D pose estimation

- [Isaac ROS Nvblox](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/index.html) for 3D scene reconstruction

- [Isaac ROS cuMotion](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html) for motion planning with obstacle avoidance

- [Isaac ROS Object Attachment](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion_object_attachment/index.html) for estimating the object collision spheres


This tutorial assumes the following:

- A [Universal Robots](https://www.universal-robots.com/) manipulator (UR5e or UR10e) and a [Robotiq two-finger gripper](https://robotiq.com/products/adaptive-grippers#Two-Finger-Gripper) (2F-85 or 2F-140).

- A [RealSense cameras](https://www.intelrealsense.com/) or a [Hawk stereo camera](https://leopardimaging.com/leopard-imaging-hawk-stereo-camera/).

- The object is one supported by [sdetr\_grasp](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/isaac/models/synthetica_detr). In this tutorial, we use the “mac and cheese” box.

- Tabletop scene is static when the object is being picked and placed.

- Tabletop scene is static when the object detection and pose estimation are being performed.


This tutorial uses the following action servers:

- **Object detection server:** For detecting objects in the scene.

- **Pose estimation server:** For estimating the pose of the object in the scene.

- **Object info server:** For wrapping the object detection and pose estimation servers. This provides a common interface for getting the object information like the object pose or 2D bounding box.

- **Pick and place action server:** For triggering pick and place pipeline.

- **Planner server:** For planning with cuMotion.

- **Object attachment server:** For object attachment and detachment during planning.

- **Gripper server:** For controlling the gripper via ROS 2 actions.


When the pipeline is triggered using the action call mentioned below, the following things happen:

1. The objects in the scene are detected using the object info server and one object to pick is selected.

2. Using the `object_id` of object of interest from the previous call, the pose of the object is estimated using the pose estimation server.

3. After the pose of the object is determined, the pick phase planning using the planner server begins.

4. Pick phase planning involves the following steps:

   - Execute the first trajectory given by the planner server. This is the approach trajectory, which brings the gripper close to the object.

   - Close the gripper.

   - Execute the second trajectory given by the planner server. This is the retract trajectory which lifts the object.
5. After the object is picked, the robot footprint is modified to include the object’s collision spheres using the object attachment server.

6. Planning for the place phase using the planner server with updated robot footprint begins.

7. Place phase planning involves the following steps:

   - Execute the trajectory given by the planner server to reach the place pose.

   - Then we open the gripper to release the object.
8. The object attachment server is called to detach the object from the robot footprint.


Warning

The obstacle avoidance behavior demonstrated in this tutorial is not a safety function and does not
comply with any national or international functional safety standards. When testing obstacle avoidance
behavior, do not use human limbs or other living entities.

The pick-and-place pipeline consists of a series of action servers for object detection, pose estimation, object attachment, and motion planning,
along with a “pick-and-place orchestrator” for coordinating the overall pipeline. Expensive operations such as object detection and pose estimation
are performed on demand through action calls in order to minimize system load. For the purpose of collision avoidance, nvblox constantly integrates depth input from one or two depth cameras, maintaining a surface reconstruction in the form of a truncated signed distance field (TSDF).
A Euclidean signed distance field (ESDF) is computed only when needed for planning.

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_pick_and_place_workflow.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_pick_and_place_workflow.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_pick_and_place_workflow.png/)

High-level architecture of the Isaac Manipulator pick-and-place reference workflow [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place.html#id1 "Link to this image")

## Supported Hardware [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place.html\#supported-hardware "Link to this heading")

Compute:

The pick-and-place reference workflow has been tested on Jetson AGX Orin (64 GB).

Robots:

- [UR5e or UR10e](https://www.universal-robots.com/)


Grippers:

- [Robotiq 2F-85 or 2F-140](https://robotiq.com/products/adaptive-grippers#Two-Finger-Gripper)


Cameras:

Up to two RealSense cameras [supported by Isaac ROS](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/realsense_setup.html) or one [Hawk stereo camera](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/hawk_setup.html).

Multiple cameras can help reduce occlusion and noise in the scene and therefore increase the quality and completeness of the 3D reconstruction used for collision avoidance.
While the object following tutorial with multiple cameras runs scene reconstruction for obstacle-aware planning on all cameras, object detection and pose estimation are only enabled on the camera with the lowest index.

Reflective or smooth, featureless surfaces in the environment may increase noise in the depth estimation.

Use of multiple cameras is recommended.

Mixing stereo camera types is untested but may work with modifications to the launch files.

Warning

The obstacle avoidance behavior demonstrated in this tutorial is not a safety function and does not
comply with any national or international functional safety standards. When testing obstacle avoidance
behavior, do not use human limbs or other living entities.

## Requirements [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place.html\#requirements "Link to this heading")

Ensure that you have one of the NGC catalog objects that can be grasped, for example [sdetr\_grasp](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/isaac/models/synthetica_detr). This tutorial uses the **Mac and Cheese Box**.

If you are using FoundationPose, for the desired object, ensure that you have a mesh and a texture file available for it.

To prepare an object, review [FoundationPose’s documentation](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_pose_estimation/isaac_ros_foundationpose/index.html).

## Tutorial [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place.html\#tutorial "Link to this heading")

### Set Up Development Environment [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place.html\#set-up-development-environment "Link to this heading")

1. Set up your development environment by following the instructions in [getting started](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html).

2. Complete the camera setup:

   - For a RealSense camera, using the steps in [RealSense setup tutorial](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/realsense_setup.html).

   - For a Hawk stereo camera, using the steps in [Hawk setup tutorial](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/hawk_setup.html).
3. Clone `isaac_ros_common` under `${ISAAC_ROS_WS}/src`.





```
cd ${ISAAC_ROS_WS}/src && \
     git clone -b release-3.2 https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common.git isaac_ros_common

```

Copy to clipboard


### Set Up UR Robot [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place.html\#set-up-ur-robot "Link to this heading")

1. Refer to the [Set Up UR Robot section](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_e2e.html#set-up-ur-robot).


### Set Up Cameras for Robot [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place.html\#set-up-cameras-for-robot "Link to this heading")

1. Refer to the [Set Up Cameras for Robot section](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_e2e.html#set-up-cameras-for-robot).


### Build the Code [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place.html\#build-the-code "Link to this heading")

1. Open a **new** terminal inside the Docker container or launch the container for the first time:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
     ./scripts/run_dev.sh

```

Copy to clipboard

2. Clone the Isaac ROS fork of `ros2_robotiq_gripper` and `tylerjw/serial` under `${ISAAC_ROS_WS}/src`:





```
cd ${ISAAC_ROS_WS}/src && \
     git clone --recursive https://github.com/NVIDIA-ISAAC-ROS/ros2_robotiq_gripper && \
     git clone -b ros2 https://github.com/tylerjw/serial

```

Copy to clipboard





Note



- The fork is used to fix [this bug](https://github.com/PickNikRobotics/ros2_robotiq_gripper/issues/57) in the original repository.

- The custom `serial` package build is required because of [Issue 21](https://github.com/PickNikRobotics/ros2_robotiq_gripper/issues/21)


3. Building dependencies:





```
cd ${ISAAC_ROS_WS}
colcon build --symlink-install --packages-select-regex robotiq* serial --cmake-args "-DBUILD_TESTING=OFF" && \
source install/setup.bash

```

Copy to clipboard

4. Install this tutorial using source or Debian.



Installation from SourceInstallation from Debian





1. Clone this repository under `${ISAAC_ROS_WS}/src`:





```
cd ${ISAAC_ROS_WS}/src && git clone --recursive -b release-3.2 https://github.com/NVIDIA-ISAAC-ROS/isaac_manipulator.git isaac_manipulator

```

Copy to clipboard

2. Use `rosdep` to install the package’s dependencies:


> ```
> sudo apt-get update
>
> ```
>
> Copy to clipboard






```
rosdep update && rosdep install -i -r --from-paths \
      ${ISAAC_ROS_WS}/src/isaac_manipulator/isaac_manipulator_pick_and_place \
      --ignore-src --rosdistro humble -y

```

Copy to clipboard

3. Build and source the ROS workspace:





```
cd ${ISAAC_ROS_WS}
colcon build --symlink-install --packages-up-to isaac_manipulator_pick_and_place && \
source install/setup.bash

```

Copy to clipboard


### Set Up Perception Deep Learning Models [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place.html\#set-up-perception-deep-learning-models "Link to this heading")

1. Make sure to follow the FoundationPose tab of the tutorial.

2. Refer to the [Set Up Perception Deep Learning Models section](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_e2e.html#set-up-perception-deep-learning-models).


### Run Launch Files and Deploy to Robot [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place.html\#run-launch-files-and-deploy-to-robot "Link to this heading")

We recommend setting a `ROS_DOMAIN_ID` via `export ROS_DOMAIN_ID=<ID_NUMBER>` for every
new terminal where you run ROS commands, to avoid interference
with other computers in the same network ( [ROS Guide](https://docs.ros.org/en/humble/Concepts/Intermediate/About-Domain-ID.html)).

We recommend using Cyclone DDS for this tutorial when trying on real robot for better performance.

01. To enable Cyclone DDS, run the following command in each terminal (once) before running any other command.





    ```
    export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp

    ```

    Copy to clipboard

02. On the UR teach pendant, ensure that the robot’s **remote program** is loaded and that the robot is **paused** or **stopped** for safety purposes.

03. Open a **new** terminal inside the Docker container:





    ```
    cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
      ./scripts/run_dev.sh

    ```

    Copy to clipboard

04. Launch the tool\_communication.py script to communicate with the gripper on the UR robot:





    ```
    ros2 run ur_robot_driver tool_communication.py --ros-args -p robot_ip:=<ROBOT_IP_ADDRESS>

    ```

    Copy to clipboard



    This is an important step because it interacts with the Robotiq gripper and allows for programmatic control of the gripper.

05. Open a **new** terminal inside the Docker container:





    ```
    cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
      ./scripts/run_dev.sh

    ```

    Copy to clipboard

06. Launch perception nodes along with cuMotion for the UR robot:

    This tutorial was validated using `ur_type:=ur5e`, `ur_type:=ur10e` with gripper types of `robotiq_2f_140` and `robotiq_2f_85`, respectively. The default launch arguments assume
    that the model to be used is SyntheticaDETR v1.0.0 and that the object to be picked is the
    Mac and Cheese box. To pick a different object, change the object class ID and optionally
    the file path to the desired RT-DETR model file.



    Warning



    Add any obstacles that are not visible by the camera into the scene to prevent potential collisions.
    Consult the documentation [here](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion_moveit/index.html).





    HawkRealSense





1. Install the support package for the Hawk stereo camera:





```
sudo apt-get install -y ros-humble-isaac-ros-hawk

```

Copy to clipboard

2. Launch the example:


```
ros2 launch isaac_manipulator_pick_and_place ur_pick_and_place.launch.py \
   ur_type:=<UR_TYPE> robot_ip:=<ROBOT_IP_ADDRESS> \
   gripper_type:=<GRIPPER_TYPE> camera_type:=hawk setup:=<SETUP_NAME> \
   use_pose_from_rviz:=True \
   ess_engine_file_path:=${ISAAC_ROS_WS}/isaac_ros_assets/models/dnn_stereo_disparity/dnn_stereo_disparity_v4.1.0_onnx/ess.engine

```

Copy to clipboard

Warning

If you see the log `No depth images from X seconds`, consider changing the
`filter_depth_buffer_time:=` to a higher value (the unit is seconds). This will
allow object attachment to buffer more depth images from the past. The caveat
is that the software will operate on an older set of data that might lead to
creating object spheres that might not model the object position accurately.

The
other parameter of interest is the `time_sync_slop` parameter. It defines the
synchronization threshold for finding a matched pair of depth images, joint states
and transforms. For slower systems, slightly tweaking the value up will allow for
collision sphere and obstacle avoidance to work. Using a very large
value will synchronize older pieces of data together leading to unpredictable
downstream effects in planning, collision voxel cloud generation and obstacle
avoidance.

This only applies when `SPHERE` is used as the object attachment
type. The other modes do not use depth as input to create the object spheres.

07. Open **another** terminal inside the Docker container:





    ```
    cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
      ./scripts/run_dev.sh

    ```

    Copy to clipboard

08. On the UR teach pendant, press **play** to enable the robot.

09. Trigger the object detection:





    ```
    ros2 action send_goal /get_objects isaac_manipulator_interfaces/action/GetObjects {}

    ```

    Copy to clipboard



    If the action call does not return in a few seconds, it is possible that the object detection did not return any objects. In this case, try again.

10. Trigger the pick and place pipeline:

    When launching the ROS graph earlier, `` `use_pose_from_rviz` `` is set to `` `True` `` which creates a interactive marker that can be used to set the place pose.
    Use the marker controls to set the desired position and orientation. In this mode, the `` `place_pose` `` in the below command is ignored.





    ```
    ros2 action send_goal /pick_and_place isaac_manipulator_interfaces/action/PickAndPlace "{object_id : 0}"

    ```

    Copy to clipboard



    1. Wait for the terminal log to show `cuMotion is ready for planning queries!`, before triggering the pick and place pipeline.

    2. If the above action call does not return in a few seconds, it is possible that the pose estimation did not return any results. In this case, try again by calling the object detection action from previous step.
11. To do another object pick and place, one would need to do a service call to clear all current
    objects in the cache. One can do that via this service call. After this, one can run the entire
    pipeline again to pick and place another object.





    ```
    ros2 service call /clear_objects isaac_manipulator_interfaces/srv/ClearObjects

    ```

    Copy to clipboard



    This will clear all objects from the cache, if you only want to clear a certain object then
    you will need to specify the object id in the request.


Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place.html)[latest](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/index.html)