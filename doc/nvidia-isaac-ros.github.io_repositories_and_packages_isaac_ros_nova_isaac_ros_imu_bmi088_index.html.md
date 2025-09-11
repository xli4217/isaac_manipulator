- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Nova](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html)
- `isaac_ros_imu_bmi088`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.rst.txt)

* * *

# `isaac_ros_imu_bmi088` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova/blob/main/isaac_ros_imu_bmi088).

![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/bosch_bmi088.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/bosch_bmi088.png/)

## Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html\#overview "Link to this heading")

An IMU provides raw inertial data using an accelerometer and gyroscope as part of a perception for understanding of the robot and its environment.

The `isaac_ros_imu_bmi088` driver provides support for BMI088 IMU that provides a high-performance 6-axis inertial sensor that allows for highly accurate measurement of orientation and detection of motion along three orthogonal axes.

## Quickstart [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html\#quickstart "Link to this heading")

### Set Up Development Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html\#set-up-development-environment "Link to this heading")

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


### Build `isaac_ros_imu_bmi088` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html\#build-package-name "Link to this heading")

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
sudo apt-get install -y ros-humble-isaac-ros-imu-bmi088

```

Copy to clipboard


### Run Launch File [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html\#run-launch-file "Link to this heading")

1. Continuing inside the Docker container, launch the BMI088 driver:





```
ros2 launch isaac_ros_imu_bmi088 bmi088.launch.py

```

Copy to clipboard


### Visualize Results [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html\#visualize-results "Link to this heading")

1. Open a **new** terminal inside the Docker container:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
      ./scripts/run_dev.sh

```

Copy to clipboard

2. Echo the IMU data:





```
ros2 topic echo /imu/imu

```

Copy to clipboard


## Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html\#troubleshooting "Link to this heading")

### Isaac ROS Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html\#isaac-ros-troubleshooting "Link to this heading")

For solutions to problems with Isaac ROS, see [troubleshooting](https://nvidia-isaac-ros.github.io/troubleshooting/index.html).

## API [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html\#api "Link to this heading")

### Usage [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html\#usage "Link to this heading")

```
ros2 launch isaac_ros_imu_bmi088 bmi088.launch.py target_container:=<Target Container> namespace:=<Namespace> bmi_id:=<BMI ID> imu_frequency:=<IMU Frequency> nvpps_dev_file:=<NVPPS Dev Name> use_time_since_epoch:=<Use Time Since Epoch>

```

Copy to clipboard

### Bmi088Node [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html\#bmi088node "Link to this heading")

#### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html\#ros-parameters "Link to this heading")

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `imu_frequency` | `uint` | `100` | IMU(Accelerometer and Gyroscope) Update Frequency (Hz) |
| `bmi_id` | `uint` | `69` | ID selecting which BMI088 IMU to use |

#### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html\#ros-topics-subscribed "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `correlated_timestamp` | [isaac\_ros\_nova\_interfaces::msg::CorrelatedTimestamp](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common/blob/main/isaac_ros_nova_interfaces/msg/CorrelatedTimestamp.msg) | Timestamp correlation data. |

#### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html\#ros-topics-published "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `imu` | [sensor\_msgs/Imu](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Imu.msg) | The IMU data from BMI088. |

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_nova/isaac_ros_imu_bmi088/index.html)