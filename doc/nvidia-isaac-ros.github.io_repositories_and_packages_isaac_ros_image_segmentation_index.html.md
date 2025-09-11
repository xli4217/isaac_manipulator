- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- Isaac ROS Image Segmentation
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_image_segmentation/index.rst.txt)

* * *

# Isaac ROS Image Segmentation [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/index.html\#repo-name "Link to this heading")

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_image_segmentation_example.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_image_segmentation_example.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_image_segmentation_example.png/) [![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_image_segmentation_example_seg.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_image_segmentation_example_seg.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_image_segmentation_example_seg.png/)

## Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/index.html\#overview "Link to this heading")

[Isaac ROS Image Segmentation](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_image_segmentation) contains ROS packages for semantic image segmentation.

These packages provide methods for classification of an input image
at the pixel level by running GPU-accelerated inference on a DNN model.
Each pixel of the input image is predicted to belong to a set of defined classes.
The output prediction can be used by perception functions to understand where each
class is spatially in a 2D image or fuse with a corresponding depth location in a 3D scene.

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_image_segmentation_nodegraph.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_image_segmentation_nodegraph.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_image_segmentation_nodegraph.png/)

| Package | Model Architecture | Description |
| --- | --- | --- |
| [Isaac ROS U-NET](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_unet/index.html#quickstart) | [U-NET](https://en.wikipedia.org/wiki/U-Net) | Convolutional network popular for biomedical imaging segmentation models |
| [Isaac ROS Segformer](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html#quickstart) | [Segformer](https://arxiv.org/abs/2105.15203) | Transformer-based network that works well for objects of varying scale |
| [Isaac ROS Segment Anything](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segment_anything/index.html#quickstart) | [Segment Anything](https://segment-anything.com/) | Segments any object in an image when given a prompt as to which one |

Input images may need to be cropped and resized to maintain the aspect ratio and match the input
resolution expected by the DNN model; image resolution may be reduced to improve
DNN inference performance, which typically scales directly with the
number of pixels in the image.

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_image_segmentation_example_bboxseg.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_image_segmentation_example_bboxseg.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_image_segmentation_example_bboxseg.png/)

Image segmentation provides more information and uses more compute than
object detection to produce classifications per pixel, whereas object
detection classifies a simpler bounding box rectangle in image
coordinates. Object detection is used to know if, and where spatially in
a 2D image, the object exists. On the other hand, image segmentation is used to know which
pixels belong to the class. One application is using the segmentation result, and fusing it with the corresponding depth
information in order to know an object location in a 3D scene.

## Quickstarts [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/index.html\#quickstarts "Link to this heading")

- [Isaac ROS U-NET](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_unet/index.html#quickstart)

- [Isaac ROS Segformer](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html#quickstart)

- [Isaac ROS Segment Anything](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segment_anything/index.html#quickstart)


## Isaac ROS NITROS Acceleration [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/index.html\#isaac-ros-nitros-acceleration "Link to this heading")

This package is powered by [NVIDIA Isaac Transport for ROS (NITROS)](https://developer.nvidia.com/blog/improve-perception-performance-for-ros-2-applications-with-nvidia-isaac-transport-for-ros/), which leverages type adaptation and negotiation to optimize message formats and dramatically accelerate communication between participating nodes.

## DNN models [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/index.html\#dnn-models "Link to this heading")

Each image segmentation package relies on a pre-trained DNN model. You can train your own models or download pre-trained models
from one of the many model zoo’s available online or from [NGC](https://catalog.ngc.nvidia.com/models) which provides pre-trained models for use in your robotics application.

NGC pre-trained models can be fine-tuned for your application using TAO used in the examples provided.
See [here](https://nvidia-isaac-ros.github.io/concepts/dnn_inference/model_preparation.html) for more information on how to use various models from NVIDIA hosted on [NGC](https://catalog.ngc.nvidia.com/models).

### U-NET [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/index.html\#u-net "Link to this heading")

A U-NET model is required to use `isaac_ros_unet`.

[PeopleSemSegNet](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/tao/models/peoplesemsegnet) provides a pre-trained model for best-in-class,
real-time people segmentation.

This package has been validated against the following models:

| Model Name | Use Case |
| --- | --- |
| [PeopleSemSegNet AMR](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/tao/models/peoplesemsegnet_amr) | Segment people from point of view of a mobile robot |
| [PeopleSemSegNet ShuffleSeg](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/tao/models/peoplesemsegnet/files?version=deployable_shuffleseg_unet_v1.0) | Semantically segment people inference at a high speed |
| [PeopleSemSegNet](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/tao/models/peoplesemsegnet/files?version=deployable_vanilla_unet_v2.0) | Semantically segment people |

### Segformer [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/index.html\#segformer "Link to this heading")

A Segformer model is required to use `isaac_ros_segformer`.

[PeopleSemSegFormer](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/tao/models/peoplesemsegformer) provides a pre-trained model for more effective real-time people segmentation.

This package has been validated against the following models:

| Model Name | Use Case |
| --- | --- |
| [PeopleSemSegFormer](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/tao/models/peoplesemsegformer) | Semantically segment people |

### Segment Anything [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/index.html\#segment-anything "Link to this heading")

The [Segment Anything (SAM)](https://segment-anything.com/) model is required to use `isaac_ros_segment_anything`.
This model requires an additional prompt value to indicate what object in the image should be segmented.

This package has been validated against the following versions of SAM:

| Model Name | Description |
| --- | --- |
| [SAM](https://segment-anything.com/) | Full accuracy model that can consume over 8GB of GPU memory and require more computational power |
| [Mobile SAM](https://docs.ultralytics.com/models/mobile-sam/) | Slimmer variant of SAM optimized for a smaller memory and compute footprint. |

## Packages [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/index.html\#packages "Link to this heading")

- [`isaac_ros_segformer`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html#quickstart)
  - [Try More Examples](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html#try-more-examples)
  - [Troubleshooting](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html#troubleshooting)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html#api)
- [`isaac_ros_segment_anything`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segment_anything/index.html)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segment_anything/index.html#quickstart)
  - [Try More Examples](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segment_anything/index.html#try-more-examples)
  - [Troubleshooting](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segment_anything/index.html#troubleshooting)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segment_anything/index.html#api)
- [`isaac_ros_unet`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_unet/index.html)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_unet/index.html#quickstart)
  - [Try More Examples](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_unet/index.html#try-more-examples)
  - [Troubleshooting](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_unet/index.html#troubleshooting)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_unet/index.html#api)

## Supported Platforms [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/index.html\#supported-platforms "Link to this heading")

This package is designed and tested to be compatible with ROS 2 Humble running on [Jetson](https://developer.nvidia.com/embedded-computing) or an x86\_64 system with an NVIDIA GPU.

Note

Versions of ROS 2 other than Humble are **not** supported. This package depends on specific ROS 2 implementation features that were introduced beginning with the Humble release. ROS 2 versions after Humble have not yet been tested.

| Platform | Hardware | Software | Notes |
| --- | --- | --- | --- |
| Jetson | [Jetson Orin](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/) | [JetPack 6.1 and 6.2](https://developer.nvidia.com/embedded/jetpack) | For best performance, ensure that [power settings](https://docs.nvidia.com/jetson/archives/r34.1/DeveloperGuide/text/SD/PlatformPowerAndPerformance.html) are configured appropriately.<br>Jetson Orin Nano 4GB may not have enough memory to run many of the Isaac ROS packages and is not recommended. |
| x86\_64 | `Ampere` or higher NVIDIA GPU Architecture with 8 GB RAM or higher | [Ubuntu 22.04+](https://releases.ubuntu.com/22.04/) | [CUDA 12.6+](https://developer.nvidia.com/cuda-downloads) |

## Docker [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/index.html\#docker "Link to this heading")

To simplify development, we strongly recommend leveraging the Isaac ROS Dev Docker images by following [these steps](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html).
This streamlines your development environment setup with the correct versions of dependencies on both Jetson and x86\_64 platforms.

Note

All Isaac ROS Quickstarts, tutorials, and examples have been designed with the Isaac ROS Docker images as a prerequisite.

## Customize your Dev Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/index.html\#customize-your-dev-environment "Link to this heading")

To customize your development environment, reference [this guide](https://nvidia-isaac-ros.github.io/concepts/docker_devenv/index.html#development-environment).

## Updates [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/index.html\#updates "Link to this heading")

| Date | Changes |
| --- | --- |
| 2024-12-10 | Update to be compatible with JetPack 6.1 |
| 2024-09-26 | Update for ZED compatibility |
| 2024-05-30 | Add SegFormer and Segment Anything packages |
| 2023-10-18 | Updated for Isaac ROS 2.0.0. |
| 2023-05-25 | Performance improvements |
| 2023-04-05 | Source available GXF extensions |
| 2022-10-19 | Updated OSS licensing |
| 2022-08-31 | Update to be compatible with JetPack 5.0.2 |
| 2022-06-30 | Removed frame\_id, queue\_size and tensor\_output\_order parameter. Added network\_output\_type parameter (support for sigmoid and argmax output layers). Switched implementation to use NITROS. Removed support for odd sized images. Switched tutorial to use PeopleSemSegNet ShuffleSeg and moved unnecessary details to other READMEs. |
| 2021-11-15 | Isaac Sim HIL documentation update |
| 2021-10-20 | Initial release |

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_image_segmentation/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_image_segmentation/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_image_segmentation/index.html)