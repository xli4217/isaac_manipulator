- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Getting Started](https://nvidia-isaac-ros.github.io/getting_started/index.html)
- [Hardware Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/index.html)
- [Sensors Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/index.html)
- Isaac ROS ZED Setup
- [View page source](https://nvidia-isaac-ros.github.io/_sources/getting_started/hardware_setup/sensors/zed_setup.rst.txt)

* * *

# Isaac ROS ZED Setup [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/zed_setup.html\#isaac-ros-zed-setup "Link to this heading")

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/zed_parts.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/zed_parts.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/zed_parts.png/)

ZED cameras require the following to be able to publish data to ROS 2 topics:

1. ZED SDK (Step 5 of Setup Instructions)

2. `zed-ros2-wrapper` (Cloned in Step 1 of Setup Instructions)

3. ZED X driver (Installed by user on host machine in Step 3 of Setup Instructions)


## Camera Compatibility [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/zed_setup.html\#camera-compatibility "Link to this heading")

All cameras supported by the [zed ros2 wrapper](https://github.com/stereolabs/zed-ros2-wrapper) work with Isaac ROS.

| ZED Model | SQA Testing? |
| --- | --- |
| ZED 2i | ✓ |
| ZED X | ✓ |
| ZED 2 | ✗ |
| ZED | ✗ |
| ZED Mini | ✗ |
| ZED X Mini | ✗ |

## Setup Instructions [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/zed_setup.html\#setup-instructions "Link to this heading")

Note

This tutorial assumes that you have set up your development environment by following the instructions [here](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html).

1. Clone the `zed-ros2-wrapper` repository on the `master` branch:


> ```
> cd ${ISAAC_ROS_WS}/src && \
> git clone --recurse-submodules https://github.com/stereolabs/zed-ros2-wrapper -b humble-v4.2.5
>
> ```
>
> Copy to clipboard

2. If you are using ZED X or ZED X Mini refer to the appropriate [stereolabs setup guide](https://www.stereolabs.com/docs/get-started-with-zed-x/).


> Note
>
> You do **not** need to install the `ZED SDK` because this is done in Step 5 of Setup Instructions.
>
> Note
>
> You must install the `ZED driver`.

3. Plug in the USB cable of your ZED camera before launching the Docker container in the next step.

4. Launch the Docker container.


> ```
> cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
> ./scripts/run_dev.sh
>
> ```
>
> Copy to clipboard

5. Install the `ZED SDK`.


> Note
>
> Use `install-zed-x86_64.sh` below if you are running on X86.
>
> ```
> sudo chmod +x ${ISAAC_ROS_WS}/src/isaac_ros_common/docker/scripts/install-zed-aarch64.sh && \
> ${ISAAC_ROS_WS}/src/isaac_ros_common/docker/scripts/install-zed-aarch64.sh
>
> ```
>
> Copy to clipboard

6. Install the dependencies for the `zed_wrapper` package and build it:


> ```
> cd ${ISAAC_ROS_WS} && \
> sudo apt update && \
> rosdep update && rosdep install --from-paths src/zed-ros2-wrapper --ignore-src -r -y && \
> colcon build --symlink-install --packages-up-to zed_wrapper
>
> ```
>
> Copy to clipboard

7. After the container image is rebuilt and you are inside the container, you can run `/usr/local/zed/tools/ZED_Explorer` to check that the ZED camera is connected.


> ```
> /usr/local/zed/tools/ZED_Explorer
>
> ```
>
> Copy to clipboard
>
> If everything is working as expected, you should see something similar to the following:
>
> ![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/zed_explorer.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/zed_explorer.png/)

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/getting_started/hardware_setup/sensors/zed_setup.html)[latest](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/zed_setup.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/getting_started/hardware_setup/sensors/zed_setup.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/getting_started/hardware_setup/sensors/zed_setup.html)