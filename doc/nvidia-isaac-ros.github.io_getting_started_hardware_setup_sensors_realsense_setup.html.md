- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Getting Started](https://nvidia-isaac-ros.github.io/getting_started/index.html)
- [Hardware Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/index.html)
- [Sensors Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/index.html)
- Isaac ROS RealSense Setup
- [View page source](https://nvidia-isaac-ros.github.io/_sources/getting_started/hardware_setup/sensors/realsense_setup.rst.txt)

* * *

# Isaac ROS RealSense Setup [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/realsense_setup.html\#isaac-ros-realsense-setup "Link to this heading")

## Camera Compatibility [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/realsense_setup.html\#camera-compatibility "Link to this heading")

| RealSense Model | Supported? |
| --- | --- |
| D455 | ✓ |
| D435i | ✓ |
| D415 | ✗ |

Note

It is required to use RealSense firmware [version 5.13.0.50](https://dev.intelrealsense.com/docs/firmware-releases),
librealsense SDK [version 2.55.1](https://github.com/IntelRealSense/librealsense/releases/tag/v2.55.1)
and realsense-ros driver [version 4.51.1-isaac](https://github.com/NVIDIA-ISAAC-ROS/realsense-ros/tree/release/4.51.1-isaac).
**Any deviation from these versions will break Isaac ROS examples.**

Note

The correct versions of the librealsense SDK and the realsense-ros driver are automatically installed in the docker container
when specifying the RealSense image key as explained in the [Setup Instructions](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/realsense_setup.html#setup-instructions).

Note

For best results we suggest increasing the maximum Linux kernel receive buffer size as
detailed [here](https://docs.ros.org/en/rolling/How-To-Guides/DDS-tuning.html#cyclone-dds-tuning).

Note

The `realsense-viewer` tool has performance issues when running multiple cameras on the Jetson platforms. However the RealSense ROS drivers work fine with multiple cameras.

## Setup Instructions [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/realsense_setup.html\#setup-instructions "Link to this heading")

Note

This tutorial assumes that you have completed the instructions in [Developer Environment Setup](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html).

1. Plug in your RealSense camera before launching the Docker container in the next step.

2. Clone `isaac_ros_common` under `${ISAAC_ROS_WS}/src`.





```
cd ${ISAAC_ROS_WS}/src && \
      git clone -b release-3.2 https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common.git isaac_ros_common

```

Copy to clipboard

3. Configure the container created by `isaac_ros_common/scripts/run_dev.sh` to include RealSense packages.
To accomplish this, create the `.isaac_ros_common-config` file in the `isaac_ros_common/scripts` directory,
then run the following to modify the `CONFIG_IMAGE_KEY` to include `librealsense SDK` and `realsense-ros`
in Isaac ROS Dev Docker:


> ```
> cd ${ISAAC_ROS_WS}/src/isaac_ros_common/scripts && \
> touch .isaac_ros_common-config && \
> echo CONFIG_IMAGE_KEY=ros2_humble.realsense > .isaac_ros_common-config
>
> ```
>
> Copy to clipboard

4. Launch the Docker container:


> ```
> cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
> ./scripts/run_dev.sh -d ${ISAAC_ROS_WS}
>
> ```
>
> Copy to clipboard
>
> This rebuilds the container image using `Dockerfile.realsense` in one of its layered stages. Rebuilding can take several minutes.

5. After the container image is rebuilt and you are inside the container, you can run `realsense-viewer` to verify that the RealSense camera is connected.


> Note
>
> The `realsense-viewer` tool requires a graphical environment, to use it you must connect the host to a monitor, use a remote desktop connection or similar.
>
> ```
> realsense-viewer
>
> ```
>
> Copy to clipboard
>
> If you turn on the “Stereo Module” in the GUI, you should see something like the following:

![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/realsense_viewer.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/realsense_viewer.png/)

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/getting_started/hardware_setup/sensors/realsense_setup.html)[latest](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/realsense_setup.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/getting_started/hardware_setup/sensors/realsense_setup.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/getting_started/hardware_setup/sensors/realsense_setup.html)