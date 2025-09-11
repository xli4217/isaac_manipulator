- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Getting Started](https://nvidia-isaac-ros.github.io/getting_started/index.html)
- [Hardware Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/index.html)
- [Sensors Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/index.html)
- Isaac ROS Hawk Setup
- [View page source](https://nvidia-isaac-ros.github.io/_sources/getting_started/hardware_setup/sensors/hawk_setup.rst.txt)

* * *

# Isaac ROS Hawk Setup [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/hawk_setup.html\#isaac-ros-hawk-setup "Link to this heading")

Leopard Imaging’s [Hawk](https://leopardimaging.com/leopard-imaging-hawk-stereo-camera/) camera is a popular choice of stereo camera for use with the Isaac ROS ecosystem of packages. This document details important setup instructions for configuring the Hawk camera with Isaac ROS.

Note

For best results, you must update the camera’s firmware.

Note

Hawk stereo cameras through Argus are only supported on Jetson platforms

## Jetson Platforms [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/hawk_setup.html\#jetson-platforms "Link to this heading")

### Camera Configuration [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/hawk_setup.html\#camera-configuration "Link to this heading")

The Hawk camera must be connected to your Jetson via a GMSL expansion board. If using [Leopard Imaging’s P3762\_A03 board](https://leopardimaging.com/product/accessories/adapters-carrier-boards/for-nvidia-jetson/li-jag-adp-gmsl2-8ch/) you can set up Hawk by [installing Nova Init](https://nvidia-isaac-ros.github.io/nova/nova_init/index.html#install).

For other GMSL boards please contact your vendor for information on how to configure them.

### Verifying Hawk Set Up [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/hawk_setup.html\#verifying-hawk-set-up "Link to this heading")

Hawk can be verified with Argus by building and running `argus_syncstereo`:

```
cd /usr/src/jetson_multimedia_api/argus/cmake/
sudo cmake ..
sudo make install
argus_syncstereo

```

Copy to clipboard

This should run without error.

## ROS Driver Setup [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/hawk_setup.html\#ros-driver-setup "Link to this heading")

1. Clone the `isaac_ros_argus_camera` repositories that contains the Hawk camera ROS driver:


> ```
> cd ${ISAAC_ROS_WS}/src && \
> git clone -b release-3.2 https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_argus_camera.git isaac_ros_argus_camera
>
> ```
>
> Copy to clipboard

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/getting_started/hardware_setup/sensors/hawk_setup.html)[latest](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/hawk_setup.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/getting_started/hardware_setup/sensors/hawk_setup.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/getting_started/hardware_setup/sensors/hawk_setup.html)