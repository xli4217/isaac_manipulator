- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Compression](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/index.html)
- `isaac_ros_h264_encoder`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.rst.txt)

* * *

# `isaac_ros_h264_encoder` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_compression/blob/main/isaac_ros_h264_encoder).

## Quickstart [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html\#quickstart "Link to this heading")

### Set Up Development Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html\#set-up-development-environment "Link to this heading")

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


### Download Quickstart Assets [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html\#download-quickstart-assets "Link to this heading")

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
PACKAGE_NAME="isaac_ros_h264_encoder"
NGC_RESOURCE="isaac_ros_h264_encoder_assets"
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


### Build `isaac_ros_h264_encoder` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html\#build-package-name "Link to this heading")

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
sudo apt-get install -y ros-humble-isaac-ros-h264-encoder

```

Copy to clipboard


### Run Launch File [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html\#run-launch-file "Link to this heading")

RealSense CameraHawk CameraZED Camera

1. Ensure that you have already set up your RealSense camera using the [RealSense setup tutorial](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/realsense_setup.html). If you have not, please set up the sensor and then restart this quickstart from the beginning.

2. Continuing inside the container, install the following dependencies:


> ```
> sudo apt-get update
>
> ```
>
> Copy to clipboard






```
sudo apt-get install -y ros-humble-isaac-ros-examples ros-humble-isaac-ros-realsense

```

Copy to clipboard

3. Run the launch file. This launch file launches the example with the RealSense camera:





```
ros2 launch isaac_ros_examples isaac_ros_examples.launch.py launch_fragments:=realsense_stereo_rect,stereo_h264_encoder

```

Copy to clipboard


### Visualize Results [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html\#visualize-results "Link to this heading")

Note

Visualization is **optional**. To visualize the encoded output, you need to complete the [Isaac ROS H264 decoder](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_decoder/index.html#quickstart).

1. Open a **new** terminal inside the Docker container:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
      ./scripts/run_dev.sh

```

Copy to clipboard





RealSense CameraHawk CameraZED Camera





2. Decode and visualize the images:





```
ros2 launch isaac_ros_examples isaac_ros_examples.launch.py launch_fragments:=stereo_h264_decoder

```

Copy to clipboard







```
ros2 run image_view image_view --ros-args -r image:=/left/image_uncompressed

```

Copy to clipboard







```
ros2 run image_view image_view --ros-args -r image:=/right/image_uncompressed

```

Copy to clipboard



For example, the result looks like:
![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/compression/h264/realsense_example.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/compression/h264/realsense_example.png/)

## API [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html\#api "Link to this heading")

### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html\#ros-parameters "Link to this heading")

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `input_width` | `uint32_t` | `1920` | The width of the input image. |
| `input_height` | `uint32_t` | `1200` | The height of the input image. |
| `qp` | `uint32_t` | `20` | The encoder constant QP value. |
| `hw_preset` | `uint32_t` | `0` | The encoder hardware preset type. The value can be an integer from `0` to `3`, representing `Ultrafast`, `Fast`, `Medium` and, `Slow`, respectively. |
| `profile` | `uint32_t` | `0` | The profile to be used for encoding. The value can be an integer from `0` to `2`, representing `Main`, `Baseline`, and `High`, respectively. |
| `iframe_interval` | `int32_t` | `5` | Interval between two I frames, in number of frames. E.g., `iframe_interval=5` represents `IPPPPI...`, `iframe_interval=1` represents I frame only |
| `config` | `std::string` | `pframe_cqp` | A preset combination of `qp`, `hw_preset`, `profile` and `iframe_interval`. The value can be `iframe_cqp`, `pframe_cqp` or `custom`. When `iframe_cqp` or `pframe_cqp` is used, the default value of these four parameters will be applied. Only when this field is `custom` will the custom values will be used. |

### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html\#ros-topics-subscribed "Link to this heading")

| ROS Topic | Type | Description |
| --- | --- | --- |
| `image_raw` | [sensor\_msgs/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | Raw input image. |

Warning

All input images are required to have height and width that are both an even number of pixels.

### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html\#ros-topics-published "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `image_compressed` | [sensor\_msgs/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg) | H.264 compressed image. |

### Input Restrictions [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html\#input-restrictions "Link to this heading")

1. The input image resolution must be the same as the dimension you
provided, and the resolution must be **no larger than**
**\`\`1920x1200\`\`**.

2. The input image should be in `rgb8` or `bgr8` format, and it will
be converted to `nv12` format before being sent to the encoder.


### Output Interpretations [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html\#output-interpretations "Link to this heading")

1. The encoder could perform All-I frame or P-frame encoding and output
the H.264 compressed data. The input and output are one-to-one
mapped.


Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_compression/isaac_ros_h264_encoder/index.html)