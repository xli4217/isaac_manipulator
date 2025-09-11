- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Getting Started](https://nvidia-isaac-ros.github.io/getting_started/index.html)
- Developer Environment Setup
- [View page source](https://nvidia-isaac-ros.github.io/_sources/getting_started/dev_env_setup.rst.txt)

* * *

# Developer Environment Setup [](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html\#developer-environment-setup "Link to this heading")

The development flow currently supported by Isaac ROS is to build on your target platform. You can setup ROS 2 Humble in your host machine with the [Isaac Apt Repository](https://nvidia-isaac-ros.github.io/getting_started/isaac_apt_repository.html) and setup dependencies with `rosdep`
OR you can use a Docker-based development environment through [Isaac ROS Dev](https://nvidia-isaac-ros.github.io/concepts/docker_devenv/index.html#development-environment).

We **strongly** recommend that you set up your [developer environment](https://nvidia-isaac-ros.github.io/concepts/docker_devenv/index.html#development-environment) with Isaac ROS Dev.
This will streamline your development environment setup with the correct versions of dependencies on both Jetson and x86\_64 platforms.
Working within the Isaac ROS Dev Docker containers will also automatically give you access to our [Isaac Apt Repository](https://nvidia-isaac-ros.github.io/getting_started/isaac_apt_repository.html).

Note

All Isaac ROS Quickstarts, tutorials, and examples have been designed with the **Isaac ROS Dev Docker images** as a prerequisite.

For more information on the Isaac ROS Dev Docker-based development environment and how to customize it for your needs, check out this [guide](https://nvidia-isaac-ros.github.io/concepts/docker_devenv/index.html#development-environment).

Note

Before you begin, verify that you have sufficient storage
space available on your device. We recommend at least **30 GB**, to
account for the size of the container and datasets.

On Jetson platforms, NVMe SSD storage is **required** for
sufficient and fast storage. See [here](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/jetson_storage.html)

Setting up Jetson for using VPI. See [here](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/jetson_vpi.html)

1. Docker configuration:



On Jetson platformsOn x86\_64 platforms





Follow [this instruction](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/jetson_storage.html)
to first set your Jetson up with SSD, then come back to this document and resume from Step 2.

2. Restart Docker:





```
sudo systemctl daemon-reload && sudo systemctl restart docker

```

Copy to clipboard

3. Install [Git LFS](https://git-lfs.github.com/) to pull
down all large files:





```
sudo apt-get install git-lfs

```

Copy to clipboard







```
git lfs install --skip-repo

```

Copy to clipboard

4. Create a ROS 2 workspace for experimenting with Isaac ROS:



Jetson with SSDx86\_64 and Jetson without SSD









```
mkdir -p  /mnt/nova_ssd/workspaces/isaac_ros-dev/src
echo "export ISAAC_ROS_WS=/mnt/nova_ssd/workspaces/isaac_ros-dev/" >> ~/.bashrc
source ~/.bashrc

```

Copy to clipboard







We expect to use the `ISAAC_ROS_WS` environmental variable
to refer to this ROS 2 workspace directory, in the future.


Note

To further customize your development environment, check out [this guide](https://nvidia-isaac-ros.github.io/concepts/docker_devenv/index.html#development-environment).

Note

To enable building sensor drivers for RealSense, ZED, etc, please follow [this guide](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/index.html). This needs a modification to the parameters for `run_dev.sh` script.

## Troubleshooting [](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html\#troubleshooting "Link to this heading")

Check out our [troubleshooting](https://nvidia-isaac-ros.github.io/troubleshooting/dev_env.html) section for issues with setting up your development environment.

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/getting_started/dev_env_setup.html)[latest](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/getting_started/dev_env_setup.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/getting_started/dev_env_setup.html)