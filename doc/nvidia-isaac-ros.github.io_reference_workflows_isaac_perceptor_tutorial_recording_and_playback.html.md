- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Reference Workflows](https://nvidia-isaac-ros.github.io/reference_workflows/index.html)
- [Isaac Perceptor](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/index.html)
- Tutorial: Recording and Playing Back Data for Isaac Perceptor
- [View page source](https://nvidia-isaac-ros.github.io/_sources/reference_workflows/isaac_perceptor/tutorial_recording_and_playback.rst.txt)

* * *

# Tutorial: Recording and Playing Back Data for Isaac Perceptor [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorial_recording_and_playback.html\#tutorial-recording-and-playing-back-data-for-isaac-perceptor "Link to this heading")

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_perceptor/perceptor_in_zanker.gif/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_perceptor/perceptor_in_zanker.gif/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_perceptor/perceptor_in_zanker.gif/)

Perceptor running on data recorded on Nova Carter operating in a warehouse. [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorial_recording_and_playback.html#id1 "Link to this image")

This tutorial guides you through the steps to record and playback data for Isaac Perceptor.

> Note
>
> You must run on a hardware platform equipped with Nova Orin Developer Kit to record data
> compatible with Isaac Perceptor.

## Recording Data for Isaac Perceptor [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorial_recording_and_playback.html\#recording-data-for-isaac-perceptor "Link to this heading")

To record data for use in the Running from a ROSbag tutorial below, we use
the [isaac\_ros\_nova\_recorder](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html),
which is part of [Isaac ROS Nova](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova) repository.

To generate the required data:

1\. Follow the installation instructions in
[Isaac ROS Nova Recorder Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html#quickstart).

2\. Please use [hawk-3.yaml](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova/blob/main/isaac_ros_nova/config/hawk-3.yaml) configuration for recording data with the Nova Orin Developer.
This configures the recorder to record only the front, left, and right Hawk stereo cameras.

> ```
> ros2 launch isaac_ros_nova_recorder nova_recorder.launch.py config:=hawk-3
>
> ```
>
> Copy to clipboard

Use [nova-carter\_hawk-4.yaml](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova/blob/main/isaac_ros_nova/config/nova-carter_hawk-4.yaml) for recording data with the Nova Carter.
This configures the recorder to record the front, left, right, and back Hawk stereo cameras.

> ```
> ros2 launch isaac_ros_nova_recorder nova_recorder.launch.py config:=nova-carter_hawk-4
>
> ```
>
> Copy to clipboard

3. Follow steps 2 through 6 from [Run Launch File](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html#isaac-ros-nova-recorder-launch).


By default, the resulting data is saved under `/mnt/nova_ssd/recordings`. It can be used to
generate a reconstruction using the Running from a ROSbag tutorial described below.

## Running from a ROSbag [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorial_recording_and_playback.html\#running-from-a-rosbag "Link to this heading")

### Downloading Pre-recorded Data (Optional) [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorial_recording_and_playback.html\#downloading-pre-recorded-data-optional "Link to this heading")

You can run Isaac Perceptor on the a pre-recorded ROSbag from Nova Carter.

1. Download `r2b_galileo` dataset from the [r2b 2024 dataset on NGC](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/isaac/resources/r2bdataset2024).

2. Place the dataset at `$ISAAC_ROS_WS/isaac_ros_assets/r2b_2024/r2b_galileo`.


### Launching Isaac Perceptor [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorial_recording_and_playback.html\#launching-isaac-perceptor "Link to this heading")

Note

Complete either
[Tutorial: Running Camera-based 3D Perception with Isaac Perceptor on Nova Orin Developer Kit](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorials_on_devkit/demo_perceptor.html)
or
[Tutorial: Running Camera-based 3D Perception with Isaac Perceptor on Nova Carter](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorials_on_carter/demo_perceptor.html).
You must have set up the prerequisites, launched Isaac Perceptor application,
and obtained object detection and visual odometry visualizations.

If you want to run the Isaac Perceptor from a ROSbag (rather than streaming from sensors),
you may use the `rosbag` argument.

If your ROSbag is recorded on Nova Orin Developer Kit, you can launch the app as:

```
ros2 launch nova_developer_kit_bringup perceptor.launch.py mode:=rosbag rosbag:=<YOUR_ROSBAG_PATH>

```

Copy to clipboard

If your ROSbag is recorded on Nova Carter, you can launch the app as:

```
ros2 launch nova_carter_bringup perceptor.launch.py mode:=rosbag rosbag:=<YOUR_ROSBAG_PATH>

```

Copy to clipboard

If you are using the pre-recorded ROSbag, you can run:

```
ros2 launch nova_carter_bringup perceptor.launch.py \
    stereo_camera_configuration:=front_left_right_configuration \
    mode:=rosbag \
    rosbag:=$ISAAC_ROS_WS/isaac_ros_assets/r2b_2024/r2b_galileo

```

Copy to clipboard

### Visualizing the Outputs [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorial_recording_and_playback.html\#visualizing-the-outputs "Link to this heading")

Proceed to the Foxglove studio to visualize sensor outputs and mesh of surround environments.
If your ROSbag is recorded on Nova Orin Developer Kit, follow similar steps in
[Visualizing the Outputs from Isaac Perceptor on Nova Orin Developer Kit](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorials_on_devkit/demo_perceptor.html#visualizing-the-outputs).
If your ROSbag is recorded on Nova Carter, follow
[Visualizing the Outputs from Isaac Perceptor on Nova Carter](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorials_on_carter/demo_perceptor.html#visualizing-the-outputs).

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/reference_workflows/isaac_perceptor/tutorial_recording_and_playback.html)[latest](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorial_recording_and_playback.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/index.html)