- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Nova](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html)
- `isaac_ros_correlated_timestamp_driver`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.rst.txt)

* * *

# `isaac_ros_correlated_timestamp_driver` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova/blob/main/isaac_ros_correlated_timestamp_driver).

## Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html\#overview "Link to this heading")

An autonomous robotics system needs to perceive itself, its environment, and plan its actions to achieve a task,
and control those actions, in a closed loop. Perception is provided by a combination of sensors, for example
camera, LIDAR, IMU, touch, motor positions, wheel ticks, and others. Each of these sensors operate independently
running at different frequencies and many on their own clocks. This is a challenge when more than one sensor is
needed to reconstruct an understanding of the environment for planning of actions.

![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/correlated_sensor_timeline.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/correlated_sensor_timeline.png/)

Example of 10hz LIDAR, 30Hz camera, and 100hz IMU sensors operating concurrently. [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html#id1 "Link to this image")

A common practice is to use the system time as the clock on which all sensor data is tracked in a single timeline.
When the sensor’s data is received, an interrupt occurs causing the software to record the sensor acquisition time
as the current system time. This lacks accuracy as the current system time does not account for time spent transmitting
the sensor data to the processor, or kernel delays in servicing the interrupt which records the system time. The
acquisition time of the sensor data will be both late, and subject to jitter. For some applications this lack of
accuracy in the single timeline for sensor data is a problem for a precise reconstruction of the environment and
precise planning at higher action speeds.

The `isaac_ros_correlated_timestamp_driver` provides functionality for highly accurate acquisition time of sensor
data translated to the system time. This is needed for precise reconstruction of the environment such as 3D maps, cost
maps, and obstacle avoidance from multiple concurrent sensors. This enables high action speeds as multi-sensor perception
has 2x orders of magnitude lower jitter (from 1ms to <10us).

![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/correlated_timestamp_driver_dag.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/correlated_timestamp_driver_dag.png/)

This accuracy is provided by `isaac_ros_correlated_timemestamp_driver` by leveraging hardware features in Jetson platforms.
A hardware feature uses TSC as the sensor acquisition time when an interrupt is received. This eliminates jitter in sensor
acquisition time measurements caused by delays in the kernel servicing interrupts. This is used for camera, and IMU data in Nova.
`isaac_ros_correlated_timemestamp_driver` converts TSC measurements to system time. In addition, time synchronization between
system time, and Ethernet is maintained with PTP, using `phc2sys`. Hardware provides time synchronization between TSC and PTP,
for highly precise sensor data acquisition times between camera and LIDAR sensors needed for sensor fusion in 3D reconstruction.

## Quickstart [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html\#quickstart "Link to this heading")

### Set Up Development Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html\#set-up-development-environment "Link to this heading")

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


### Build `isaac_ros_correlated_timestamp_driver` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html\#build-package-name "Link to this heading")

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
sudo apt-get install -y ros-humble-isaac-ros-correlated-timestamp-driver

```

Copy to clipboard


### Run Launch File [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html\#run-launch-file "Link to this heading")

1. Continuing inside the Docker container, launch the correlated timestamp driver:





```
ros2 launch isaac_ros_correlated_timestamp_driver correlated_timestamp_driver.launch.py

```

Copy to clipboard


### Visualize Results [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html\#visualize-results "Link to this heading")

1. Open a **new** terminal inside the Docker container:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
      ./scripts/run_dev.sh

```

Copy to clipboard

2. Output the timestamps:





```
ros2 topic echo /correlated_timestamp

```

Copy to clipboard


## Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html\#troubleshooting "Link to this heading")

### Isaac ROS Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html\#isaac-ros-troubleshooting "Link to this heading")

For solutions to problems with Isaac ROS, see [troubleshooting](https://nvidia-isaac-ros.github.io/troubleshooting/index.html).

## API [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html\#api "Link to this heading")

### Usage [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html\#usage "Link to this heading")

```
ros2 launch isaac_ros_correlated_timestamp_driver correlated_timestamp_driver.launch.py target_container:=<Target Container> nvpps_dev_file:=<NVPPS Dev Name> use_time_since_epoch:=<Use Time Since Epoch>

```

Copy to clipboard

### CorrelatedTimestampDriverNode [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html\#correlatedtimestampdrivernode "Link to this heading")

#### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html\#ros-parameters "Link to this heading")

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `use_time_since_epoch` | `boolean` | `false` | Use time since epoch. |
| `nvpps_dev_file` | `string` | `/dev/nvpps0` | NVPPS device name. |

#### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html\#ros-topics-published "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `correlated_timestamp` | [isaac\_ros\_nova\_interfaces::msg::CorrelatedTimestamp](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common/blob/main/isaac_ros_nova_interfaces/msg/CorrelatedTimestamp.msg) | Timestamp correlation data. |

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_nova/isaac_ros_correlated_timestamp_driver/index.html)