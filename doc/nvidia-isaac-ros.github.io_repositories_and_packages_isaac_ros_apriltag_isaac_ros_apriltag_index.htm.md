- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS AprilTag](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/index.html)
- `isaac_ros_apriltag`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.rst.txt)

* * *

# `isaac_ros_apriltag` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_apriltag/blob/main/isaac_ros_apriltag).

## Quickstart [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html\#quickstart "Link to this heading")

### Set Up Development Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html\#set-up-development-environment "Link to this heading")

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


### Download Quickstart Assets [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html\#download-quickstart-assets "Link to this heading")

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
PACKAGE_NAME="isaac_ros_apriltag"
NGC_RESOURCE="isaac_ros_apriltag_assets"
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


### Setup Jetson device [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html\#setup-jetson-device "Link to this heading")

> Setting up Jetson for using VPI. See [here](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/jetson_vpi.html)

### Build `isaac_ros_apriltag` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html\#build-package-name "Link to this heading")

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
sudo apt-get install -y ros-humble-isaac-ros-apriltag

```

Copy to clipboard


### Run Launch File [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html\#run-launch-file "Link to this heading")

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

2. Run the following launch file to spin up a demo of this package using the quickstart rosbag:





```
ros2 launch isaac_ros_examples isaac_ros_examples.launch.py launch_fragments:=apriltag interface_specs_file:=${ISAAC_ROS_WS}/isaac_ros_assets/isaac_ros_apriltag/quickstart_interface_specs.json

```

Copy to clipboard

3. Open a **second** terminal inside the Docker container:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
./scripts/run_dev.sh

```

Copy to clipboard

4. Run the rosbag file to simulate an image stream:





```
ros2 bag play -l ${ISAAC_ROS_WS}/isaac_ros_assets/isaac_ros_apriltag/quickstart.bag --remap image:=image_rect camera_info:=camera_info_rect

```

Copy to clipboard


### Visualize Results [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html\#visualize-results "Link to this heading")

1. Open a **new** terminal inside the Docker container:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
      ./scripts/run_dev.sh

```

Copy to clipboard

2. Observe the AprilTag detection output `/tag_detections` on a separate terminal with the command:





```
ros2 topic echo /tag_detections

```

Copy to clipboard


Note

You must to calibrate the intrinsics of your camera if you want the node to determine 3D poses for tags instead of just detection and corners as 2D pixel coordinates.
See [calibration](https://docs.nav2.org/tutorials/docs/camera_calibration.html) for more details.

## Try More Examples [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html\#try-more-examples "Link to this heading")

To continue your exploration, check out the following suggested examples:

- [Tutorial with a USB camera](https://nvidia-isaac-ros.github.io/concepts/fiducials/apriltag/tutorial_usb_cam.html)
- [Tutorial with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/fiducials/apriltag/tutorial_isaac_sim.html)
- [Tutorial with NITROS ROS 1 Bridge](https://nvidia-isaac-ros.github.io/concepts/fiducials/apriltag/tutorial_apriltag_ros1_bridge.html)

## Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html\#troubleshooting "Link to this heading")

### Isaac ROS Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html\#isaac-ros-troubleshooting "Link to this heading")

For solutions to problems with Isaac ROS, see [troubleshooting](https://nvidia-isaac-ros.github.io/troubleshooting/index.html).

## API [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html\#api "Link to this heading")

### Usage [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html\#usage "Link to this heading")

```
ros2 launch isaac_ros_apriltag isaac_ros_apriltag.launch.py

```

Copy to clipboard

### Replacing `apriltag_ros` with `isaac_ros_apriltag` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html\#replacing-apriltag-ros-with-isaac-ros-apriltag "Link to this heading")

1. Add a dependency on `isaac_ros_apriltag` to
`your_package/package.xml` and `your_package/CMakeLists.txt`. The
original `apriltag_ros` dependency may be removed entirely.

2. Change the package and plugin names in any `*.launch.py` launch
files to use `isaac_ros_apriltag` and
`nvidia::isaac_ros::apriltag::AprilTagNode`, respectively.


#### Supported Packages [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html\#supported-packages "Link to this heading")

The packages under the standard `apriltag_ros` have the
following support:

| Existing Package | Isaac ROS Alternative |
| --- | --- |
| `apriltag_ros` | See `isaac_ros_apriltag` |
| `image_pipeline` | See `isaac_ros_image_pipeline` |

### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html\#ros-parameters "Link to this heading")

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `size` | `double` | `0.22` | The tag edge size in meters, assuming square markers. More [details here](https://github.com/AprilRobotics/apriltag). For example, `0.22` |
| `max_tags` | `int` | `64` | The maximum number of tags to be detected. For example, `64` |
| `tile_size` | `uint` | `4` | Tile/window size used for adaptive thresholding in pixels. For example, `4` |
| `tag_family` | `string` | `tag36h11` | Tag family to detect. CUDA backend only supports `tag36h11`. CPU and PVA backends support `tag36h11`, `tag16h5`, `tag25h9`, `tag36h10`, `tag36h11`, `circle21h7`, `circle49h12`, `custom48h12`, `standard41h12`, `standard52h13` |
| `backends` | `string` | `CUDA` | Backend to perform detection with. Options include `CPU`, `CUDA`, `PVA` |

### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html\#ros-topics-subscribed "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `image` | [sensor\_msgs/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) | The input camera stream. |
| `camera_info` | [sensor\_msgs/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | The input camera intrinsics stream. |

### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html\#ros-topics-published "Link to this heading")

| ROS Topic | Type | Description |
| --- | --- | --- |
| `tag_detections` | [isaac\_ros\_apriltag\_interfaces/AprilTagDetectionArray](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common/blob/main/isaac_ros_apriltag_interfaces/msg/AprilTagDetectionArray.msg) | The detection message array. |
| `tf` | [tf2\_msgs/TFMessage](https://github.com/ros2/geometry2/blob/ros2/tf2_msgs/msg/TFMessage.msg) | Pose of all detected AprilTags ( `TagFamily:ID`) w.r.t to the camera topic `frame_id`. |

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html)