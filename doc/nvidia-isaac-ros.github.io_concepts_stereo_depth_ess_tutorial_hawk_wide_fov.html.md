- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS DNN Stereo Depth](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html)
- [`isaac_ros_ess`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html)
- Tutorial for ESS with Hawk Camera in Wide FoV Mode
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/stereo_depth/ess/tutorial_hawk_wide_fov.rst.txt)

* * *

# Tutorial for ESS with Hawk Camera in Wide FoV Mode [](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/ess/tutorial_hawk_wide_fov.html\#tutorial-for-ess-with-hawk-camera-in-wide-fov-mode "Link to this heading")

## Overview [](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/ess/tutorial_hawk_wide_fov.html\#overview "Link to this heading")

This tutorial demonstrates how to:

- Stream stereo images using [Hawk camera](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_argus_camera) in wide FoV mode.

- Estimate depth using [Isaac ROS ESS depth estimation](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_dnn_stereo_depth/blob/main/isaac_ros_ess).


Note

This tutorial requires an Argus-compatible stereo camera from
the list of available
[cameras](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/hawk_setup.html).

## Isaac ROS NITROS Acceleration [](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/ess/tutorial_hawk_wide_fov.html\#isaac-ros-nitros-acceleration "Link to this heading")

This package is powered by [NVIDIA Isaac Transport for ROS (NITROS)](https://developer.nvidia.com/blog/improve-perception-performance-for-ros-2-applications-with-nvidia-isaac-transport-for-ros/), which leverages type adaptation and negotiation to optimize message formats and dramatically accelerate communication between participating nodes.

ArgusStereoNode (Raw Image) with wide\_fov enabled

RectifyNode (Rectified Image)

RectifyNode (Rectified Image)

left\_crop\_node

ResizeNode (Resized Image)

ESSDisparityNode (DNN Inference)

right\_crop\_node

PointCloudNode (Point Cloud Output)

Wide FoV mode is turned on in argus\_node, which generates proper camera
info for rectification. Images are cropped to remove the black spots
then fed into the ESSDisparityNode for depth estimation.

If you have an
[Argus-compatible camera](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_argus_camera),
you can also use the launch file provided in this tutorial to start a
fully NITROS-accelerated stereo depth graph.

## Tutorial Walkthrough [](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/ess/tutorial_hawk_wide_fov.html\#tutorial-walkthrough "Link to this heading")

1. Complete the [Hawk setup tutorial](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/hawk_setup.html).

2. Complete session Prepare ESS Pre-trained Model in
[ESS Quickstart Guide](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html#quickstart).

3. Open a new terminal and launch the Docker container using the
`run_dev.sh` script:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
     ./scripts/run_dev.sh

```

Copy to clipboard

4. Build and source the workspace:





```
cd /workspaces/isaac_ros-dev && \
     colcon build --symlink-install && \
     source install/setup.bash

```

Copy to clipboard

5. Follow the session Run Launch File in
[ESS Quickstart Guide](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html#quickstart)
with wide FoV launch file below:

Run the launch file, which launches the example and waits for 10
seconds:



ESSLight ESS









```
ros2 launch isaac_ros_ess isaac_ros_argus_ess_wide_fov.launch.py \
      engine_file_path:=${ISAAC_ROS_WS:?}/isaac_ros_assets/models/dnn_stereo_disparity/dnn_stereo_disparity_v4.1.0_onnx/ess.engine \
      threshold:=0.4

```

Copy to clipboard


Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/stereo_depth/ess/tutorial_hawk_wide_fov.html)[latest](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/ess/tutorial_hawk_wide_fov.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/stereo_depth/ess/tutorial_hawk_wide_fov.html)