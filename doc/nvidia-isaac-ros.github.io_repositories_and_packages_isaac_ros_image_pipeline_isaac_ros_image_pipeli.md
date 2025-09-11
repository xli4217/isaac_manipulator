- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Image Pipeline](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/index.html)
- `isaac_ros_image_pipeline`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_pipeline/index.rst.txt)

* * *

# `isaac_ros_image_pipeline` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_pipeline/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_image_pipeline/blob/main/isaac_ros_image_pipeline).

## Replacing `image_pipeline` with `isaac_ros_image_pipeline` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_pipeline/index.html\#replacing-image-pipeline-with-isaac-ros-image-pipeline "Link to this heading")

1. Add a dependency on `isaac_ros_image_pipeline` to
`your_package/package.xml` and `your_package/CMakeLists.txt`. If
all desired packages under an existing `image_pipeline` dependency
have Isaac ROS alternatives (see **Supported Packages**), then the
original `image_pipeline` dependency may be removed entirely.

2. Change the package and plugin names in any `*.launch.py` launch
files to use `[package name]` and
`nvidia::isaac_ros::image_proc::[component_name]` respectively. For
a list of all packages, see **Supported Packages**. For a list of all
ROS 2 Components made available, see the per-package detailed
documentation below.


### Supported Packages [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_pipeline/index.html\#supported-packages "Link to this heading")

At this time, the packages under the standard `image_pipeline` have
the following support:

| Existing Package | Isaac ROS Alternative |
| --- | --- |
| `image_pipeline` | See `isaac_ros_image_pipeline` |
| `image_proc` | See `isaac_ros_image_proc` |
| `stereo_image_proc` | See `isaac_ros_stereo_image_proc` |
| `depth_image_proc` | See `isaac_ros_depth_image_proc` |
| `camera_calibration` | Continue using existing package |
| `image_publisher` | Continue using existing package |
| `image_view` | Continue using existing package |
| `image_rotate` | Continue using existing package |

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_pipeline/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_pipeline/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_pipeline/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_pipeline/index.html)