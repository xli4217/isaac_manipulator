- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS DNN Stereo Depth](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/index.html)
- `isaac_ros_ess`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.rst.txt)

* * *

# `isaac_ros_ess` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_dnn_stereo_depth/blob/main/isaac_ros_ess).

## Quickstart [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#quickstart "Link to this heading")

### Set Up Development Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#set-up-development-environment "Link to this heading")

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


### Download Quickstart Assets [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#download-quickstart-assets "Link to this heading")

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
PACKAGE_NAME="isaac_ros_ess"
NGC_RESOURCE="isaac_ros_ess_assets"
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


### Build `isaac_ros_ess` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#build-package-name "Link to this heading")

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
sudo apt-get install -y ros-humble-isaac-ros-ess && \
      sudo apt-get install -y ros-humble-isaac-ros-ess-models-install

```

Copy to clipboard

3. Download and install the pre-trained ESS model files:


> ```
> sudo apt-get update
>
> ```
>
> Copy to clipboard






```
ros2 run isaac_ros_ess_models_install install_ess_models.sh --eula

```

Copy to clipboard


Note

Limitations on x86\_64: ESS plugins only run with GPU with sm80 and above. This limits the GPU devices on x86\_64 to devices with compute\_80 and above.
For CUDA compute capability details, please refer to [cuda-gpus](https://developer.nvidia.com/cuda-gpus).

### Run Launch File [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#run-launch-file "Link to this heading")

RosbagRealSense CameraHawk CameraZED Camera

1. Continuing inside the Docker container, install the following dependencies:


> ```
> sudo apt-get update
>
> ```
>
> Copy to clipboard






```
sudo apt-get install -y ros-humble-isaac-ros-examples

```

Copy to clipboard

2. Run the following launch file to spin up a demo using quickstart rosbag:

To run ESS at a threshold of 0.0 (fully dense output):





```
ros2 launch isaac_ros_examples isaac_ros_examples.launch.py launch_fragments:=ess_disparity \
      engine_file_path:=${ISAAC_ROS_WS:?}/isaac_ros_assets/models/dnn_stereo_disparity/dnn_stereo_disparity_v4.1.0_onnx/ess.engine \
      threshold:=0.0

```

Copy to clipboard

3. Open a **second terminal** and attach to the container:


> ```
> cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
>    ./scripts/run_dev.sh
>
> ```
>
> Copy to clipboard

4. In the **second terminal**, play the ESS sample rosbag downloaded in the quickstart assets:


> ```
> ros2 bag play -l ${ISAAC_ROS_WS}/isaac_ros_assets/isaac_ros_ess/rosbags/ess_rosbag \
>    --remap /left/camera_info:=/left/camera_info_rect /right/camera_info:=/right/camera_info_rect
>
> ```
>
> Copy to clipboard

### Visualize Output [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#visualize-output "Link to this heading")

Visualizer scriptFoxglove

1. Open a **terminal** and attach to the container:


> ```
> cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
>    ./scripts/run_dev.sh
>
> ```
>
> Copy to clipboard

2. In the **terminal**, visualize and validate the disparity output using the visualizer script:


> ```
> ros2 run isaac_ros_ess isaac_ros_ess_visualizer.py
>
> ```
>
> Copy to clipboard
>
> With threshold set to 0.4, the example result is:
>
> ![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/warehouse_conf0.4.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/warehouse_conf0.4.png/)
>
> With threshold set to 0.0, the example result is:
>
> ![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/warehouse_conf0.0.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/warehouse_conf0.0.png/)

## Try More Examples [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#try-more-examples "Link to this heading")

To continue your exploration, check out the following suggested examples:

- [Tutorial with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/ess/tutorial_isaac_sim.html)
- [Tutorial with HAWK with Wide FoV](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/ess/tutorial_hawk_wide_fov.html)
- [Visualize ESS Disparity Output](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/ess/visualize_image.html)

## Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#troubleshooting "Link to this heading")

### Package not found while launching the visualizer script [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#package-not-found-while-launching-the-visualizer-script "Link to this heading")

#### Symptom [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#symptom "Link to this heading")

```
$ ros2 run isaac_ros_ess isaac_ros_ess_visualizer.py
Package 'isaac_ros_ess' not found

```

Copy to clipboard

#### Solution [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#solution "Link to this heading")

Use the `colcon build --packages-up-to isaac_ros_ess` command to build
`isaac_ros_ess`; do not use the `--symlink-install` option. Run
`source install/setup.bash` after the build.

### Problem reserving CacheChange in reader [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#problem-reserving-cachechange-in-reader "Link to this heading")

#### Symptom [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#id1 "Link to this heading")

When using a rosbag as input, `isaac_ros_ess` throws an error if the
input topics are published too fast:

```
[component_container-1] 2022-06-24 09:04:43.584 [RTPS_MSG_IN Error] (ID:281473268431152) Problem reserving CacheChange in reader: 01.0f.cd.10.ab.f2.65.b6.01.00.00.00|0.0.20.4 -> Function processDataMsg

```

Copy to clipboard

#### Solution [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#id2 "Link to this heading")

Make sure that the rosbag has a reasonable size and publish rate.

### Isaac ROS Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#isaac-ros-troubleshooting "Link to this heading")

For solutions to problems with Isaac ROS, please check [here](https://nvidia-isaac-ros.github.io/troubleshooting/index.html).

## API [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#api "Link to this heading")

### Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#overview "Link to this heading")

The `isaac_ros_ess` package offers functionality to generate a stereo
disparity map from stereo images using a trained ESS model. Given a pair
of stereo input images, the package generates a continuous disparity
image for the left input image.

### Usage [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#usage "Link to this heading")

```
ros2 launch isaac_ros_ess isaac_ros_ess.launch.py engine_file_path:=<your ESS engine plan absolute path>

```

Copy to clipboard

### ESSDisparityNode [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#essdisparitynode "Link to this heading")

#### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#ros-parameters "Link to this heading")

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `engine_file_path` | `string` | N/A - Required | The absolute path to the ESS engine file. |
| `threshold` | `double` | 0.40 | Threshold value ranges between 0.0 and 1.0 for filtering disparity with confidence. Pixels with confidence less than threshold will be marked as invalid in the disparity output. Value 0.0 means a fully dense disparity output. |
| `image_type` | `string` | `"RGB_U8"`. | The input image encoding type. Supports `"RGB_U8"` and `"BGR_U8"`. Note that if this parameter is not specified and there is an upstream Isaac ROS NITROS node, the type will be decided automatically. |

#### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#ros-topics-subscribed "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `left/image_rect` | [sensor\_msgs/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | The left image of a stereo pair. |
| `right/image_rect` | [sensor\_msgs/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | The right image of a stereo pair. |
| `left/camera_info` | [sensor\_msgs/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | The left camera model. |
| `right/camera_info` | [sensor\_msgs/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | The right camera model. |

Note

The images on input topics ( `left/image_rect` and `right/image_rect`) should be a color image either in `rgb8` or `bgr8` format.

#### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#ros-topics-published "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `disparity` | [stereo\_msgs/DisparityImage](https://github.com/ros2/common_interfaces/blob/humble/stereo_msgs/msg/DisparityImage.msg) | The continuous stereo disparity estimation. |

#### Input Restrictions [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#input-restrictions "Link to this heading")

1. The input left and right images must have the **same dimension and**
**resolution**, and the resolution must be **no larger than**
**1920x1200**.

2. Each input pair ( `left/image_rect`, `right/image_rect`,
`left/camera_info` and `right/camera_info`) should have the
**same timestamp**; otherwise, the synchronizing module inside the
ESS Disparity Node will drop the input with smaller timestamps.


#### Output Interpretations [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html\#output-interpretations "Link to this heading")

1. The `isaac_ros_ess` package outputs a disparity image with dimension same as the ESS model output dimension.




| ESS Model | Output Dimension |
| --- | --- |
| `ess.onnx` | 960 x 576 |
| `light_ess.onnx` | 480 x 288 |



The input images are rescaled to the ESS model input dimension
before inferencing. There are two outputs from the ESS model
with the same dimension: disparity output and confidence output.
The disparity is filtered with confidence using a pre-configured threshold.
Pixels with confidence less than the threshold is replaced with -1.0 as
invalid before the inference result is published. For fully dense disparity output
without confidence thresholding, set the threshold to 0.0.

2. The left and right `CameraInfo` are used to composite a
`stereo_msgs/DisparityImage`. If you only care about the disparity
image, and don’t need the baseline and focal length information, you
can pass dummy camera messages.


Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html)