- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Concepts](https://nvidia-isaac-ros.github.io/concepts/index.html)
- NITROS
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/nitros/index.rst.txt)

* * *

# NITROS [](https://nvidia-isaac-ros.github.io/concepts/nitros/index.html\#nitros "Link to this heading")

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nitros/image5-1.gif/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nitros/image5-1.gif/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nitros/image5-1.gif/)

NVIDIA Isaac Transport for ROS (NITROS) is a technology to enable streaming through NVIDIA-accelerated ROS graphs.

## Motivation [](https://nvidia-isaac-ros.github.io/concepts/nitros/index.html\#motivation "Link to this heading")

ROS 2 Humble introduces new hardware acceleration features, including
type adaptation and type negotiation, that significantly increase
performance for developers seeking to incorporate AI/machine learning
and computer vision functionality into their ROS-based applications.

Type adaptation (REP-2007) is common for hardware accelerators, which
require a different data format to deliver optimal performance. Type
adaptation allows ROS nodes to work in a format better suited to the
hardware. Processing graphs can eliminate memory copies between the CPU
and the memory accelerator using the adapted type. Unnecessary memory
copies consume CPU compute, waste power, and slow down performance,
especially as the image size increases.

Type negotiation (REP-2009) allows different ROS nodes in a processing
graph to advertise their supported types so that formats yielding ideal
performance are chosen. The ROS framework performs this negotiation
process and maintains compatibility with legacy nodes that don’t support
negotiation.

Accelerating processing graphs using type adaptation and negotiation
makes the hardware accelerator zero-copy possible. This reduces
software/CPU overhead and unlocks the potential of the underlying
hardware. As roboticists migrate to more powerful compute platforms like
NVIDIA Jetson Orin, they can expect to realize more of the performance
gains enabled by the hardware.

NITROS is NVIDIA’s implementation of type adaption and negotiation.
ROS application graphs made up of NITROS-based, NVIDIA-accelerated Isaac ROS
modules (also known as GEMs or Isaac ROS nodes) can deliver exciting performance and results.

## System Assumptions [](https://nvidia-isaac-ros.github.io/concepts/nitros/index.html\#system-assumptions "Link to this heading")

The design of NITROS makes the following assumptions of the ROS 2
applications:

- To leverage the benefit of zero-copy in NITROS, all
NITROS-accelerated nodes must run in the same process.

- For a given topic in which type negotiation takes place, there can
only be one negotiating publisher.

- For a NITROS-accelerated node, received-frame IDs are assumed to be
constant throughout the runtime.


## NITROS-Accelerated Nodes [](https://nvidia-isaac-ros.github.io/concepts/nitros/index.html\#nitros-accelerated-nodes "Link to this heading")

Most Isaac ROS GEMs have been updated to be NITROS-accelerated. The
acceleration is in effect between NITROS-accelerated nodes when two or
more of them are connected next to each other. In such a case,
NITROS-accelerated nodes can discover each other through type
negotiation and leverage type adaptation for data transmission
automatically at runtime.

NITROS-accelerated nodes are also compatible with non-NITROS nodes: A
NITROS-accelerated node can be used together with any existing,
non-NITROS ROS 2 node, and it will function like a typical ROS 2 node.

## CUDA with NITROS [](https://nvidia-isaac-ros.github.io/concepts/nitros/index.html\#cuda-with-nitros "Link to this heading")

For information about using CUDA with NITROS, please click [here](https://nvidia-isaac-ros.github.io/concepts/nitros/cuda_with_nitros.html).

## PyNITROS [](https://nvidia-isaac-ros.github.io/concepts/nitros/index.html\#pynitros "Link to this heading")

For information about using PyNITROS (NITROS for Python), please click [here](https://nvidia-isaac-ros.github.io/concepts/nitros/pynitros/index.html).

## NITROS Data Types [](https://nvidia-isaac-ros.github.io/concepts/nitros/index.html\#nitros-data-types "Link to this heading")

NITROS supports transporting various common data types with zero-copy in
its own NITROS types. Each NITROS type is one-to-one-mapped to a ROS
message type, which ensures compatibility with existing tools,
workflows, and codebases. A non-NITROS node supporting the corresponding
ROS message types can publish data to or subscribe to data from a
NITROS-accelerated node that supports the corresponding NITROS types.

Visit [isaac\_ros\_nitros\_type](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type) for a complete
list of supported NITROS types.
The following table lists a subset of commonly used types:

| NITROS Interface | ROS Interface |
| --- | --- |
| NitrosImage | [sensor\_msgs/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) |
| NitrosCompressedImage | [sensor\_msgs/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg) |
| NitrosCameraInfo | [sensor\_msgs/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) |
| NitrosTensorList | [isaac\_ros\_tensor\_list\_interfaces/TensorList](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common/blob/main/isaac_ros_tensor_list_interfaces/msg/TensorList.msg) |
| NitrosDisparityImage | [stereo\_msgs/DisparityImage](https://github.com/ros2/common_interfaces/blob/humble/stereo_msgs/msg/DisparityImage.msg) |
| NitrosPointCloud | [sensor\_msgs/PointCloud2](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/PointCloud2.msg) |
| NitrosOccupancyGrid | [nav\_msgs/OccupancyGrid](https://github.com/ros2/common_interfaces/blob/humble/nav_msgs/msg/OccupancyGrid.msg) |
| NitrosOdometry | [nav\_msgs/Odometry](https://github.com/ros2/common_interfaces/blob/humble/nav_msgs/msg/Odometry.msg) |
| NitrosDetection2DArray | [vision\_msgs/Detection2D.msg](https://github.com/ros-perception/vision_msgs/blob/ros2/vision_msgs/msg/Detection2D.msg) |
| NitrosDetection3DArray | [vision\_msgs/Detection3D.msg](https://github.com/ros-perception/vision_msgs/blob/ros2/vision_msgs/msg/Detection3D.msg) |
| NitrosPoseArray | [geometry\_msgs/PoseArray](https://github.com/ros2/common_interfaces/blob/humble/geometry_msgs/msg/PoseArray.msg) |
| NitrosPoseCovStamped | [geometry\_msgs/PoseWithCovariance](https://github.com/ros2/common_interfaces/blob/humble/geometry_msgs/msg/PoseWithCovariance.msg) |
| NitrosTwist | [geometry\_msgs/Twist](https://github.com/ros2/common_interfaces/blob/humble/geometry_msgs/msg/Twist.msg) |
| NitrosImu | [sensor\_msgs/Imu](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Imu.msg) |

## NITROS-Accelerated Graphs [](https://nvidia-isaac-ros.github.io/concepts/nitros/index.html\#nitros-accelerated-graphs "Link to this heading")

ROS 2 graphs built with NITROS-accelerated nodes yield promising
performance. The following highlights three graphs that are created and
tested fully with Isaac ROS NITROS-accelerated nodes.
For more detailed performance outcomes, visit [this page](https://nvidia-isaac-ros.github.io/performance/index.html).
To learn more about creating your own graphs with NITROS-accelerated nodes or adding
NITROS-accelerated nodes in an existing non-NITROS graph,
visit [this page](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros/index.html)

### AprilTag Detection Graph [](https://nvidia-isaac-ros.github.io/concepts/nitros/index.html\#apriltag-detection-graph "Link to this heading")

The AprilTag detection graph uses the NVIDIA GPU-accelerated AprilTags
library to detect AprilTags in images and publishes their poses, IDs,
and additional metadata. Visit [Isaac ROS AprilTag](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_apriltag) for
more details.

ArgusMonoNode (Raw Image)

RectifyNode (Rectified Image)

AprilTagNode (AprilTag Detection)

### Stereo Depth Graph [](https://nvidia-isaac-ros.github.io/concepts/nitros/index.html\#stereo-depth-graph "Link to this heading")

The stereo depth graph performs DNN-based stereo depth estimation
via continuous disparity prediction. It produces a depth image or point
cloud of the scene that can be used for robot navigation. Visit
[Isaac ROS DNN Stereo Depth](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_dnn_stereo_depth) for more details.

ArgusStereoNode (Raw Image)

RectifyNode (Rectified Image)

RectifyNode (Rectified Image)

ESSDisparityNode (DNN Inference)

PointCloudNode (Point Cloud Output)

### Image Segmentation Graph [](https://nvidia-isaac-ros.github.io/concepts/nitros/index.html\#image-segmentation-graph "Link to this heading")

The image segmentation graph uses a deep learning U-Net model to
generate an image mask segmenting out objects of interest. Visit
Isaac ROS Image Segmentation <isaac\_ros\_image\_segmentation>
for more details.

ArgusMonoNode (Raw Image)

RectifyNode (Rectified Image)

DnnImageEncoderNode (DNN Pre-Processed Tensors)

TritonNode (DNN Inference)

UNetDecoderNode (Segmentation Image)

## Repositories and Packages [](https://nvidia-isaac-ros.github.io/concepts/nitros/index.html\#repositories-and-packages "Link to this heading")

The Isaac ROS implementations of this technology are available here:

- [Isaac ROS NITROS](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/index.html)


Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/nitros/index.html)[latest](https://nvidia-isaac-ros.github.io/concepts/nitros/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/concepts/nitros/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/nitros/index.html)