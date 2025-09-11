- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS NITROS](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/index.html)
- [Isaac ROS NITROS Bridge](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_bridge/index.html)
- Create a ROS Noetic Container with ROS 1 Bridge
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/nitros_bridge/setup_ros1_docker.rst.txt)

* * *

# Create a ROS Noetic Container with ROS 1 Bridge [](https://nvidia-isaac-ros.github.io/concepts/nitros_bridge/setup_ros1_docker.html\#create-a-ros-noetic-container-with-ros-1-bridge "Link to this heading")

1. Clone the `isaac_ros_common` repository and `ros1_bridge` under ROS 1 workspace:





```
mkdir -p ~/workspaces/ros1_ws/isaac_ros-dev/src && \
    cd ~/workspaces/ros1_ws/isaac_ros-dev/src && \
    git clone -b release-3.2 https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common.git isaac_ros_common && \
    git clone https://github.com/ros2/ros1_bridge

```

Copy to clipboard

2. Clone the `isaac_ros_noetic_interfaces` and `isaac_ros_nitros_bridge` repositories under the ROS 1 workspace:





```
mkdir -p ~/workspaces/ros1_ws/isaac_ros_1-dev/src && \
    cd ~/workspaces/ros1_ws/isaac_ros_1-dev/src && \
    git clone -b release-3.2 https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_noetic_interfaces.git isaac_ros_noetic_interfaces && \
    git clone -b release-3.2 https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros.git isaac_ros_nitros

```

Copy to clipboard





Note



The ROS 1 packages within `isaac_ros_nitros_bridge` are excluded by `COLCON_IGNORE`. In this Dockerfile, `COLCON_IGNORE` is automatically removed
to build ROS 1 workspace. If you intend to build these packages in your own environment, ensure that you remove the `COLCON_IGNORE` file first.

3. Build the `Noetic` Docker:





```
cd ~/workspaces && \
    docker build --build-arg USER_UID=$(id -u) --build-arg USER_GID=$(id -g) -t nitros_bridge -f ros1_ws/isaac_ros_1-dev/src/isaac_ros_nitros/isaac_ros_nitros_bridge/docker/Dockerfile.ros1_noetic .

```

Copy to clipboard


Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/nitros_bridge/setup_ros1_docker.html)[latest](https://nvidia-isaac-ros.github.io/concepts/nitros_bridge/setup_ros1_docker.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/nitros_bridge/setup_ros1_docker.html)