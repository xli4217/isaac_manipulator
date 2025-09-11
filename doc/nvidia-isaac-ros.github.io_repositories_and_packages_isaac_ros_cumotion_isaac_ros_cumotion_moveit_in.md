- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS cuMotion](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/index.html)
- `isaac_ros_cumotion_moveit`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion_moveit/index.rst.txt)

* * *

# `isaac_ros_cumotion_moveit` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion_moveit/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_cumotion/blob/main/isaac_ros_cumotion_moveit).

## Quickstart [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion_moveit/index.html\#quickstart "Link to this heading")

### Set Up Development Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion_moveit/index.html\#set-up-development-environment "Link to this heading")

1. Set up your development environment by following the instructions in [Getting Started](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html).

2. Clone `isaac_ros_common` under `${ISAAC_ROS_WS}/src`.





```
cd ${ISAAC_ROS_WS}/src && \
      git clone -b release-3.2 https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common.git isaac_ros_common

```

Copy to clipboard


### Install `isaac_ros_cumotion_moveit` and Examples [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion_moveit/index.html\#install-package-name-and-examples "Link to this heading")

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
sudo apt-get install -y ros-humble-isaac-ros-cumotion-examples

```

Copy to clipboard


### Set Up MoveIt 2 with cuMotion [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion_moveit/index.html\#set-up-moveit-2-with-cumotion "Link to this heading")

Complete the following steps for your selected robot.

Universal Robot (UR)FrankaAnother Robot

1. **Optional:** If running on a physical UR robot, launch the robot driver in a separate terminal.
Substitute the robot’s IP address for `<ROBOT_IP_ADDRESS>`, and replace `ur10e` with the correct
robot model (both here and in the steps below) if using a UR robot other than UR10e.





```
ros2 launch ur_robot_driver ur_control.launch.py ur_type:=ur10e robot_ip:=<ROBOT_IP_ADDRESS> launch_rviz:=false

```

Copy to clipboard

2. Generate a URDF for the UR10e from the corresponding xacro:





```
mkdir -p ${ISAAC_ROS_WS}/isaac_ros_assets/urdf && \
       xacro -o ${ISAAC_ROS_WS}/isaac_ros_assets/urdf/ur10e.urdf /opt/ros/humble/share/ur_description/urdf/ur.urdf.xacro ur_type:=ur10e name:=ur10e

```

Copy to clipboard

3. Launch MoveIt (including RViz).





```
ros2 launch isaac_ros_cumotion_examples ur.launch.py ur_type:=ur10e

```

Copy to clipboard

4. In a separate terminal, run the cuMotion planner node. Remember to first `source install/setup.bash` if package was built from source.





```
ros2 run isaac_ros_cumotion cumotion_planner_node --ros-args \
      -p robot:=$(ros2 pkg prefix --share isaac_ros_cumotion_robot_description)/xrdf/ur10e.xrdf \
      -p urdf_path:=${ISAAC_ROS_WS}/isaac_ros_assets/urdf/ur10e.urdf

```

Copy to clipboard


### Using the cuMotion Planner in MoveIt [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion_moveit/index.html\#using-the-cumotion-planner-in-moveit "Link to this heading")

Enable cuMotion by ensuring that `isaac_ros_cumotion` and `cuMotion` are selected within the **“Planning Library”** pane within the **“Context”** tab in
the bottom left corner of the RViz window.

![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion_moveit/moveit_context.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion_moveit/moveit_context.png/)

Then select a target pose for the robot end effector, and click the **“Plan”** button in the **“Planning”** tab.
For a demonstration of collision-aware planning, first add one or more obstacles in the **“Scene Objects”** tab.

Note

The **“Execute”** and **“Plan & Execute”** buttons in RViz will only work if a physical robot is connected or if the
robot is emulated in the corresponding robot driver or via an external simulator. The MoveIt configuration for Franka
includes such emulation via a “fake hardware” interface, but the same is not true for UR, which would require an
external simulator such as URSim.

![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion_moveit/moveit_planning.gif/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion_moveit/moveit_planning.gif/)

Warning

In normal operation, only a single MoveIt `move_group` node should be running at a time. If the node fails to
exit cleanly, however, it may result in two nodes running when the graph is next launched. RViz then connects
to both instances, which results in both requesting a plan from the cuMotion planner node via a `MoveGroup` action
at nearly the same time. The second to arrive preempts the first without waiting for in-flight CUDA operations to
complete, possibly resulting in “CUDA invalid” errors.

If such errors are observed, please ensure that only a single `move_group` node is running. The cuMotion planner
node will be made more robust in a future release.

## Try More Examples [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion_moveit/index.html\#try-more-examples "Link to this heading")

To continue your exploration, check out the following suggested examples:

- [Tutorial for integrating custom manipulators with cuMotion](https://nvidia-isaac-ros.github.io/concepts/manipulation/cumotion_moveit/tutorial_custom_manipulator.html)

- [Tutorial for cuMotion MoveIt plugin with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/manipulation/cumotion_moveit/tutorial_isaac_sim.html)

- [Tutorial for obstacle avoidance and object following using cuMotion with perception](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_e2e.html)

- [Tutorial for pick and place using cuMotion with perception](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place.html)


## Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion_moveit/index.html\#troubleshooting "Link to this heading")

### Isaac ROS Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion_moveit/index.html\#isaac-ros-troubleshooting "Link to this heading")

For solutions to problems with Isaac ROS, see [troubleshooting](https://nvidia-isaac-ros.github.io/troubleshooting/index.html).

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion_moveit/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion_moveit/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_cumotion/isaac_ros_cumotion_moveit/index.html)