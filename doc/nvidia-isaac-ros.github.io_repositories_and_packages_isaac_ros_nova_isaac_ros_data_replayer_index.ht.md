- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Nova](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html)
- `isaac_ros_data_replayer`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.rst.txt)

* * *

# `isaac_ros_data_replayer` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova/blob/main/isaac_ros_data_replayer).

## Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#overview "Link to this heading")

The `isaac_ros_data_replayer` package enables replaying sensor data recorded using the `isaac_ros_data_recorder`
package. This package also enables reliable extraction of stereo cameras in different formats,
including depth using AI-based perception. Supported formats include MP4, PNG, and PFM. Stereo camera
frames are extracted in synchronized pairs while depth frames are aligned to the left stereo imager.

## Quickstart [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#quickstart "Link to this heading")

### Set Up Development Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#set-up-development-environment "Link to this heading")

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


### Download Quickstart Assets [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#download-quickstart-assets "Link to this heading")

1. Download quickstart data from NGC:

Make sure required libraries are installed.





```
sudo apt-get install -y curl jq tar

```

Copy to clipboard



Then, run these commands to download the asset from NGC:





```
NGC_ORG="nvidia"
NGC_TEAM="isaac"
PACKAGE_NAME="isaac_ros_data_replayer"
NGC_RESOURCE="isaac_ros_data_replayer_assets"
NGC_FILENAME="quickstart.tar.gz"
MAJOR_VERSION=3
MINOR_VERSION=2
VERSION_REQ_URL="https://catalog.ngc.nvidia.com/api/resources/versions?orgName=$NGC_ORG&teamName=$NGC_TEAM&name=$NGC_RESOURCE&isPublic=true&pageNumber=0&pageSize=100&sortOrder=CREATED_DATE_DESC"
AVAILABLE_VERSIONS=$(curl -s \
       -H "Accept: application/json" "$VERSION_REQ_URL")
LATEST_VERSION_ID=$(echo $AVAILABLE_VERSIONS | jq -r "
       .recipeVersions[]
       | .versionId as \$v
       | \$v | select(test(\"^\\\\d+\\\\.\\\\d+\\\\.\\\\d+$\"))
       | split(\".\") | {major: .[0]|tonumber, minor: .[1]|tonumber, patch: .[2]|tonumber}
       | select(.major == $MAJOR_VERSION and .minor <= $MINOR_VERSION)
       | \$v
       " | sort -V | tail -n 1
)
if [ -z "$LATEST_VERSION_ID" ]; then
       echo "No corresponding version found for Isaac ROS $MAJOR_VERSION.$MINOR_VERSION"
       echo "Found versions:"
       echo $AVAILABLE_VERSIONS | jq -r '.recipeVersions[].versionId'
else
       mkdir -p ${ISAAC_ROS_WS}/isaac_ros_assets && \
       FILE_REQ_URL="https://api.ngc.nvidia.com/v2/resources/$NGC_ORG/$NGC_TEAM/$NGC_RESOURCE/\
versions/$LATEST_VERSION_ID/files/$NGC_FILENAME" && \
       curl -LO --request GET "${FILE_REQ_URL}" && \
       tar -xf ${NGC_FILENAME} -C ${ISAAC_ROS_WS}/isaac_ros_assets && \
       rm ${NGC_FILENAME}
fi

```

Copy to clipboard


### Build `isaac_ros_data_replayer` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#build-package-name "Link to this heading")

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
sudo apt-get install -y ros-humble-isaac-ros-data-replayer
source /opt/ros/humble/setup.bash

```

Copy to clipboard

3. Install the required assets:





```
sudo apt-get install -y ros-humble-isaac-ros-ess-models-install
source /opt/ros/humble/setup.bash

ros2 run isaac_ros_ess_models_install install_ess_models.sh --eula

```

Copy to clipboard


### Run Launch File [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#run-launch-file "Link to this heading")

```
ros2 launch isaac_ros_data_replayer data_replayer.launch.py \
   rosbag:=${ISAAC_ROS_WS}/isaac_ros_assets/isaac_ros_data_replayer/quickstart

```

Copy to clipboard

### Replay a single camera [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#replay-a-single-camera "Link to this heading")

> ```
> ros2 launch isaac_ros_data_replayer data_replayer.launch.py \
>    rosbag:=${ISAAC_ROS_WS}/isaac_ros_assets/isaac_ros_data_replayer/quickstart \
>    enabled_stereo_cameras:=front_stereo_camera enabled_fisheye_cameras:=None enable_3d_lidar:=False
>
> ```
>
> Copy to clipboard

### Visualize [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#visualize "Link to this heading")

1. Open a **new** terminal inside the Docker container:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
      ./scripts/run_dev.sh

```

Copy to clipboard

2. Run RViz to visualize data:





```
rviz2

```

Copy to clipboard


[![data_replayer_rviz](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/data_replayer_rviz.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/data_replayer_rviz.png/)

### Data Extraction [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#data-extraction "Link to this heading")

To extract synchronized camera frames from a rosbag, run:

ESSLight ESS

```
ros2 run isaac_ros_data_replayer data_extraction.py \
   --rosbag ${ISAAC_ROS_WS}/isaac_ros_assets/isaac_ros_data_replayer/quickstart \
   --camera front_stereo_camera --output ${ISAAC_ROS_WS} \
   --engine_file_path ${ISAAC_ROS_WS}/isaac_ros_assets/models/dnn_stereo_disparity/dnn_stereo_disparity_v4.1.0_onnx/ess.engine

```

Copy to clipboard

By default, rectified camera frames will be extracted alongside disparity and depth images generated by ESS.
To extract rectified images without depth or disparity, run:

```
ros2 run isaac_ros_data_replayer data_extraction.py \
   --rosbag ${ISAAC_ROS_WS}/isaac_ros_assets/isaac_ros_data_replayer/quickstart \
   --camera front_stereo_camera --output ${ISAAC_ROS_WS} \
   --mode rectify

```

Copy to clipboard

To extract raw images, run:

```
ros2 run isaac_ros_data_replayer data_extraction.py \
   --rosbag ${ISAAC_ROS_WS}/isaac_ros_assets/isaac_ros_data_replayer/quickstart \
   --camera front_stereo_camera --output ${ISAAC_ROS_WS} \
   --mode raw

```

Copy to clipboard

### Rosbag Conversion [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#rosbag-conversion "Link to this heading")

The [isaac\_ros\_data\_recorder](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/index.html)
records camera streams as
[sensor\_msgs/msg/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg)
messages encoded with H.264 encoding, and 3D LIDAR data as
[hesai\_ros\_driver/msg/UdpFrame](https://github.com/HesaiTechnology/HesaiLidar_ROS_2.0/blob/master/msg/msg_ros2/UdpFrame.msg).

The `rosbag_converter` tool converts rosbags from `isaac_ros_data_recorder` so that camera
streams are stored uncompressed as
[sensor\_msgs/msg/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg)
messages and 3D LIDAR data as
[sensor\_msgs/msg/PointCloud2](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/PointCloud2.msg)
messages (with fields `x, y, z, intensity, ring, timestamp`).
For additional details refer to
[isaac\_ros\_hesai](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_hesai/index.html)
documentation.

> ```
> ros2 run isaac_ros_data_replayer rosbag_converter.py \
>    -i ${ISAAC_ROS_WS}/isaac_ros_assets/isaac_ros_data_replayer/quickstart \
>    -o ${ISAAC_ROS_WS}/uncompressed_rosbag/ \
>    --all
>
> ```
>
> Copy to clipboard

Note

**Disk space requirements**: The `rosbag_converter` tool requires free disk space of at least
100 times the size of the original rosbag. Specific requirements may vary and depend on the
number of sensors recorded in the original rosbag, among other things.

Note

The `rosbag_converter` tool is expected to be used on `x86_64`, with sufficient disk space
under the `/tmp` directory. The original rosbag needs to be copied to the `x86_64` machine
where the conversion will be performed.

## Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#troubleshooting "Link to this heading")

### Isaac ROS Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#isaac-ros-troubleshooting "Link to this heading")

For solutions to problems with Isaac ROS, see [troubleshooting](https://nvidia-isaac-ros.github.io/troubleshooting/index.html).

### Not all camera frames extracted from recording [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#not-all-camera-frames-extracted-from-recording "Link to this heading")

Data extraction extracts synchronized camera frames from a recording. If the number of frames
from the left and right camera are not equal, then some frames will be ignored to keep the left
and right cameras in sync.

### Not enough free disk space to run the `rosbag_converter` tool [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#not-enough-free-disk-space-to-run-the-rosbag-converter-tool "Link to this heading")

#### Symptom [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#symptom "Link to this heading")

Execution of the `rosbag_converter` tool crashes with a python error, including but not limited to:

```
RuntimeError: Exception on parsing info file: invalid node; first invalid key: "version"
[ros2run]: Process exited with failure 1

```

Copy to clipboard

#### Solution [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#solution "Link to this heading")

> Free additional disk space before executing the `rosbag_converter` tool.
>
> Alternatively, consider reducing the size of the input rosbag, or consider converting only the
> subset of topics needed for your application. Note that the argument `-a` or `--all`
> converts all the possible topics in the input rosbag, but the arguments
> `--topics_decode_h264` and `--topics_udp_to_pc2` allow for the conversion of
> specific topics.
>
> Make sure to use the tool on a `x86_64` machine.

## API [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#api "Link to this heading")

### Usage [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#usage "Link to this heading")

```
ros2 launch isaac_ros_data_replayer data_replayer.launch.py rosbag:=<rosbag>

```

Copy to clipboard

### ROS Launch Arguments [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#ros-launch-arguments "Link to this heading")

| ROS Launch Argument | Default Value | Description |
| --- | --- | --- |
| `rosbag` | None | Path to recording. |
| `enabled_stereo_cameras` | `front_stereo_camera` `back_stereo_camera` `left_stereo_camera` `right_stereo_camera` | Stereo cameras enabled for replay. |
| `enabled_fisheye_cameras` | `front_fisheye_camera` `back_fisheye_camera` `left_fisheye_camera` `right_fisheye_camera` | Fisheye cameras enabled for replay. |
| `enable_3d_lidar` | `True` | Enable 3D lidar for replay. |
| `replay_loop` | `False` | Loop replay. |
| `replay_rate` | `1.0` | Extraction rate; 1.0 for real-time, <1.0 for sub-real-time, >1.0 for super-real-time. |
| `replay_delay` | None | Delay to start replay. |
| `replay_additional_args` | `--disable-keyboard-controls` | Additional arguments for replay. |

### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#ros-topics-published "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `/rosout` | [rcl\_interfaces/msg/Log](https://github.com/ros2/rcl_interfaces/blob/humble/rcl_interfaces/msg/Log.msg) | Console logs. |
| `/tf` | [tf2\_msgs/msg/TFMessage](https://github.com/ros2/geometry2/blob/humble/tf2_msgs/msg/TFMessage.msg) | Movable transforms on the robot. |
| `/tf_static` | [tf2\_msgs/msg/TFMessage](https://github.com/ros2/geometry2/blob/humble/tf2_msgs/msg/TFMessage.msg) | Fixed transforms on the robot. |
| `/front_stereo_camera/left/image_raw` | [sensor\_msgs/msg/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | Front stereo camera left camera stream. |
| `/front_stereo_camera/left/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Front stereo camera left camera intrinsics. |
| `/front_stereo_camera/right/image_raw` | [sensor\_msgs/msg/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | Front stereo camera right camera stream. |
| `/front_stereo_camera/right/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Front stereo camera right camera intrinsics. |
| `/back_stereo_camera/left/image_raw` | [sensor\_msgs/msg/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | Back stereo camera left camera stream. |
| `/back_stereo_camera/left/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Back stereo camera left camera intrinsics. |
| `/back_stereo_camera/right/image_raw` | [sensor\_msgs/msg/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | Back stereo camera right camera stream. |
| `/back_stereo_camera/right/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Back stereo camera right camera intrinsics. |
| `/left_stereo_camera/left/image_raw` | [sensor\_msgs/msg/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | Left stereo camera left camera stream. |
| `/left_stereo_camera/left/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Left stereo camera left camera intrinsics. |
| `/left_stereo_camera/right/image_raw` | [sensor\_msgs/msg/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | Left stereo camera right camera stream. |
| `/left_stereo_camera/right/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Left stereo camera right camera intrinsics. |
| `/right_stereo_camera/left/image_raw` | [sensor\_msgs/msg/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | Right stereo camera left camera stream. |
| `/right_stereo_camera/left/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Right stereo camera left camera intrinsics. |
| `/right_stereo_camera/right/image_raw` | [sensor\_msgs/msg/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | Right stereo camera right camera stream. |
| `/right_stereo_camera/right/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Right stereo camera right camera intrinsics. |
| `/front_fisheye_camera/left/image_raw` | [sensor\_msgs/msg/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | Front fisheye camera stream. |
| `/front_fisheye_camera/left/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Front fisheye camera intrinsics. |
| `/back_fisheye_camera/left/image_raw` | [sensor\_msgs/msg/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | Back fisheye camera stream. |
| `/back_fisheye_camera/left/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Back fisheye camera intrinsics. |
| `/left_fisheye_camera/left/image_raw` | [sensor\_msgs/msg/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | Left fisheye camera stream. |
| `/left_fisheye_camera/left/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Left fisheye camera intrinsics. |
| `/right_fisheye_camera/left/image_raw` | [sensor\_msgs/msg/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | Right fisheye camera stream. |
| `/right_fisheye_camera/left/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Right fisheye camera intrinsics. |
| `/front_2d_lidar/scan` | [sensor\_msgs/msg/LaserScan](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/LaserScan.msg) | Front 2D lidar scan. |
| `/back_2d_lidar/scan` | [sensor\_msgs/msg/LaserScan](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/LaserScan.msg) | Back 2D lidar scan. |
| `/front_3d_lidar/lidar_points` | [sensor\_msgs/msg/PointCloud2](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/PointCloud2.msg) | Front 3D lidar point cloud. |
| `/front_stereo_imu/imu` | [sensor\_msgs/msg/Imu](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Imu.msg) | Front stereo camera inertial measurement unit. |
| `/chassis/imu` | [sensor\_msgs/msg/Imu](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Imu.msg) | Chassis inertial measurement unit. |
| `/chassis/ticks` | [isaac\_ros\_nova\_interfaces/msg/EncoderTicks](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common/blob/main/isaac_ros_nova_interfaces/msg/EncoderTicks.msg) | Chassis encoder count. |
| `/chassis/odom` | [nav\_msgs/msg/Odometry](https://github.com/ros2/common_interfaces/blob/humble/nav_msgs/msg/Odometry.msg) | Chassis odometry. |
| `/chassis/battery_state` | [sensor\_msgs/msg/BatteryState](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/BatteryState.msg) | Chassis battery state. |

### Usage [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#id1 "Link to this heading")

```
ros2 run isaac_ros_data_replayer data_extraction.py --rosbag <rosbag> --camera <camera> --output <output>

```

Copy to clipboard

### ROS Run Arguments [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#ros-run-arguments "Link to this heading")

| ROS Run Argument | Default Value | Description |
| --- | --- | --- |
| `--rosbag` | None | Path to recording. |
| `--camera` | None | Camera to extract i.e. `front_stereo_camera`. |
| `--output` | None | Directory to store extracted data. |
| `--min_disparity` | `3.0` | Minimum disparity value for normalization. |
| `--max_disparity` | `200.0` | Maximum disparity value for normalization. |
| `--threshold` | `0.0` | ESS threshold. |
| `--engine_file_path` | `isaac_ros_assets/models/dnn_stereo_disparity/dnn_stereo_disparity_v4.1.0_onnx/ess.engine` | ESS engine file path. |
| `--mode` | `depth` | Image extraction mode (‘raw’, ‘rectify’, or ‘depth’) |

### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#ros-topics-subscribed "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `/<camera>/left/image_compressed` | [sensor\_msgs/msg/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg) | Left camera stream. |
| `/<camera>/left/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Left camera intrinsics. |
| `/<camera>/right/image_compressed` | [sensor\_msgs/msg/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg) | Right camera stream. |
| `/<camera>/right/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Right camera intrinsics. |

### Usage [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#id2 "Link to this heading")

```
ros2 run isaac_ros_data_replayer rosbag_converter.py --input_rosbag <input_rosbag> --output_rosbag <output_rosbag> --all

```

Copy to clipboard

### ROS Run Arguments [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#id3 "Link to this heading")

| ROS Run Argument | Default Value | Description |
| --- | --- | --- |
| `-i`, `--input_rosbag` | None | Path to the input rosbag (recording from the `isaac_ros_data_recorder`). |
| `-o`, `--output_rosbag` | None | Path to the output (converted) rosbag. |
| `-a`, `--all` | None | Whether to convert all possible topics in the input rosbag. If provided, it takes priority over other arguments listing topics. |
| `--topics_decode_h264` | \[\] | List of image topics with [sensor\_msgs/msg/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg) h264 messages to be decoded as [sensor\_msgs/msg/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg). |
| `--topics_udp_to_pc2` | \[\] | List of LIDAR topics with [hesai\_ros\_driver/msg/UdpFrame](https://github.com/HesaiTechnology/HesaiLidar_ROS_2.0/blob/master/msg/msg_ros2/UdpFrame.msg) packets to be converted to [sensor\_msgs/msg/PointCloud2](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/PointCloud2.msg). |

### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html\#id4 "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `/<camera>/left/image_compressed` | [sensor\_msgs/msg/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg) | Left camera stream. |
| `/<camera>/left/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Left camera intrinsics. |
| `/<camera>/right/image_compressed` | [sensor\_msgs/msg/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg) | Right camera stream. |
| `/<camera>/right/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Right camera intrinsics. |
| `/<lidar>/lidar_packets` | [hesai\_ros\_driver/msg/UdpFrame](https://github.com/HesaiTechnology/HesaiLidar_ROS_2.0/blob/master/msg/msg_ros2/UdpFrame.msg) | LIDAR UDP packets. |

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html)