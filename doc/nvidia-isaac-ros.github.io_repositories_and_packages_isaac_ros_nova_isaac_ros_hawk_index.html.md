- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Nova](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html)
- `isaac_ros_hawk`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.rst.txt)

* * *

# `isaac_ros_hawk` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova/blob/main/isaac_ros_hawk).

![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/li_hawk_ar0234cs-stereo.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/li_hawk_ar0234cs-stereo.png/)

## Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html\#overview "Link to this heading")

A camera provides images using an electronic image sensor as part of perception for understanding of the robot and its environment. A stereo camera provides images from two slightly different perspectives which can be used to compute precise depth.

The `isaac_ros_hawk` driver provides support for the `LI-AR0234CS-STEREO-GMSL2-30` stereo camera with IMU. This provides an wide-angle (>120 degree) field of view with two global shutter HD (1920x1200) color imagers. Global shutter enables light sensing in the imager for all pixels simultaneously, reducing motion artifacts caused by rolling-shutter imagers which capture a row of pixels at a time. The stereo imagers are time-synchronized to capture simultaneously allowing for computation of precise depth. This design has the benefit that the same imager can be used to provide depth and color information for AI-based perception functions, where other designs have parallax errors as depth and color come from different sensors reducing accuracy for AI-based perception. `LI-AR0234CS-STEREO-GMSL2-30` is jointly developed with Leopard Imaging and NVIDIA.

## Quickstart [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html\#quickstart "Link to this heading")

### Set Up Nova [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html\#set-up-nova "Link to this heading")

1. Verify if Nova Orin software is the latest version by comparing the installed package with the candidate:





```
$ apt policy nova-orin-init
nova-orin-init:
     Installed: 1.3.2+b4
     Candidate: 1.3.2+b4
     Version table:
    *** 1.3.2+b4 600
           600 https://isaac.download.nvidia.com/nova-init jammy/main arm64 Packages

```

Copy to clipboard

2. If the installed version is lower than the candidate, follow [the nova init upgrade process here](https://nvidia-isaac-ros.github.io/nova/nova_init/index.html#upgrade). Otherwise proceed to the next step.

3. Verify that sensors are functioning by running:





```
nova_preflight_checker

```

Copy to clipboard



It is expected for all tests to pass. Refer to the [Nova PFC documentation](https://nvidia-isaac-ros.github.io/nova/tools/pfc.html) for more details.


### Set Up Development Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html\#set-up-development-environment "Link to this heading")

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


4. Create a file called `.isaac_ros_dev-dockerargs` with the following:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common/scripts
echo -e "-v /etc/nova/:/etc/nova/" > .isaac_ros_dev-dockerargs

```

Copy to clipboard


### Build `isaac_ros_hawk` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html\#build-package-name "Link to this heading")

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
sudo apt-get install -y ros-humble-isaac-ros-hawk

```

Copy to clipboard


### Run Launch File [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html\#run-launch-file "Link to this heading")

1. Continuing inside the Docker container, run the following launch file to spin up a demo of this package:





```
cd ${ISAAC_ROS_WS} && \
     ros2 launch isaac_ros_hawk hawk.launch.py

```

Copy to clipboard


### Visualize Results [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html\#visualize-results "Link to this heading")

1. Open a **new** terminal inside the Docker container:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
      ./scripts/run_dev.sh

```

Copy to clipboard

2. Visualize and validate the output of the package using `rviz`:
![Rviz showing hawk images](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/hawk_raw.png/)

## API [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html\#api "Link to this heading")

### Usage [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html\#usage "Link to this heading")

```
ros2 launch isaac_ros_hawk hawk.launch.py target_container:=<Target Container> namespace:=<Namespace> module_id:=<Index specifying the camera module to use.> mode:=<Supported Resolution mode from the camera. For example, 0: 1920 x 1200> fsync_type:=<Specifies what kind of Frame Synchronization to use. Supported values are: 0 for internal, 1 for external.> wide_fov:=<Specifies FoV mode. Supported values are: 0 normal, 1 for wide.> nvpps_dev_file:=<NVPPS Dev Name> use_time_since_epoch:=<Use Time Since Epoch>

```

Copy to clipboard

### HawkNode [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html\#hawknode "Link to this heading")

#### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html\#ros-parameters "Link to this heading")

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `camera_id` | `uint` | `0` | The video device index E.g. `/dev/video0` |
| `module_id` | `uint` | `0` | The camera module index in the device tree when there is more than one of the same camera module connected |
| `mode` | `uint` | `0` | The resolution mode supported by the camera sensor and driver. Currently only 1920x1200 (mode 0) is supported. |
| `fsync_type` | `uint` | `1` | Specifies what kind of Frame Synchronization to use, supported values are 0 for internal and 1 for external. For e3653 boards (carter 2.3) choose 0, for p3762 boards (carter 2.4) choose 1 |
| `camera_type` | `uint` | `0` | 0 for Monocular type camera. |
| `camera_link_frame_name` | `string` | `camera` | The frame name associated with the origin of the camera body. |
| `left_camera_frame_name` | `string` | `left_cam` | The frame name associated with the left imager inside camera body. |
| `right_camera_frame_name` | `string` | `right_cam` | The frame name associated with the right imager inside camera body. |
| `left_camera_info_url` | `string` | N/A | Optional URL of a camera info `.yaml` file to read intrinsic information. |
| `right_camera_info_url` | `string` | N/A | Optional URL of a camera info `.yaml` file to read intrinsic information. |

#### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html\#ros-topics-subscribed "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `correlated_timestamp` | [isaac\_ros\_nova\_interfaces::msg::CorrelatedTimestamp](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common/blob/main/isaac_ros_nova_interfaces/msg/CorrelatedTimestamp.msg) | Timestamp correlation data. |

#### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html\#ros-topics-published "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `left/image_raw` | [sensor\_msgs/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | The left image of a stereo pair. |
| `left/camera_info` | [sensor\_msgs/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | The left camera model. |
| `right/image_raw` | [sensor\_msgs/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | The right image of a stereo pair. |
| `right/camera_info` | [sensor\_msgs/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | The right camera model. |

## Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html\#troubleshooting "Link to this heading")

### Restarting the Argus Daemon [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html\#restarting-the-argus-daemon "Link to this heading")

[Argus](https://docs.nvidia.com/jetson/l4t-multimedia/group__LibargusAPI.html) is a library used by
Isaac ROS on NVIDIA Jetson systems. It runs a daemon independent of ROS in a separate process to manage the camera.
If you are having any sort of trouble with the camera, one of the first things you should try is restarting this daemon. From outside the container you can run:

```
sudo systemctl restart nvargus-daemon.service

```

Copy to clipboard

This can resolve `(Argus) Error EndOfFile` and `Failed to create capture session` errors, among others.
Generally these errors are caused by improper shutdown of a camera application.

### Re-probing camera modules [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html\#re-probing-camera-modules "Link to this heading")

If you are still having trouble with the camera, for example if restarting the daemon does not resolve a `Failed to create capture session` error, you can try re-probing the camera modules.
This can be done from outside the container by running the following

```
sudo modprobe -r cam_cdi_tsc
sudo modprobe -r nv_hawk_owl
sudo modprobe nv_hawk_owl
sudo modprobe cam_cdi_tsc
sudo systemctl restart nvargus-daemon.service

```

Copy to clipboard

If you want to restart the daemon from a script and from within the container, there is service installed with nova-init that will listen on a socket to restart
the daemon. You can use the following python snippet in your launch files or elsewhere to achieve this:

```
import socket
s=socket.socket(socket.AF_UNIX)
s.connect('/tmp/argus_restart_socket')
s.send(b'RESTART_SERVICE'); s.close()

```

Copy to clipboard

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_nova/isaac_ros_hawk/index.html)