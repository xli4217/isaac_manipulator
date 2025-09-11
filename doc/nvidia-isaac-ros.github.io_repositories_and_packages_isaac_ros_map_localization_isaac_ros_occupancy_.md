- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Map Localization](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/index.html)
- `isaac_ros_occupancy_grid_localizer`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.rst.txt)

* * *

# `isaac_ros_occupancy_grid_localizer` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_map_localization/blob/main/isaac_ros_occupancy_grid_localizer).

## Quickstart [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html\#quickstart "Link to this heading")

### Set Up Development Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html\#set-up-development-environment "Link to this heading")

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


### Download Quickstart Assets [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html\#download-quickstart-assets "Link to this heading")

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
PACKAGE_NAME="isaac_ros_occupancy_grid_localizer"
NGC_RESOURCE="isaac_ros_occupancy_grid_localizer_assets"
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


### Build `isaac_ros_occupancy_grid_localizer` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html\#build-package-name "Link to this heading")

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
sudo apt-get install -y ros-humble-isaac-ros-occupancy-grid-localizer

```

Copy to clipboard


### Run Launch File [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html\#run-launch-file "Link to this heading")

1. Continuing inside the Docker container, `rviz2`:





```
rviz2 -d  $(ros2 pkg prefix isaac_ros_occupancy_grid_localizer --share)/rviz/quickstart.rviz

```

Copy to clipboard

2. Create another terminal in the Docker container using the
`run_dev.sh` script:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
     ./scripts/run_dev.sh

```

Copy to clipboard

3. Run the launch file to spin up a demo of this package:





```
ros2 launch isaac_ros_occupancy_grid_localizer isaac_ros_occupancy_grid_localizer_quickstart.launch.py

```

Copy to clipboard

4. Create another terminal in the Docker container using the
`run_dev.sh` script:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
     ./scripts/run_dev.sh

```

Copy to clipboard

5. Run the rosbag:


> ```
> ros2 bag play -l ${ISAAC_ROS_WS}/isaac_ros_assets/isaac_ros_occupancy_grid_localizer/rosbags/flatscan
>
> ```
>
> Copy to clipboard

6. Create another terminal in the Docker container using the

`run_dev.sh` script:







```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
     ./scripts/run_dev.sh

```

Copy to clipboard

7. Trigger the localization using a command line service call:


> ```
> ros2 service call trigger_grid_search_localization std_srvs/srv/Empty {}
>
> ```
>
> Copy to clipboard

8. Verify that you see a frame being generated in the map showing the

position of the LIDAR.


[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_map_localization/quickstart.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_map_localization/quickstart.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_map_localization/quickstart.png/)

## Try More Examples [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html\#try-more-examples "Link to this heading")

To continue your exploration, check out the following suggested examples:

- [Tutorial with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/localization/lidar/tutorial_isaac_sim.html)

## Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html\#troubleshooting "Link to this heading")

### Isaac ROS Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html\#isaac-ros-troubleshooting "Link to this heading")

For solutions to problems with Isaac ROS, see [here](https://nvidia-isaac-ros.github.io/troubleshooting/index.html).

## API [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html\#api "Link to this heading")

### Usage [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html\#usage "Link to this heading")

```
ros2 launch isaac_ros_occupancy_grid_localizer isaac_ros_occupancy_grid_localizer.launch.py

```

Copy to clipboard

Note

Use the `flatscan` topic with the
`trigger_grid_search_localization` service to trigger localization
using a service.

**Or** publish directly to the `flatscan_localization` topic to
trigger localization every time a FlatScan message is received on
this topic.

**Do not** publish FlatScan messages to both `flatscan` and
`flatscan_localization` topics.

### OccupancyGridLocalizerNode [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html\#occupancygridlocalizernode "Link to this heading")

#### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html\#ros-parameters "Link to this heading")

Note

The ROS parameter names are the same as the Nav2
[map\_server](https://github.com/ros-planning/navigation2/blob/main/nav2_map_server/src/map_io.cpp#L136)
YAML parameters. This allows to load and pass the same YAML file to both Nav2 and
`isaac_ros_occupancy_grid_localizer` as shown in the
[Isaac Sim Launch File](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_map_localization/blob/main/isaac_ros_occupancy_grid_localizer/launch/isaac_ros_occupancy_grid_localizer_nav2.launch.py)

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `loc_result_frame` | `std::string` | `map` | frame\_id of localization result |
| `resolution` | `double` | `0.05` | The meters per pixel of the `.png` map being loaded. This parameter is loaded from the the map YAML file. |
| `origin` | `std::vector<double>` | `[0.0, 0.0, 0.0]` | The origin of the map loaded. Used to transform the output to compensate for the same transform made to the PNG file loaded by the Nav2 [map\_server](https://github.com/ros-planning/navigation2/blob/main/nav2_map_server/src/map_io.cpp#L136). |
| `occupied_thresh` | `double` | `0.65` | Pixels with occupancy probability greater than this threshold are considered completely occupied. This parameter is loaded from the the map YAML file. Supported values: `[0,1)` |\
| `image` | `std::string` | `""` | Name of the PNG file used to load map. This should be in the same directory as the map YAML file specified in `map_yaml_path`. This parameter is loaded from the the map YAML file. |\
| `map_yaml_path` | `std::string` | `""` | Absolute path to the map YAML file. From which we load the `resolution` and `occupied_thresh` |\
| `max_points` | `int` | `20000` | Maximum number of points in FlatScan Message that can be received used to pre-allocate GPU memory. |\
| `robot_radius` | `double` | `0.25` | The radius of the robot. This parameter is used to exclude poses which are too close to an obstacle.memory. |\
| `min_output_error` | `double` | `0.22` | The minimal output error used to normalize and compute confidence, if output error from best sample smaller or equal to this, the confidence is 1 |\
| `max_output_error` | `double` | `0.35` | The max output error from our best sample, if output error larger than this threshold, we conclude localization failed |\
| `max_beam_error` | `double` | `0.5` | The maximum beam error used when comparing range scans. |\
| `num_beams_gpu` | `int` | `512` | The GPU accelerated scan-and-match function can only handle a certain number of beams per range scan. The allowed values are {32, 64, 128, 256, 512}. If the number of beams in the range scan does not match this number a subset of beams will be taken. |\
| `batch_size` | `int` | `512` | This is the number of scans to collect into a batch for the GPU kernel. Choose a value which matches your GPU well. |\
| `sample_distance` | `double` | `0.1` | Distance between sample points in meters. The smaller this number, the more sample poses will be considered. This leads to a higher accuracy and lower performance. |\
| `out_of_range_threshold` | `double` | `100.0` | Points range larger than this threshold will be marked as out of range and not used. |\
| `invalid_range_threshold` | `double` | `0.0` | Points range smaller than this threshold will be marked as invalid and not used. |\
| `min_scan_fov_degrees` | `double` | `270.0` | Minimal required scan FoV to run the localizer. |\
| `use_closest_beam` | `bool` | `true` | Whether or not pick the closest angle beam in angle bucket, if not pick the average within an angular bucket |\
\
#### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html\#ros-topics-subscribed "Link to this heading")\
\
| ROS Topic | Type | Description |\
| --- | --- | --- |\
| `flatscan` | [isaac\_ros\_pointcloud\_interfaces::msg::FlatScan](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common/blob/main/isaac_ros_pointcloud_interfaces/msg/FlatScan.msg) | The input FlatScan messages buffer. The last message on this topic will be used as input for localization when the `trigger_grid_search_localization` service is called. |\
| `flatscan_localization` | [isaac\_ros\_pointcloud\_interfaces::msg::FlatScan](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common/blob/main/isaac_ros_pointcloud_interfaces/msg/FlatScan.msg) | The topic to trigger localization directly without a buffer. Localization will be triggered every time a FlatScan message is received on this topic. |\
\
| ROS Topic | Interface | Description |\
| --- | --- | --- |\
| `localization_result` | [geometry\_msgs::msg::PoseWithCovarianceStamped](https://github.com/ros2/common_interfaces/blob/humble/geometry_msgs/msg/PoseWithCovarianceStamped.msg) | Pose of the scan data with respect to the map origin, as specified in the first note in the [overview section](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html#overview) |\
\
#### ROS Services Advertised [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html\#ros-services-advertised "Link to this heading")\
\
| ROS Service | Interface | Description |\
| --- | --- | --- |\
| `trigger_grid_search_localization` | [std\_srvs::srv::Empty](https://github.com/ros2/common_interfaces/blob/humble/std_srvs/srv/Empty.srv) | The service to trigger the global localization using the last scan received on the `flatscan` input topic. |\
\
Version:\
v: latest\
\
\
Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_map_localization/isaac_ros_occupancy_grid_localizer/index.html)