- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Concepts](https://nvidia-isaac-ros.github.io/concepts/index.html)
- [Stereo Depth](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/index.html)
- Bi3D
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/stereo_depth/bi3d/index.rst.txt)

* * *

# Bi3D [](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/bi3d/index.html\#bi3d "Link to this heading")

The [Bi3D DNN model](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/isaac/models/bi3d_proximity_segmentation)
performs stereo-depth estimation using binary classification for depth segmentation. Depth segmentation can be used to
determine whether an obstacle is within a proximity field and to avoid
collisions with obstacles during navigation.

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_depth_segmentation/isaac_ros_bi3d_nodegraph.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_depth_segmentation/isaac_ros_bi3d_nodegraph.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_depth_segmentation/isaac_ros_bi3d_nodegraph.png/)

[Bi3D](https://arxiv.org/abs/2005.07274) is used in a graph of nodes
to provide depth segmentation from a time-synchronized input left
and right stereo image pair. Images to Bi3D need to be rectified and
resized to the appropriate input resolution. The aspect ratio of the
image needs to be maintained. A crop and resize might be required
to maintain the input aspect ratio. The graph for DNN encode, to DNN
inference, to DNN decode is part of the Bi3D node. Inference is
performed using TensorRT, because the Bi3D DNN model is designed to use
optimizations supported by TensorRT.

Compared to other stereo disparity functions, depth segmentation
provides a prediction of whether an obstacle is within a proximity
field, while simultaneously predicting
freespace from the ground plane. Also unlike other stereo disparity functions in Isaac ROS,
depth segmentation runs on NVIDIA DLA (deep learning accelerator),
which is separate and independent from the GPU. For more information on
disparity, refer to [Binocular disparity](https://en.wikipedia.org/wiki/Binocular_disparity).

## Repositories and Packages [](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/bi3d/index.html\#repositories-and-packages "Link to this heading")

The Isaac ROS implementations of this technology are available here:

- [Isaac ROS Depth Segmentation using Bi3D](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_depth_segmentation/isaac_ros_bi3d/index.html)


Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/stereo_depth/bi3d/index.html)[latest](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/bi3d/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/concepts/stereo_depth/bi3d/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/stereo_depth/bi3d/index.html)