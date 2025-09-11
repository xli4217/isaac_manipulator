- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Reference Workflows](https://nvidia-isaac-ros.github.io/reference_workflows/index.html)
- [Isaac Manipulator](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/index.html)
- Isaac Manipulator Reference Architecture
- [View page source](https://nvidia-isaac-ros.github.io/_sources/reference_workflows/isaac_manipulator/reference_architecture.rst.txt)

* * *

# Isaac Manipulator Reference Architecture [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/reference_architecture.html\#isaac-manipulator-reference-architecture "Link to this heading")

## What is NVIDIA Isaac Manipulator? [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/reference_architecture.html\#what-is-nvidia-isaac-manipulator "Link to this heading")

NVIDIA Isaac Manipulator is a collection of GPU-accelerated libraries and Isaac ROS
packages for perception-driven manipulation using robotic arms. The targeted use-cases
range from automated inspection to pick-and-place for logistics and manufacturing. Isaac
Manipulator provides capabilities for object detection, object pose estimation, and
time-optimal collision-free motion generation using cuMotion. cuMotion is a
GPU-accelerated library developed by NVIDIA for computing time-optimal, minimal-jerk
trajectories for serial robot arms. These modular and extensible capabilities allow
developers to integrate any combination of packages into their existing pipelines to
accelerate tasks.

## What is this Reference Architecture? [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/reference_architecture.html\#what-is-this-reference-architecture "Link to this heading")

This reference architecture explains the various components that are part of Isaac Manipulator. It
includes a validated reference architecture that has been tested and validated to run on
NVIDIA Jetson Orin platforms and Ampere or higher NVIDIA GPUs.

## Who is this document for? [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/reference_architecture.html\#who-is-this-document-for "Link to this heading")

This document is to assist engineers in the robotic platform ecosystem, including those at
Original Equipment Manufacturers (OEMs), Systems Integrators (SIs), and Independent
Software Vendors (ISVs). It provides guidance on utilizing the reference architecture as a
starting point to develop robust and performant manipulation solutions for robotic arms.

## Reference Architecture [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/reference_architecture.html\#reference-architecture "Link to this heading")

The Reference Architecture for Isaac Manipulator includes the following components.

1. **Visual Input:** Generates RGB and Depth images using live or simulated stereo cameras. This component uses the **ESS depth estimation** model to generate depth images when using RGB cameras.

2. **Environment and Obstacle Perception:** Integrates RGB images, depth images, camera poses, and segmentation masks to create environment representation. This component uses **Nvblox** for 3D scene reconstruction.

3. **Robot Configuration:** Provides robot description.

4. **Goal State Estimation:** Provides desired goal pose to achieve.

5. **Motion Planner:** Generates trajectory for collision-free motion to goal. This component uses the **cuMotion** library for generating time-optimal trajectories.

6. **Hardware Platform:** Includes list of supported compute platforms and robot arm manipulators.


This architecture is shown in the following diagram:

![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_RA_main.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_RA_main.png/)

The components in the reference architecture represent functional blocks that can have
multiple implementations. The primary implementation for each component has
undergone testing by NVIDIA.

## Components [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/reference_architecture.html\#components "Link to this heading")

This section details the components used in Isaac Manipulator and provides links to tested
examples you can run out of the box.

### Component 1 - Visual Input and depth estimation [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/reference_architecture.html\#component-1-visual-input-and-depth-estimation "Link to this heading")

![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_RA_component1.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_RA_component1.png/)

This component is responsible for supplying the RGB and depth images required by the
Environment and Obstacle Perception (2) component.

In the reference architecture, this component is implemented using:

- RGB camera: HAWK stereo cameras for capturing RGB images. The RGB images provided by these cameras are processed by the ESS Depth Estimation model, which generates the depth images required for environment perception.

- RGBD camera: The Intel RealSense camera provides RGBD images, which can be used directly by the environment perception component.


Useful Links

- [HAWK stereo cameras](https://leopardimaging.com/leopard-imaging-hawk-stereo-camera)

- [ESS Depth Estimation](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/ess/index.html)

- [Supported Intel RealSense cameras](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/realsense_setup.html)


There are other ways visual input can be provided to the workflow. These include the
following input sources:

- Rosbag: Running with pre-recorded data from a rosbag.

- NVIDIA Isaac Sim: The tutorial below shows how to use Isaac Manipulator with Isaac Sim. It demonstrates trajectory planning for a Franka robot simulated in Isaac Sim.


Hint

- [Tutorial for robot trajectory planning using HAWK or RealSense camera.](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_e2e.html)

- [Tutorial for cuMotion MoveIt Plugin with Isaac Sim.](https://nvidia-isaac-ros.github.io/concepts/manipulation/cumotion_moveit/tutorial_isaac_sim.html)

- [Tutorial for Isaac Manipulator workflows with Isaac Sim](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_isaac_sim.html)


### Component 2 - Nvblox for Environment and Obstacle Perception [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/reference_architecture.html\#component-2-nvblox-for-environment-and-obstacle-perception "Link to this heading")

![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_RA_component2.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_RA_component2.png/)

This component provides a representation of the environment and obstacles in it for
avoiding collisions during robot arm motion.

In the reference architecture, this obstacle representation is created by Nvblox, a library for
3D scene reconstruction. Nvblox processes RGB images and depth images from the Visual
Input (component 1) to produce cuboids or meshes representing obstacles. This is utilized
by the Motion Planner (component 5) for planning collision-free motion.

Camera calibration is required while using Nvblox, which is done using the MoveIt
Calibration package. For information on how to use this, please visit the link for setting up a
UR5e robot below.

Useful Links

- [Nvblox](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/index.html)

- [MoveIt Calibration](https://github.com/illumo-robotics/moveit_calibration)

- [Setting Up UR5e robot](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_e2e.html)


### Component 3 - Robot Configuration [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/reference_architecture.html\#component-3-robot-configuration "Link to this heading")

![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_RA_component3.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_RA_component3.png/)

This component provides the robot configuration to cuMotion - the Motion Planner
(component 5). This configuration is used to generate motion and perform segmentation
to filter out the robot arm.

Two files are required:

1. URDF (Universal Robot Description Format) file - for basic kinematics.


2\. XRDF (Extended Robot Description Format) file - for collision geometry, definition of
the configuration space, and more data. Please see Robot Configuration below.

Useful Links

- [Robot Configuration](https://nvidia-isaac-ros.github.io/concepts/manipulation/index.html)


### Component 4 - Goal State Estimation [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/reference_architecture.html\#component-4-goal-state-estimation "Link to this heading")

![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_RA_component4.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_RA_component4.png/)

This component provides an interface for users to give a goal pose to the robot arm.
In the reference architecture, this is done using RViz or via Python scripts.

Below is the MotionPlanning panel on RViz from where you can enable cuMotion and select
a target goal pose for the robot.

![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_RA_rviz.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_RA_rviz.png/)

Refer to the Isaac ROS MoveIt goal setter on how to provide the goal state using scripts.

Useful Links

- [Isaac ROS MoveIt goal setter](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_moveit_goal_setter/index.html)


### Component 5 - Motion Planner [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/reference_architecture.html\#component-5-motion-planner "Link to this heading")

![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_RA_component5.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_RA_component5.png/)

This is the key component in the Isaac Manipulator pipeline - cuMotion.

The environment state (from component 2), robot configuration (from component 3) and
goal pose (from component 4) are passed as input to cuMotion. The cuMotion Planner’s
motion generation functions are exposed via a plugin for MoveIt 2.

cuMotion includes functions to accurately segment and filter out parts of the robot. This
eliminates misleading contributions from the robot itself while reconstructing obstacles in
the environment.

![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_RA_segmentation.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_RA_segmentation.png/)

cuMotion generates a time-optimal trajectory to the goal state considering obstacles in
the environment. This joint-space trajectory is passed to the robot controller on the robot
for execution. The robot controller is exposed via drivers on a physical robot or the Isaac
Sim API in a simulated setup.

Useful Links

- [NVIDIA cuMotion](https://nvidia-isaac-ros.github.io/concepts/manipulation/index.html)

- [Isaac ROS cuMotion](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/index.html)

- [cuRobo library](https://curobo.org/)

- [MoveIt 2 documentation](https://moveit.picknik.ai/main/index.html)

- [Isaac ROS cuMotion Robot Segmentation](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion/index.html)


### Component 6 - Hardware Platform [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/reference_architecture.html\#component-6-hardware-platform "Link to this heading")

![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_RA_component6.PNG/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/reference_workflows/isaac_manipulator/manipulator_RA_component6.PNG/)

This component consists of hardware platforms and robot arm manipulators on which
Isaac Manipulator has been tested:

1. Universal Robots UR5e manipulator

2. Universal Robots UR10e manipulator

3. Franka


Isaac Manipulator has been tested on the Jetson Orin platforms running JetPack 6 and on
x86\_64 systems with NVIDIA GPUs. See more in Supported Platforms below.

Useful Links

- [cuMotion Supported Platforms](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/index.html)

- [NVIDIA Jetson Orin](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin)

- [Universal Robots](https://www.universal-robots.com/)

- [Franka Robotics](https://franka.de/)


Hint

- [Tutorial for cuMotion with a Universal Robots manipulator.](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_e2e.html)

- [Tutorial for Isaac Manipulator workflows with Isaac Sim](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_isaac_sim.html)


### Integration with additional Isaac packages [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/reference_architecture.html\#integration-with-additional-isaac-packages "Link to this heading")

Some use-cases would additionally benefit from the following Isaac packages. These can
be integrated into existing pipelines that optionally may be using Isaac Manipulator
components.

- **Isaac ROS Pose Estimation** \- estimating the 6 DoF pose of objects using FoundationPose. FoundationPose is a pre-trained model that works on different, novel objects without retraining. The output includes object pose and tracking information, which can be used by manipulator grippers to calculate grasps for objects.

- **Isaac ROS Object Detection** \- detecting objects using SyntheticaDETR trained models. SyntheticaDETR is trained exclusively on synthetically generated data using NVIDIA Omniverse. This package can be used individually for object detection and the output bounding boxes can also be used as initial estimates for FoundationPose when used together.


Useful Links

- [FoundationPose model](https://nvidia-isaac-ros.github.io/concepts/pose_estimation/foundationpose/index.html)

- [Isaac ROS Pose Estimation](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_pose_estimation/index.html)

- [SyntheticaDETR models](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/isaac/models/synthetica_detr)

- [Isaac ROS Object Detection](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_object_detection/index.html)

- [NVIDIA Omniverse](https://www.nvidia.com/en-us/omniverse)


Hint

- [Isaac ROS Pose Estimation using FoundationPose](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_pose_estimation/isaac_ros_foundationpose/index.html)

- [Tutorial to launch FoundationPose Tracking](https://nvidia-isaac-ros.github.io/concepts/pose_estimation/foundationpose/tutorial_tracking.html)


## Summary [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/reference_architecture.html\#summary "Link to this heading")

This reference architecture for Isaac Manipulator explains components and examples that have undergone
testing, and provide a robust foundation for developing end-use case applications. Individual components of Isaac Manipulator can be used together and integrated to meet diverse application requirements.

## Additional Resources [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/reference_architecture.html\#additional-resources "Link to this heading")

Check out our resources on using Isaac Manipulator with your robots.

**Learn more about featured NVIDIA solutions:**

> - [cuMotion](https://nvidia-isaac-ros.github.io/concepts/manipulation/index.html)
>
> - [Nvblox](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/index.html)
>
> - [NVIDIA Isaac Sim](https://developer.nvidia.com/isaac/sim)
>
> - [NVIDIA Omniverse](https://www.nvidia.com/en-us/omniverse)
>
> - [NGC pre-trained models](https://catalog.ngc.nvidia.com/models?filters=&orderBy=weightPopularDESC&query=&page=&pageSize=)

**Review Documentation & Samples:**

> - [Isaac Manipulator](https://developer.nvidia.com/isaac/manipulator)
>
> - [Developer Documentation](https://nvidia-isaac-ros.github.io/index.html)
>
> - [NVIDIA Isaac ROS](https://developer.nvidia.com/isaac/ros)
>
> - [Isaac ROS Developer Forum](https://forums.developer.nvidia.com/c/agx-autonomous-machines/isaac/isaac-ros/600)

**Build your business case:**

> - See how [Intrinsic.ai](https://developer.nvidia.com/blog/automating-smart-pick-and-place-with-intrinsic-flowstate-and-nvidia-isaac-manipulator/) uses Isaac Manipulator to generate grasp poses and robot motions, generating motion plans within 30 milliseconds.
>
> - See how to [design, create, and deploy](https://developer.nvidia.com/blog/create-design-and-deploy-robotics-applications-using-new-nvidia-isaac-foundation-models-and-workflows/) robotics applications using NVIDIA Isaac.

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/reference_workflows/isaac_manipulator/reference_architecture.html)[latest](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/reference_architecture.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/index.html)