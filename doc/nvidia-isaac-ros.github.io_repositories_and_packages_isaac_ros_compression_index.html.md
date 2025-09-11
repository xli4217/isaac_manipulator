- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- Isaac ROS Compression
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_compression/index.rst.txt)

* * *

# Isaac ROS Compression [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/index.html\#repo-name "Link to this heading")

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_compression/isaac_ros_compression_nodegraph.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_compression/isaac_ros_compression_nodegraph.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_compression/isaac_ros_compression_nodegraph.png/)

## Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/index.html\#overview "Link to this heading")

[Isaac ROS Compression](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_compression) provides H.264 image encoder
and decoder that leverages the specialized hardware in NVIDIA GPUs and the
[Jetson](https://developer.nvidia.com/embedded-computing) platform.
The `isaac_ros_h264_encoder` package can compress an image into H.264
data using the NVENC. The
`isaac_ros_h264_decoder` package can decode the H.264 data into
original images using the NVDEC.

Image compression reduces the data footprint of images when written to
storage or transmitted between computers. A 1080p camera at 30fps
produces 177MB/s of data; image compression reduces this by
approximately 10 times to 17MB/s of data, reducing the throughput needed
to send this to another computer or write out to storage; a one minute
1080p camera recording is reduced from ~10GB to ~1GB. This compression
is provided by dedicated NVIDIA acceleration (NvEnc) separate from
other hardware engines such as the GPU.

A common use case for image compression during the development of robots
is to capture camera images to storage. This captured data is processed
offline from the robot to produce training datasets for AI models, test
datasets for perception functions, and test data for open-loop
re-simulation of software in development with real data. The compression
parameters are tuned to minimize visual quality reduction from lossy
compression for AI model and perception function development.
Compression reduces the amount of data written to storage, the time
required to offload the recording, and footprint of the data at rest in
a data lake.

Compression can be used with event data recorders to capture camera
images to storage when an event of interest occurs, often due to
failures on the robot. This provides visual information to assist in the
debugging of the event or to improve perception and robot functions.

[H.264](https://en.wikipedia.org/wiki/Advanced_Video_Coding) is an
efficient and popular compression algorithm with broad support across
many platforms. The output of the `isaac_ros_h264_encoder` package can then
be decoded with NVIDIA acceleration using the
`isaac_ros_h264_decoder` on Jetson and x86\_64 systems, or by
third-party H.264 decoder packages on non-NVIDIA platforms.

This package is powered by [NVIDIA Isaac Transport for ROS (NITROS)](https://developer.nvidia.com/blog/improve-perception-performance-for-ros-2-applications-with-nvidia-isaac-transport-for-ros/),
which leverages type adaptation and negotiation to optimize message
formats and dramatically accelerate communication between participating nodes.

Note

ROS 2 relies on `image_transport_plugins` for CPU based compression.
We recommend using `isaac_ros_h264_encoder` as part of the graph of
nodes when capturing to a rosbag for performance benefits of NITROS;
ROS 2 type adaptation used by NITROS is not supported by `image_transport_plugins`,
resulting in more CPU load, and less encode performance.

## Quickstarts [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/index.html\#quickstarts "Link to this heading")

- [Isaac ROS H.264 Encoder](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html#quickstart)

- [Isaac ROS H.264 Decoder](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_decoder/index.html#quickstart)


## H.264 compared to JPEG image\_transport\_plugins [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/index.html\#h-264-compared-to-jpeg-image-transport-plugins "Link to this heading")

ROS [image\_transport\_plugins](https://index.ros.org/p/image_transport_plugins) use the JPEG standard for image compression on the CPU, where each frame is compressed spatially within the 2D image. `isaac_ros_h264_encoder` uses the H.264 standard for video compression, which allows it to compress more efficiently than JPEG by using more advanced spatial and temporal compression. `isaac_ros_h264_encoder` uses the GPU for higher performance, while offloading compute work from the CPU.

H.264 video compression of individual images, or I-frame (inter-frame) are smaller in size than equivalent quality JPEG images; H.264 uses improved compression functions for smooth areas and less artifacts in high frequencies areas of the image. H.264 adds temporal compression on a sequence of images by using a P-frame (predicted frame) from a previous I or P frame. P-frames benefit on image similarity from frame to frame as occurs from a camera stream. P-frames are ~25% the size of an I-Frame. A sequence of images is compressed to one I-Frame followed by one or more P-Frames. P-Frames require less computation improving frame rate of compression.

As a result, `isaac_ros_h264_encoder` can perform compression to smaller sizes with the same quality as JPEG `image_transport_plugins`, at higher frame rates (throughput), while simultaneously offloading the CPU from this compute for compression.

Note

Check your requirements against the package’s [input limitations](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html#api).

## Packages [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/index.html\#packages "Link to this heading")

- [`isaac_ros_h264_decoder`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_decoder/index.html)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_decoder/index.html#quickstart)
  - [Troubleshooting](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_decoder/index.html#troubleshooting)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_decoder/index.html#api)
- [`isaac_ros_h264_encoder`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html)
  - [Quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html#quickstart)
  - [API](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html#api)

## Supported Platforms [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/index.html\#supported-platforms "Link to this heading")

| Node | Jetson `aarch64` | GPU x86\_64 |
| --- | --- | --- |
| `isaac_ros_h264_encoder` | ✓ | ✓ |
| `isaac_ros_h264_decoder` | ✓ | ✓ |

This package is designed and tested to be compatible with ROS 2 Humble running on [Jetson](https://developer.nvidia.com/embedded-computing) or an x86\_64 system with an NVIDIA GPU.

Note

Versions of ROS 2 other than Humble are **not** supported. This package depends on specific ROS 2 implementation features that were introduced beginning with the Humble release. ROS 2 versions after Humble have not yet been tested.

| Platform | Hardware | Software | Notes |
| --- | --- | --- | --- |
| Jetson | [Jetson Orin](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/) | [JetPack 6.1 and 6.2](https://developer.nvidia.com/embedded/jetpack) | For best performance, ensure that [power settings](https://docs.nvidia.com/jetson/archives/r34.1/DeveloperGuide/text/SD/PlatformPowerAndPerformance.html) are configured appropriately.<br>Jetson Orin Nano 4GB may not have enough memory to run many of the Isaac ROS packages and is not recommended. |
| x86\_64 | `Ampere` or higher NVIDIA GPU Architecture with 8 GB RAM or higher | [Ubuntu 22.04+](https://releases.ubuntu.com/22.04/) | [CUDA 12.6+](https://developer.nvidia.com/cuda-downloads) |

## Docker [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/index.html\#docker "Link to this heading")

To simplify development, we strongly recommend leveraging the Isaac ROS Dev Docker images by following [these steps](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html).
This streamlines your development environment setup with the correct versions of dependencies on both Jetson and x86\_64 platforms.

Note

All Isaac ROS Quickstarts, tutorials, and examples have been designed with the Isaac ROS Docker images as a prerequisite.

## Customize your Dev Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/index.html\#customize-your-dev-environment "Link to this heading")

To customize your development environment, reference [this guide](https://nvidia-isaac-ros.github.io/concepts/docker_devenv/index.html#development-environment).

## Updates [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/index.html\#updates "Link to this heading")

| Date | Changes |
| --- | --- |
| 2024-12-10 | Update to be compatible with JetPack 6.1 |
| 2024-09-26 | Update for ZED compatibility |
| 2024-05-30 | Update to be compatible with JetPack 6.0 |
| 2023-10-18 | Added support on `x86_64` for `isaac_ros_h264_encoder` |
| 2023-05-25 | Performance improvements |
| 2023-04-05 | P-frame encoder and Tegra decoder support |
| 2022-10-19 | Initial release |

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_compression/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_compression/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_compression/index.html)