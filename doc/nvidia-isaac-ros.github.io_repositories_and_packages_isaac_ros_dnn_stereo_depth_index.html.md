- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- Isaac ROS DNN Stereo Depth
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.rst.txt)

* * *

# Isaac ROS DNN Stereo Depth [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html\#repo-name "Link to this heading")

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_dnn_stereo_depth/ess4.1_config0.0_galileo.gif/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_dnn_stereo_depth/ess4.1_config0.0_galileo.gif/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_dnn_stereo_depth/ess4.1_config0.0_galileo.gif/)

## Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html\#overview "Link to this heading")

The vision depth perception problem is generally useful in many fields of robotics such as estimating
the pose of a robotic arm in an object manipulation task, estimating distance of static or moving targets
in autonomous robot navigation, tracking targets in delivery robots and so on.
[Isaac ROS DNN Stereo Depth](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_dnn_stereo_depth) is targeted at two Isaac applications,
Isaac Manipulator and Isaac Perceptor. In Isaac Manipulator application, ESS is deployed in
[Isaac ROS cuMotion](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_cumotion/index.html)
package as a plug-in node to provide depth perception maps for robot arm motion planning and control.
In this scenario, multi-camera stereo streams of industrial robot arms on a table task are passed to ESS to
obtain corresponding depth streams. The depth streams are used to segment the relative distance of robot arms from
corresponding objects on the table; thus providing signals for collision avoidance and fine-grain control.
Similarly, the Isaac Perceptor application uses several Isaac ROS packages, namely,
[Isaac ROS Nova](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html),
[Isaac ROS Visual Slam](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_visual_slam/index.html),
[Isaac ROS Stereo Depth (ESS)](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html#),
[Isaac ROS Nvblox](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/index.html)
and [Isaac ROS Image Pipeline](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/index.html).

ESS is deployed in Isaac Perceptor to enable Nvblox to create
3D voxelized images of the robot surroundings. Specifically, the Nova developer suite provides 3x stereo-camera
streams to Isaac Perceptor. Each stream corresponds to the front, left, and right cameras.
In both Isaac Manipulator and Isaac Perceptor, a camera-specific image processing pipeline consisting of
GPU-accelerated operations, provides rectification and undistortion of the input stereo images. All stereo stream image
pair are time synchronized before before passing them to ESS. ESS node outputs corresponding depth maps for all three
preprocessed image streams and combines the depth images with motion signals provided by cuVSLAM module.
The combined depth and motion integrated signals are fed to Nvblox module to produce a dense 3D volumetric scene
reconstruction of the surrounding scene.

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess_nodegraph.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess_nodegraph.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess_nodegraph.png/)

Above, ESS node is used in a graph of nodes to provide a disparity prediction from an input left and right stereo image pair.
The rectify and resize nodes pre-process the left and right frames to the appropriate resolution.
The aspect ratio of the image is recommended to be maintained to avoid degrading the depth output quality.
The graph for DNN encode, DNN inference, and DNN decode is included in the ESS node. Inference is performed using
TensorRT, as the ESS DNN model is designed with optimizations supported by TensorRT.

## Quickstarts [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html\#quickstarts "Link to this heading")

- [Isaac ROS ESS](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html#quickstart)


## Isaac ROS NITROS Acceleration [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html\#isaac-ros-nitros-acceleration "Link to this heading")

This package is powered by [NVIDIA Isaac Transport for ROS (NITROS)](https://developer.nvidia.com/blog/improve-perception-performance-for-ros-2-applications-with-nvidia-isaac-transport-for-ros/), which leverages type adaptation and negotiation to optimize message formats and dramatically accelerate communication between participating nodes.

## ESS DNN [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html\#ess-dnn "Link to this heading")

[ESS](https://arxiv.org/pdf/1803.09719.pdf) stands for Efficient Semi-Supervised stereo disparity, developed by NVIDIA.
The ESS DNN is used to predict the disparity for each pixel from stereo camera image pairs.
This network has improvements over classic CV approaches that use epipolar geometry to compute disparity,
as the DNN can learn to predict disparity in cases where epipolar geometry feature matching fails.
The semi-supervised learning and stereo disparity matching makes the ESS DNN robust in environments unseen in
the training datasets and with occluded objects. This DNN is optimized for and
evaluated with color (RGB) global shutter stereo camera images, and accuracy may vary with
monochrome stereo images used in analytic computer vision approaches to stereo disparity.
The predicted [disparity](https://en.wikipedia.org/wiki/Binocular_disparity) values
represent the distance a point moves from one image to the other
in a stereo image pair (a.k.a. the binocular image pair). The disparity is inversely proportional
to the depth (i.e. `disparity = focalLength x baseline / depth`). Given the
[focal length](https://en.wikipedia.org/wiki/Focal_length) and [baseline](https://en.wikipedia.org/wiki/Stereo_camera)
of the camera that generates a stereo image pair, the predicted disparity map from the `isaac_ros_ess` package
can be used to compute depth and generate a [point cloud](https://en.wikipedia.org/wiki/Point_cloud).

Note

Compare the requirements of your use case against the package input limitations.

## DNN Models [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html\#dnn-models "Link to this heading")

An ESS model is required to run `isaac_ros_ess`.
[NGC](https://catalog.ngc.nvidia.com/models) provides pre-trained models for use in your robotics application.
[ESS models](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/isaac/models/dnn_stereo_disparity) are available on NGC, providing robust
depth estimation.

Click [here](https://nvidia-isaac-ros.github.io/concepts/dnn_inference/model_preparation.html) for more information on how to use NGC models.

### Confidence and Density [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html\#confidence-and-density "Link to this heading")

ESS DNN provides two outputs: **disparity** estimation and **confidence** estimation.
The disparity output can be filtered, by thresholding the confidence output,
to trade-off between confidence and density.
`isaac_ros_ess` filters out pixels with low confidence by setting:

```
disparity[confidence < threshold] = -1  # -1 means invalid

```

Copy to clipboard

The choice of threshold value is dependent on use case.

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_dnn_stereo_depth/galileo_confidence_viridis.gif/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_dnn_stereo_depth/galileo_confidence_viridis.gif/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_dnn_stereo_depth/galileo_confidence_viridis.gif/)

### Resolution and Performance [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html\#resolution-and-performance "Link to this heading")

NGC Provides **ESS** and **Light ESS** models for trade-off between resolution and performance.
A detailed comparison of the two models can be found [here](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/isaac/models/dnn_stereo_disparity).

| Model Name | Disparity Resolution |
| --- | --- |
| ESS | Estimate disparity at 1/4 HD resolution |
| Light ESS | Estimate disparity at 1/16 HD resolution |

## ESS DNN Plugins [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html\#ess-dnn-plugins "Link to this heading")

ESS TensorRT custom plugins are used to optimize the ESS DNN for inference on NVIDIA GPUs. The plugins work with
custom layers added in ESS model and they are not natively supported by TensorRT. ESS prebuilt plugins are available
for Jetson Orin and x84\_86 platforms The plugins are used during the conversion of the ESS model to TensorRT engine
plan, as well as during TensorRT inference.
ESS TensorRT custom plugins are available for download from NGC together with ESS models.

## Packages [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html\#packages "Link to this heading")

- [`isaac_ros_ess`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html#quickstart)
  - [Try More Examples](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html#try-more-examples)
  - [Troubleshooting](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html#troubleshooting)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html#api)

## Supported Platforms [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html\#supported-platforms "Link to this heading")

This package is designed and tested to be compatible with ROS 2 Humble running on [Jetson](https://developer.nvidia.com/embedded-computing) or an x86\_64 system with an NVIDIA GPU.

Note

Versions of ROS 2 other than Humble are **not** supported. This package depends on specific ROS 2 implementation features that were introduced beginning with the Humble release. ROS 2 versions after Humble have not yet been tested.

| Platform | Hardware | Software | Notes |
| --- | --- | --- | --- |
| Jetson | [Jetson Orin](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/) | [JetPack 6.1 and 6.2](https://developer.nvidia.com/embedded/jetpack) | For best performance, ensure that [power settings](https://docs.nvidia.com/jetson/archives/r34.1/DeveloperGuide/text/SD/PlatformPowerAndPerformance.html) are configured appropriately.<br>Jetson Orin Nano 4GB may not have enough memory to run many of the Isaac ROS packages and is not recommended. |
| x86\_64 | `Ampere` or higher NVIDIA GPU Architecture with 8 GB RAM or higher | [Ubuntu 22.04+](https://releases.ubuntu.com/22.04/) | [CUDA 12.6+](https://developer.nvidia.com/cuda-downloads) |

## Docker [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html\#docker "Link to this heading")

To simplify development, we strongly recommend leveraging the Isaac ROS Dev Docker images by following [these steps](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html).
This streamlines your development environment setup with the correct versions of dependencies on both Jetson and x86\_64 platforms.

Note

All Isaac ROS Quickstarts, tutorials, and examples have been designed with the Isaac ROS Docker images as a prerequisite.

## Customize your Dev Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html\#customize-your-dev-environment "Link to this heading")

To customize your development environment, reference [this guide](https://nvidia-isaac-ros.github.io/concepts/docker_devenv/index.html#development-environment).

## Updates [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html\#updates "Link to this heading")

| Date | Changes |
| --- | --- |
| 2024-09-26 | Updated for ESS 4.1 trained on additional samples |
| 2024-05-30 | Updated for ESS 4.0 with fused kernel plugins |
| 2023-10-18 | Updated for ESS 3.0 with confidence thresholding in multiple resolutions |
| 2023-05-25 | Upgraded model (1.1.0) |
| 2023-04-05 | Source available GXF extensions |
| 2022-10-19 | Updated OSS licensing |
| 2022-08-31 | Update to be compatible with JetPack 5.0.2 |
| 2022-06-30 | Initial release |

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html)