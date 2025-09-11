- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Benchmark](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/index.html)
- `isaac_ros_benchmark`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_benchmark/isaac_ros_benchmark/index.rst.txt)

* * *

# `isaac_ros_benchmark` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/isaac_ros_benchmark/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_benchmark/blob/main/isaac_ros_benchmark).

## Quickstart [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/isaac_ros_benchmark/index.html\#quickstart "Link to this heading")

Follow the steps below to run a sample benchmark for measuring
performance of an Isaac ROS AprilTag node with `ros2_benchmark`. This
process can also be used to benchmark the other Isaac ROS nodes, and the
`ros2_benchmark` framework more generally supports benchmarking
arbitrary graphs of ROS 2 nodes.

### Set Up Development Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/isaac_ros_benchmark/index.html\#set-up-development-environment "Link to this heading")

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


### Datasets [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/isaac_ros_benchmark/index.html\#datasets "Link to this heading")

Most benchmarks for Isaac ROS nodes use the standard `r2b Dataset` as input data.
Download the datasets by following the instructions [here](https://github.com/NVIDIA-ISAAC-ROS/ros2_benchmark/blob/main/README.md#datasets)
or fetch just the rosbag used in this Quickstart with the following command.

```
mkdir -p ${ISAAC_ROS_WS}/src/ros2_benchmark/assets/datasets/r2b_dataset/r2b_storage && \
cd ${ISAAC_ROS_WS}/src/ros2_benchmark/assets/datasets/r2b_dataset/r2b_storage && \
wget 'https://api.ngc.nvidia.com/v2/resources/nvidia/isaac/r2bdataset2023/versions/2/files/r2b_storage/metadata.yaml' && \
wget 'https://api.ngc.nvidia.com/v2/resources/nvidia/isaac/r2bdataset2023/versions/2/files/r2b_storage/r2b_storage_0.db3'

```

Copy to clipboard

### Build `isaac_ros_benchmark` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/isaac_ros_benchmark/index.html\#build-package-name "Link to this heading")

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
sudo apt-get install -y ros-humble-isaac-ros-apriltag-benchmark

```

Copy to clipboard


### Run Launch File [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/isaac_ros_benchmark/index.html\#run-launch-file "Link to this heading")

1. Continuing inside the Docker container, run the following launch file to start benchmarking Isaac ROS AprilTag:





```
launch_test $(ros2 pkg prefix isaac_ros_apriltag_benchmark)/share/isaac_ros_apriltag_benchmark/scripts/isaac_ros_apriltag_node.py

```

Copy to clipboard

2. Once the benchmark is finished, the final performance measurements
are displayed in the terminal.

Additionally, the final results and benchmark metadata (e.g., system
information, benchmark configurations) are also exported as a JSON
file whose path is printed in the terminal when the benchmark ends.


## Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/isaac_ros_benchmark/index.html\#troubleshooting "Link to this heading")

For solutions to problems with Isaac ROS, please check [here](https://nvidia-isaac-ros.github.io/troubleshooting/index.html).

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_benchmark/isaac_ros_benchmark/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_benchmark/isaac_ros_benchmark/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_benchmark/isaac_ros_benchmark/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_benchmark/isaac_ros_benchmark/index.html)