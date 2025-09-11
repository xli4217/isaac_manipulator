- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Nova](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html)
- [`isaac_ros_data_recorder`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/index.html)
- Tutorial: Running Event Recorder with RealSense
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/tutorials/tutorial_event_recorder_realsense.rst.txt)

* * *

# Tutorial: Running Event Recorder with RealSense [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/tutorials/tutorial_event_recorder_realsense.html\#tutorial-running-event-recorder-with-realsense "Link to this heading")

## Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/tutorials/tutorial_event_recorder_realsense.html\#overview "Link to this heading")

This tutorial will demonstrate how to run the event recorder with a RealSense camera.

## Quickstart [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/tutorials/tutorial_event_recorder_realsense.html\#quickstart "Link to this heading")

### Set Up Development Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/tutorials/tutorial_event_recorder_realsense.html\#set-up-development-environment "Link to this heading")

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


### Set Up RealSense [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/tutorials/tutorial_event_recorder_realsense.html\#set-up-realsense "Link to this heading")

Follow [Isaac ROS RealSense Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/realsense_setup.html)
to set up the RealSense camera.

### Build `isaac_ros_data_recorder` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/tutorials/tutorial_event_recorder_realsense.html\#build-isaac-ros-data-recorder "Link to this heading")

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
sudo apt-get install -y ros-humble-isaac-ros-data-recorder

```

Copy to clipboard


### Run Launch File [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/tutorials/tutorial_event_recorder_realsense.html\#run-launch-file "Link to this heading")

1. Run the RealSense node:





```
ros2 launch realsense2_camera rs_launch.py

```

Copy to clipboard

2. In a separate terminal, run the event recorder node:


> ```
> ros2 launch isaac_ros_data_recorder data_recorder.launch.py event_recorder:=True \
>   topics:="[ '/rosout', '/diagnostics', '/tf', '/tf_static', '/camera/color/image_raw/compressed', '/camera/color/camera_info', '/camera/color/metadata', '/camera/imu' ]"
>
> ```
>
> Copy to clipboard
>
> Note
>
> By default, the event recorder will save 60 seconds of RealSense data in each MCAP file.
> This can be configured via the `max_bag_duration` launch argument.
>
> Warning
>
> If `max_bag_duration` is greater than `look_back_window`, then `max_bag_duration` will be reduced to `look_back_window`.

3. In a separate terminal, use the `event_start` service to signal the start of an event:





```
ros2 service call event_start isaac_ros_data_recorder/srv/Event

```

Copy to clipboard





Note



By default, the event recorder will save 60 seconds of RealSense data before the start of the event.
This can be configured via the `look_back_window` launch argument.

4. In the same terminal as Step 3, use the `event_end` service to signal the end of an event:





```
ros2 service call event_end isaac_ros_data_recorder/srv/Event

```

Copy to clipboard





Note



By default, the event recorder will save 60 seconds of RealSense data after the end of the event.
This can be configured via the `look_ahead_window` launch argument.


Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/tutorials/tutorial_event_recorder_realsense.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/tutorials/tutorial_event_recorder_realsense.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/index.html)