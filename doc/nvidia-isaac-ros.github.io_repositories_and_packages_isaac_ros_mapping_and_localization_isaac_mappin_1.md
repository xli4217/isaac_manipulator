- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Mapping And Localization](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/index.html)
- Isaac Mapping ROS
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/index.rst.txt)

* * *

# Isaac Mapping ROS [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/index.html\#package-name "Link to this heading")

Note

This package is only released as a Debian package and is not available in source form.

## Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/index.html\#overview "Link to this heading")

This package contains a thin ROS 2 wrapper for ISAAC Mapping package to expose the built libraries
and headers to ROS 2 ecosystem.

It also contains tools for running the Isaac Mapping applications, including:

- `rosbag_to_mapping_data`: a tool to convert rosbags to Isaac Mapping format, which is used for cuVGL map creation.

- `rosbag_poses_to_tum_format`: a tool to convert pose messages in the rosbag to [TUM](https://cvg.cit.tum.de/data/datasets/rgbd-dataset/file_formats) format, where each line contains a single pose of `timestamp tx ty tz qx qy qz qw`.

- `run_rosbag_to_mapping_data`: a wrapper for running `rosbag_poses_to_tum_format` with default parameters.


## Installation [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/index.html\#installation "Link to this heading")

### Set Up Development Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/index.html\#set-up-development-environment "Link to this heading")

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


**For Nova Carter Setup** follow: [Nova Carter Getting Started](https://nvidia-isaac-ros.github.io/robots/nova_carter/getting_started.html).

**For x86 Rosbag Replay Testing** follow:

> 1. Add your data volume to the Docker container by adding the following line to the `.isaac_ros_dev-dockerargs` file under scripts, for example:
>
>
> > ```
> > echo -e '-v /mnt/nova_ssd/recordings:/mnt/nova_ssd/recordings' > ${ISAAC_ROS_WS}/src/isaac_ros_common/scripts/.isaac_ros_dev-dockerargs
> >
> > ```
> >
> > Copy to clipboard
>
> 2. Add the extra image layer that is needed for mapping and localization. If you are not setting up the Nova Carter robot, remove `nova_carter` from `CONFIG_IMAGE_KEY`.
>
>
> > ```
> > echo -e "CONFIG_IMAGE_KEY=ros2_humble\nCONFIG_DOCKER_SEARCH_DIRS=(../docker)" > ${ISAAC_ROS_WS}/src/isaac_ros_common/scripts/.isaac_ros_common-config
> >
> > ```
> >
> > Copy to clipboard
>
> 3. Launch the Docker container using the `run_dev.sh` script:
>
>
> > ```
> > cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
> > ./scripts/run_dev.sh
> >
> > ```
> >
> > Copy to clipboard

### Install the Package [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/index.html\#install-the-package "Link to this heading")

1. Launch dev container and install it with:


> ```
> sudo apt-get update
>
> ```
>
> Copy to clipboard


> ```
> sudo apt-get install -y ros-humble-isaac-mapping-ros
>
> ```
>
> Copy to clipboard

## Tools [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/index.html\#tools "Link to this heading")

After building the package, you can use `--help` to find the command line flags by using:

```
ros2 run isaac_mapping_ros <Binary Name> --help

```

Copy to clipboard

For example:

```
ros2 run isaac_mapping_ros rosbag_to_mapping_data --help

```

Copy to clipboard

### rosbag\_to\_mapping\_data [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/index.html\#rosbag-to-mapping-data "Link to this heading")

This tool converts rosbags to Isaac Mapping format. It can either extract raw images from rosbags or extract
keyframes from them.

To extract raw images from the rosbag, use the following command:

```
ros2 run isaac_mapping_ros rosbag_to_mapping_data \
    --sensor_data_bag_file <path_to_rosbag> \
    --pose_bag_file <output_dir> \
    --output_folder_path <output_dir> \
    --pose_topic_name <pose_topic_name>

```

Copy to clipboard

To extract keyframes with image features from the rosbag, use the following command. The feature type is sift.

```
ros2 run isaac_mapping_ros rosbag_to_mapping_data \
    --sensor_data_bag_file <path_to_rosbag> \
    --pose_bag_file <output_dir> \
    --pose_topic_name <pose_topic_name> \
    --output_folder_path <keyframe_dir> \
    --keyframe_topic_name <keyframe_topic_name> \
    --extract_feature=true \
    --keypoint_creation_config <keypoint_creation_config>

```

Copy to clipboard

These are the available options:

- `sensor_data_bag_file`: required, the path to the bag file that contains sensor data. Default value: “”.

- `output_folder_path`: required, the path to the converted folder. Default value: “”.

- `rectify_images`: optional, if to rectify images. Default value: true.

- `sample_sync_threshold_microseconds`: optional, the max timestamp differences of two images to be considered the same sample. Default value: 50 microseconds.

- `extracted_image_dir`: optional, if specified, use images under this directory instead of parsing it from sensor data bag. Default value: “”.

- `expected_pose_child_frame_name`: optional, the expected pose message’s child frame name, if the name doesn’t match, it raises error. Default value: `base_link`.


For sampling images:

- `pose_bag_file`: optional, the path to the bag file that contains poses for keyframe selection. Default value: “”.

- `pose_topic_name`: optional, the topic name of the pose to use. Default value: `/visual_slam/vis/slam_odometry`.

- `tum_pose_file`: optional, the path to the TUM pose file that contains poses for keyframe selection. Default value: “”.

- `min_inter_frame_distance`: optional, minimal inter-frame distance between two key frames. Default value: 0.05 meters.

- `min_inter_frame_rotation_degrees`: optional, the minimal inter-frame rotation between two key frames. Default value: 1 degree.


For configuring feature extraction:

- `extract_feature`: optional, whether to directly extract keyframes to the output folder, instead of saving the raw images. Default value: false.

- `keypoint_creation_config`: required, the config file of keypoint creation. Default value: “”.


### run\_rosbag\_to\_mapping\_data [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/index.html\#run-rosbag-to-mapping-data "Link to this heading")

This script is a wrapper for the `rosbag_to_mapping_data` tool. It is used to run the tool with the default parameters.

To run:

```
ros2 run isaac_mapping_ros run_rosbag_to_mapping_data.py \
    --sensor_data_bag <path_to_rosbag> \
    --output_folder <output_dir> \
    --pose_bag <output_dir>

```

Copy to clipboard

### rosbag\_poses\_to\_tum\_format [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/index.html\#rosbag-poses-to-tum-format "Link to this heading")

This tool converts the pose messages in the rosbag to TUM format.

```
ros2 run isaac_mapping_ros rosbag_poses_to_tum_format \
  --pose_bag_file <path_to_pose_bag> \
  --pose_topic_name <pose_topic_name> \
  --output_tum_file <output_tum_file>

```

Copy to clipboard

These are available options:

- `pose_bag_file`: required, the path to the bag file that contains pose messages. Default value: “”.

- `output_tum_file`: required, the file path where poses will be written. Default value: “”.

- `pose_topic_name`: optional, the topic name of the pose to use. Default value: `/visual_slam/vis/slam_odometry`.

- `base_link_name`: optional, the name of the base link, or the vehicle coordinate. Default value: `base_link`.


Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/index.html)