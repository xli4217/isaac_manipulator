- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Image Pipeline](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/index.html)
- `isaac_ros_depth_image_proc`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_depth_image_proc/index.rst.txt)

* * *

# `isaac_ros_depth_image_proc` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_depth_image_proc/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_image_pipeline/blob/main/isaac_ros_depth_image_proc).

## API [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_depth_image_proc/index.html\#api "Link to this heading")

### Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_depth_image_proc/index.html\#overview "Link to this heading")

The `isaac_ros_depth_image_proc` package offers functionality for
processing depth images and producing a point cloud. It largely replaces
the `depth_image_proc` package.

### Available Components [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_depth_image_proc/index.html\#available-components "Link to this heading")

| Component | Topics Subscribed | Topics Published | Parameters |
| --- | --- | --- | --- |
| `PointCloudXyzNode` | `image_rect`, `camera_info`: The depth image and the associated camera info with it | `points`: The resultant XYZ point cloud | `skip`: Skips `skip` number of depth pixels in order to limit the number of pixels converted to points. `output_height` The output height of the point cloud. This should be equivalent to the height of the image. `output_width` The output width of the point cloud. This should be equivalent to the width of the image. |
| `PointCloudXyzrgbNode` | `depth_registered/image_rect`: The depth image `rgb/image_rect_color`: The RGB image associated with the depth image `rgb/camera_info`: The camera info associated with the RGB image and depth image | `points` The resultant XYZRGB point cloud | `skip`: Skips `skip` number of depth pixels in order to limit the number of pixels converted to points. `output_height` The output height of the point cloud. This should be equivalent to the height of the image. `output_width` The output width of the point cloud. This should be equivalent to the width of the image. |

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_depth_image_proc/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_depth_image_proc/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_depth_image_proc/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_depth_image_proc/index.html)