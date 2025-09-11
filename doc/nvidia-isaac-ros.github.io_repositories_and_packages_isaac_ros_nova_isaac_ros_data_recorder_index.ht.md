- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Nova](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html)
- `isaac_ros_data_recorder`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/index.rst.txt)

* * *

# `isaac_ros_data_recorder` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova/blob/main/isaac_ros_data_recorder).

## Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/index.html\#overview "Link to this heading")

The `isaac_ros_data_recorder` package enables recording data in Isaac ROS. Data is recorded as an
[MCAP](https://mcap.dev/) file with messages serialized in CDR format. Camera streams are
encoded in [H.264](https://en.wikipedia.org/wiki/Advanced_Video_Coding) to reduce storage
footprint. For more information on recording data in ROS 2, refer to the documentation for
[rosbag2](https://github.com/ros2/rosbag2).

## Tutorials [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/index.html\#tutorials "Link to this heading")

- [Tutorial: Running Event Recorder with RealSense](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/tutorials/tutorial_event_recorder_realsense.html)

## API [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/index.html\#api "Link to this heading")

### Usage [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/index.html\#usage "Link to this heading")

```
ros2 launch isaac_ros_data_recorder data_recorder.launch.py

```

Copy to clipboard

#### ROS Launch Arguments [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/index.html\#ros-launch-arguments "Link to this heading")

| ROS Launch Argument | Default Value | Description |
| --- | --- | --- |
| `sensors` | `{}` | Sensor recording configuration. |
| `topics` | `['--all']` | Additional topics to record. |
| `files` | `['']` | Files to record. |
| `recording_directory` | `.` | Recording directory. |
| `recording_name` | `rosbag2` | Recording name. |
| `encoder_qp` | `20` | H.264 encoder quality parameter, 0-50, higher values mean lower quality. |
| `event_recorder` | `False` | Enable event recording. |
| `max_bag_duration` | `60` | Event recorder max bag duration in seconds. |
| `look_back_window` | `60` | Event recorder look-back window duration in seconds. |
| `look_ahead_window` | `60` | Event recorder look-ahead window duration in seconds. |

Note

`max_bag_duration`, `look_back_window`, and `look_ahead_window` only apply when `event_recorder` is `True`.

#### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/index.html\#ros-topics-published "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `/rosbag2/recording_info` | [isaac\_ros\_data\_recorder/RecordingInfo](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova) | Recording information. |

Note

`/rosbag2/recording_info` is only available when `event_recorder` is `False`.

#### ROS Services Advertised [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/index.html\#ros-services-advertised "Link to this heading")

| ROS Service | Interface | Description |
| --- | --- | --- |
| `rosbag2/start_recording` | [isaac\_ros\_data\_recorder/StartRecording](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova) | Starts a new recording. |
| `rosbag2/stop_recording` | [isaac\_ros\_data\_recorder/StopRecording](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova) | Stops the current recording. |
| `event_start` | [isaac\_ros\_data\_recorder/Event](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova) | Signals the start of an event. |
| `event_end` | [isaac\_ros\_data\_recorder/Event](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova) | Signals the end of an event. |

Note

`rosbag2/start_recording` and `rosbag2/stop_recording` are only available when `event_recorder` is `False`.

Note

`event_start` and `event_end` are only available when `event_recorder` is `True`.

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/index.html)