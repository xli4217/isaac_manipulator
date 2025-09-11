- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS cuMotion](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/index.html)
- `isaac_ros_moveit_goal_setter`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_cumotion/isaac_ros_moveit_goal_setter/index.rst.txt)

* * *

# `isaac_ros_moveit_goal_setter` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_moveit_goal_setter/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_cumotion/blob/main/isaac_ros_moveit_goal_setter).

## Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_moveit_goal_setter/index.html\#overview "Link to this heading")

The `isaac_ros_moveit_goal_setter` package offers functionality for interfacing with MoveIt 2.
The package consists of two parts: (1) a server node that receives goals as a service request and
forwards the request to MoveIt and (2) a client node that generates goals that will be sent to the server.

## API [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_moveit_goal_setter/index.html\#api "Link to this heading")

### GoalSetterNode [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_moveit_goal_setter/index.html\#goalsetternode "Link to this heading")

#### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_moveit_goal_setter/index.html\#ros-parameters "Link to this heading")

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `planner_group_name` | `string` | `ur_manipulator` | Planning group that will be planned for |
| `planner_id` | `string` | `cuMotion` | Planner ID that MoveIt should use |
| `end_effector_link` | `string` | `wrist_3_link` | Name of the end-effector link |

#### ROS Services Advertised [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_moveit_goal_setter/index.html\#ros-services-advertised "Link to this heading")

| ROS Service | Interface | Description |
| --- | --- | --- |
| `set_target_pose` | [isaac\_ros\_goal\_setter\_interfaces/SetTargetPose](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_cumotion/blob/main/isaac_ros_goal_setter_interfaces/srv/SetTargetPose.srv) | Service to set the end-effector pose that MoveIt should target |

### PoseToPoseNode [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_moveit_goal_setter/index.html\#posetoposenode "Link to this heading")

#### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_moveit_goal_setter/index.html\#id1 "Link to this heading")

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `world_frame` | `string` | `base_link` | World frame to use for representing target poses |
| `target_frames` | `string array` | `['target1_frame']` | List of frames that the node should target. The node will target each frame in turn and then loop around. |
| `plan_time_period` | `float` | `0.01` | Desired period in seconds between requests sent by the node |

#### ROS Services Requested [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_moveit_goal_setter/index.html\#ros-services-requested "Link to this heading")

| ROS Service | Interface | Description |
| --- | --- | --- |
| `set_target_pose` | [isaac\_ros\_goal\_setter\_interfaces/SetTargetPose](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_cumotion/blob/main/isaac_ros_goal_setter_interfaces/srv/SetTargetPose.srv) | Sends the pose of the target\_frame with respect to the world\_frame |

### Launch Files [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_moveit_goal_setter/index.html\#launch-files "Link to this heading")

This package provides a launch file `isaac_ros_goal_setter.launch.py`.
This launch file loads the robot description of a UR Robot for the `GoalSetterNode`
and launches the `GoalSetterNode`. The arguments exposed by this launch file are:

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `ur_type` | `string` | `ur5e` | Type/series of the UR robot being used |
| `robot_ip` | `string` | `192.56.1.2` | IP address of the UR robot |

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_cumotion/isaac_ros_moveit_goal_setter/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/isaac_ros_moveit_goal_setter/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_cumotion/isaac_ros_moveit_goal_setter/index.html)