- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Image Pipeline](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/index.html)
- `isaac_ros_stereo_image_proc`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_stereo_image_proc/index.rst.txt)

* * *

# `isaac_ros_stereo_image_proc` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_stereo_image_proc/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_image_pipeline/blob/main/isaac_ros_stereo_image_proc).

## Quickstart [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_stereo_image_proc/index.html\#quickstart "Link to this heading")

### Set Up Development Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_stereo_image_proc/index.html\#set-up-development-environment "Link to this heading")

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


### Download Quickstart Assets [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_stereo_image_proc/index.html\#download-quickstart-assets "Link to this heading")

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
PACKAGE_NAME="isaac_ros_stereo_image_proc"
NGC_RESOURCE="isaac_ros_stereo_image_proc_assets"
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


### Build `isaac_ros_stereo_image_proc` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_stereo_image_proc/index.html\#build-package-name "Link to this heading")

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
sudo apt-get install -y  ros-humble-isaac-ros-image-proc ros-humble-isaac-ros-stereo-image-proc

```

Copy to clipboard


### Run Launch File [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_stereo_image_proc/index.html\#run-launch-file "Link to this heading")

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
sudo apt-get install -y ros-humble-isaac-ros-examples ros-humble-isaac-ros-realsense ros-humble-isaac-ros-depth-image-proc ros-humble-isaac-ros-image-proc

```

Copy to clipboard

3. Run the launch file, which launches the example, and wait for 10
seconds.





```
ros2 launch isaac_ros_examples isaac_ros_examples.launch.py launch_fragments:=realsense_stereo_rect,disparity,disparity_to_depth,point_cloud_xyz

```

Copy to clipboard

4. Observe point cloud output `/points` on a
separate terminal with the command:





```
ros2 topic echo /points

```

Copy to clipboard


Note

For RealSense camera package issues, please
refer to the section
[here](https://nvidia-isaac-ros.github.io/troubleshooting/hardware_setup.html#realsense-driver-does-not-work-with-ros-2-humble).

Other supported cameras can be found
[here](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_argus_camera/index.html#reference-cameras).

## Try More Examples [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_stereo_image_proc/index.html\#try-more-examples "Link to this heading")

To continue your exploration, check out the following suggested examples:

- [Tutorial with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/sgm/tutorial_isaac_sim.html)

## API [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_stereo_image_proc/index.html\#api "Link to this heading")

### Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_stereo_image_proc/index.html\#overview "Link to this heading")

The `isaac_ros_stereo_image_proc` package offers functionality for
handling image pairs from a binocular/stereo camera setup, calculating
the disparity between the two images, and producing a point cloud with
depth information. It largely replaces the `stereo_image_proc`
package.

### Available Components [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_stereo_image_proc/index.html\#available-components "Link to this heading")

| Component | Topics Subscribed | Topics Published | Parameters |
| --- | --- | --- | --- |
| `DisparityNode` | `left/image_rect`, `left/camera_info`: The left camera stream `right/image_rect`, `right/camera_info`: The right camera stream | `disparity`: The disparity between the two cameras | `max_disparity`: The maximum value for disparity per pixel, which is 64 by default. With ORIN backend, this value must be 128 or 256. `backends`: The VPI backend to use, which is CUDA by default (options: “CUDA”, “XAVIER”, “ORIN”) `confidence_threshold`: The confidence threshold for VPI SGM algorithm `window_size`: The window size for SGM disparity calculation `num_passes`: The number of passes SGM takes to compute result `p1`: Penalty on disparity changes of +/- 1 between neighbor pixels `p2`: Penalty on disparity changes of more than 1 between neighbor pixels `p2_alpha`: Alpha for P2 `quality`: Quality of disparity output. It’s only applicable when using XAVIER backend. The higher the value, better the quality and possibly slower performance. **Refer to** [this VPI doc](https://docs.nvidia.com/vpi/group__VPI__StereoDisparityEstimator.html) **for more details on the parameters** |
| `PointCloudNode` | `left/image_rect_color`: The coloring for the point cloud `left/camera_info`: The left camera info `right/camera_info`: The right camera info `disparity` The disparity between the two cameras | `points`: The output point cloud | `use_color`: Whether or not the output point cloud should have color. The default value is false. `unit_scaling`: The amount to scale the `xyz` points by |
| `DisparityToDepthNode` | `disparity` The disparity image | `depth` The resultant depth image | N/A |

Note

`DisparityNode` with the `ORIN` backend requires a `max_disparity` value of 128 or 256, but the default value is 64. Besides, the `ORIN` backend requires `nv12` input image format, you can use the `ImageFormatConverterNode` to convert the input to `nv12` format.

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_stereo_image_proc/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_stereo_image_proc/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_stereo_image_proc/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_stereo_image_proc/index.html)