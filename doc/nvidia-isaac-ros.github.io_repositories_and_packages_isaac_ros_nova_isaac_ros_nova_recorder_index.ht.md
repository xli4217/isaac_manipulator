- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Nova](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/index.html)
- `isaac_ros_nova_recorder`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.rst.txt)

* * *

# `isaac_ros_nova_recorder` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova/blob/main/isaac_ros_nova_recorder).

## Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html\#overview "Link to this heading")

The `isaac_ros_nova_recorder` package enables recording data from the Nova sensor set. For more information
on data recording, refer to [isaac\_ros\_data\_recorder](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_recorder/index.html).

## Quickstart [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html\#quickstart "Link to this heading")

### Set Up AWS (Optional) [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html\#set-up-aws-optional "Link to this heading")

1. Follow [Get Started With S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/GetStartedWithS3.html) until
[Step 1: Create your first S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/creating-bucket.html).
The bucket name will be used for the `s3_bucket` parameter when launching the data recorder.

2. Sign in to the AWS Management Console and open the IAM console at [https://console.aws.amazon.com/iam](https://console.aws.amazon.com/iam).

1. In the navigation pane of the IAM console, select Users and then select the User name of the user that you created previously.

2. On the user’s page, select the Security credentials page. Then, under Access keys, select Create access key.
3. Use the information from Step 2 to create the AWS credentials and configuration files.

1. Create `~/.aws/credentials` with the following content:





      ```
      [default]
      aws_access_key_id=<AWS_ACCESS_KEY_ID>
      aws_secret_access_key=<AWS_SECRET_ACCESS_KEY>

      ```

      Copy to clipboard

2. Create `~/.aws/config` with the following content:





      ```
      [default]
      region=<REGION>

      ```

      Copy to clipboard

### Set Up Development Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html\#set-up-development-environment "Link to this heading")

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


3. Create additional mounts for the Docker container.

If the container is already running, stop it with:





```
docker rm -f isaac_ros_dev-aarch64-container

```

Copy to clipboard



Create mounts for `/etc/nova`, `/mnt/nova_ssd/recordings`, and `~/.aws`:





```
echo $'-v /etc/nova:/etc/nova\n-v /mnt/nova_ssd/recordings:/mnt/nova_ssd/recordings\n-v `realpath ~/.aws`:/home/admin/.aws:ro' > ${ISAAC_ROS_WS}/src/isaac_ros_common/scripts/.isaac_ros_dev-dockerargs

```

Copy to clipboard


### Build `isaac_ros_nova_recorder` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html\#build-package-name "Link to this heading")

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
sudo apt-get install -y ros-humble-isaac-ros-nova-recorder

```

Copy to clipboard


### Run Launch File [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html\#run-launch-file "Link to this heading")

1. Continuing inside the Docker container, start data recording:


> ```
> ros2 launch isaac_ros_nova_recorder nova_recorder.launch.py config:=nova-carter s3_bucket:=<s3_bucket>
>
> ```
>
> Copy to clipboard

2. Once the launch process is complete, open a web browser and navigate to `https://<ROBOT_IP_ADDRESS>:8080` to access the data\_recorder UI.

1. Select Open connection to connect to the robot.
      ![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/open_connection.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/open_connection.png/)
2. Type in `ws://<ROBOT_IP_ADDRESS>:8765` in WebSocket URL.

3. Click the shield/triangle icon at the top right of the Chrome address bar when the page is blocked from connecting.

4. From the menu that appears, select `Load Unsafe Scripts` to allow the connection to the WebSocket.
      ![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/enable_websocket.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/enable_websocket.png/)
3. In the data\_recorder UI, to start a new recording session, follow these steps:

1. Input Author: Enter the name of the person or entity responsible for the recording.

2. Input Title: Provide a title for the recording session.

3. Input Location: Specify the location where the recording is being conducted.
4. Start Recording: Once all three fields (Author, Title, and Location) are filled out, the `Start` button will be enabled. Press “Start” to begin recording.

5. Stop Recording: To stop recording, use the `Stop` button in the UI.

6. Switch Preview: Click on the image panel, then navigate to the left sidebar and use the “Select Camera” drop-down to change the camera view.


> ![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/switch_preview.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/switch_preview.png/)

7. Upload Recorded Data: After stopping the recording, follow these steps to upload the recorded data:

1. Navigate to the `Upload` tab in the data\_recorder UI.

2. In the `Upload` tab, select the files you want to upload by clicking the check-boxes next to each file.

3. Once the files are selected, click the `Upload` button to start the upload process.

4. The upload status will be displayed in the **Status** column, showing stages like **PENDING**, the upload percentage, and **COMPLETE** when the upload is finished.
8. By default, recordings will be stored in `/mnt/nova_ssd/recordings`.


## Try More Examples [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html\#try-more-examples "Link to this heading")

### Event Recorder [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html\#event-recorder "Link to this heading")

Event recording enables recording data during specific events. To run the event recorder:

```
ros2 launch isaac_ros_nova_recorder nova_recorder.launch.py event_recorder:=True config:=nova-carter s3_bucket:=<s3_bucket>

```

Copy to clipboard

The Cross button on the joystick will signal the start of an event while the Square button will signal the end of an event.
By default, 60 seconds of data will be buffered and saved before an event starts while 60 seconds of data will be saved after an event ends.

Note

If a sensor configuration without `drivetrain` is used, then events will have to be
triggered using `ros2 service call` in a separate terminal:

```
ros2 service call event_start isaac_ros_data_recorder/srv/Event

```

Copy to clipboard

```
ros2 service call event_end isaac_ros_data_recorder/srv/Event

```

Copy to clipboard

### Sensor Configuration [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html\#sensor-configuration "Link to this heading")

A sensor configuration file can be used to record a subset of sensors on the robot. For example, to
record only the 4 stereo cameras:

```
ros2 launch isaac_ros_nova_recorder nova_recorder.launch.py config:=nova-carter_hawk-4_imu s3_bucket:=<s3_bucket>

```

Copy to clipboard

The following sensor configurations are provided. Custom sensor configurations are also supported.

| Configuration | Description |
| --- | --- |
| `hawk-1` | Front stereo camera. |
| `hawk-2` | Front and back stereo cameras. |
| `hawk-3` | Front, left, and right stereo cameras. |
| `hawk-4` | Front, left, right and back stereo cameras. |
| `owl-1` | Front fisheye camera. |
| `owl-2` | Front and back fisheye cameras. |
| `owl-3` | Front, left, and right fisheye cameras. |
| `owl-4` | Front, left, right, and back fisheye cameras. |
| `hawk-4_owl-4` | Front, left, right and back stereo and fisheye cameras. |
| `nova-carter` | Sensor configuration for Nova Carter. |
| `nova-carter_hawk-4_imu` | Sensor configuration for Nova Carter with only stereo cameras and IMU. |
| `nova-carter_owl-4_imu` | Sensor configuration for Nova Carter with only fisheye cameras and IMU. |
| `nova-benchtop` | Sensor configuration for Nova Carter without drivetrain. |
| `nova-benchtop_hawk-4_imu` | Sensor configuration for Nova Carter without drivetrain and only stereo cameras and IMU. |
| `nova-benchtop_owl-4_imu` | Sensor configuration for Nova Carter without drivetrain and only fisheye cameras and IMU. |
| `nova-developer-kit` | Sensor configuration for Nova Orin Developer Kit. |

### Sensor Monitoring [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html\#sensor-monitoring "Link to this heading")

To monitor the status of various sensors during a recording session, the Sensor Indicators Panel in
the data recorder UI provides real-time feedback on sensor health and connectivity.

> ![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/sensor_indicators.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/sensor_indicators.png/)

Sensor Health Color Code:
Green: OK
Yellow: Warning
Red: Error
Grey: Stale

Hovering over each sensor will display a tool-tip with detailed information, including frame rates
and the total number of dropped frames for each sensor.

> ![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/sensor_tooltip.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/sensor_tooltip.png/)

You can switch between two views using the button at the top of the panel:

Summary View: Groups related topics under each sensor. For example, both left/right camera\_info and
image\_raw topics are grouped under the front Hawk camera.
Full View: Displays all topics individually for each sensor.

> ![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/sensor_full_view.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/sensor_full_view.png/)

For more detailed sensor monitoring information, navigate to the `Diagnostics` tab at the top.
This section provides comprehensive details for each sensor.

> ![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/diagnostics.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/diagnostics.png/)

For more information, refer to Visualize Results and Track System Monitors in Isaac ROS nova doc

[isaac\_ros\_nova](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova/index.html).

### Recording Via Command Line [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html\#recording-via-command-line "Link to this heading")

The data recorder can be run in headless mode to record data using only the command line:

```
ros2 launch isaac_ros_nova_recorder nova_recorder.launch.py headless:=True config:=nova-carter s3_bucket:=<s3_bucket>

```

Copy to clipboard

This will start recording data immediately and will stop when the application is terminated via `Ctrl+C`.

## Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html\#troubleshooting "Link to this heading")

[Isaac ROS Troubleshooting](https://nvidia-isaac-ros.github.io/troubleshooting/index.html)

[AWS S3 Troubleshooting](https://docs.aws.amazon.com/AmazonS3/latest/userguide/troubleshooting.html)

If the Data Recorder Panel is not visible, click on `Layouts` in the left sidebar, right-click on `Default`, and select `Revert` to restore the default layout.

> ![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/revert_layout.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/revert_layout.png/)

## API [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html\#api "Link to this heading")

### Usage [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html\#usage "Link to this heading")

```
ros2 launch isaac_ros_nova_recorder nova_recorder.launch.py

```

Copy to clipboard

#### ROS Launch Arguments [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html\#ros-launch-arguments "Link to this heading")

| ROS Launch Argument | Default Value | Description |
| --- | --- | --- |
| `s3_bucket` | None | S3 bucket. |
| `config` | `/etc/nova/systeminfo.yaml` | Sensor configuration file. |
| `topics` | `[]` | Additional topics to record. |
| `recording_directory` | `/mnt/nova_ssd/recordings` | Recording directory. |
| `recording_name` | `rosbag2` | Recording name. |
| `headless` | `False` | Record data without the UI. |
| `event_recorder` | `False` | Enable event recording. |
| `encoder_qp` | `20` | H.264 encoder quality parameter, 0-50, higher values mean lower quality. |

Note

In low light or occluded conditions, the quality parameter may need to be adjusted to prevent frame drops

#### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html\#ros-topics-subscribed "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `/rosout` | [rcl\_interfaces/msg/Log](https://github.com/ros2/rcl_interfaces/blob/humble/rcl_interfaces/msg/Log.msg) | Console logs. |
| `/diagnostics` | [diagnostic\_msgs/msg/DiagnosticArray](https://github.com/ros2/common_interfaces/blob/humble/diagnostic_msgs/msg/DiagnosticArray.msg) | Sensor diagnostics. |
| `/diagnostics_agg` | [diagnostic\_msgs/msg/DiagnosticArray](https://github.com/ros2/common_interfaces/blob/humble/diagnostic_msgs/msg/DiagnosticArray.msg) | System diagnostics. |
| `/tf` | [tf2\_msgs/msg/TFMessage](https://github.com/ros2/geometry2/blob/humble/tf2_msgs/msg/TFMessage.msg) | Movable transforms on the robot. |
| `/tf_static` | [tf2\_msgs/msg/TFMessage](https://github.com/ros2/geometry2/blob/humble/tf2_msgs/msg/TFMessage.msg) | Fixed transforms on the robot. |
| `/robot_description` | [std\_msgs/msg/String](https://github.com/ros2/common_interfaces/blob/humble/std_msgs/msg/String.msg) | The description of the robot URDF as a string. |
| `/front_stereo_camera/left/image_compressed` | [sensor\_msgs/msg/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg) | Front stereo camera left camera stream. |
| `/front_stereo_camera/left/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Front stereo camera left camera intrinsics. |
| `/front_stereo_camera/right/image_compressed` | [sensor\_msgs/msg/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg) | Front stereo camera right camera stream. |
| `/front_stereo_camera/right/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Front stereo camera right camera intrinsics. |
| `/back_stereo_camera/left/image_compressed` | [sensor\_msgs/msg/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg) | Back stereo camera left camera stream. |
| `/back_stereo_camera/left/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Back stereo camera left camera intrinsics. |
| `/back_stereo_camera/right/image_compressed` | [sensor\_msgs/msg/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg) | Back stereo camera right camera stream. |
| `/back_stereo_camera/right/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Back stereo camera right camera intrinsics. |
| `/left_stereo_camera/left/image_compressed` | [sensor\_msgs/msg/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg) | Left stereo camera left camera stream. |
| `/left_stereo_camera/left/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Left stereo camera left camera intrinsics. |
| `/left_stereo_camera/right/image_compressed` | [sensor\_msgs/msg/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg) | Left stereo camera right camera stream. |
| `/left_stereo_camera/right/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Left stereo camera right camera intrinsics. |
| `/right_stereo_camera/left/image_compressed` | [sensor\_msgs/msg/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg) | Right stereo camera left camera stream. |
| `/right_stereo_camera/left/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Right stereo camera left camera intrinsics. |
| `/right_stereo_camera/right/image_compressed` | [sensor\_msgs/msg/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg) | Right stereo camera right camera stream. |
| `/right_stereo_camera/right/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Right stereo camera right camera intrinsics. |
| `/front_fisheye_camera/left/image_compressed` | [sensor\_msgs/msg/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg) | Front fisheye camera stream. |
| `/front_fisheye_camera/left/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Front fisheye camera intrinsics. |
| `/back_fisheye_camera/left/image_compressed` | [sensor\_msgs/msg/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg) | Back fisheye camera stream. |
| `/back_fisheye_camera/left/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Back fisheye camera intrinsics. |
| `/left_fisheye_camera/left/image_compressed` | [sensor\_msgs/msg/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg) | Left fisheye camera stream. |
| `/left_fisheye_camera/left/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Left fisheye camera intrinsics. |
| `/right_fisheye_camera/left/image_compressed` | [sensor\_msgs/msg/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg) | Right fisheye camera stream. |
| `/right_fisheye_camera/left/camera_info` | [sensor\_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg) | Right fisheye camera intrinsics. |
| `/front_2d_lidar/scan` | [sensor\_msgs/msg/LaserScan](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/LaserScan.msg) | Front 2D lidar scan. |
| `/back_2d_lidar/scan` | [sensor\_msgs/msg/LaserScan](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/LaserScan.msg) | Back 2D lidar scan. |
| `/front_3d_lidar/lidar_packets` | [hesai\_ros\_driver/msg/UdpFrame](https://github.com/HesaiTechnology/HesaiLidar_ROS_2.0/blob/master/msg/msg_ros2/UdpFrame.msg) | Front 3D lidar UDP packets. |
| `/front_stereo_imu/imu` | [sensor\_msgs/msg/Imu](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Imu.msg) | Front stereo camera inertial measurement unit. |
| `/chassis/imu` | [sensor\_msgs/msg/Imu](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Imu.msg) | Chassis inertial measurement unit. |
| `/chassis/ticks` | [isaac\_ros\_nova\_interfaces/msg/EncoderTicks](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common/blob/main/isaac_ros_nova_interfaces/msg/EncoderTicks.msg) | Chassis encoder count. |
| `/chassis/odom` | [nav\_msgs/msg/Odometry](https://github.com/ros2/common_interfaces/blob/humble/nav_msgs/msg/Odometry.msg) | Chassis odometry. |
| `/chassis/battery_state` | [sensor\_msgs/msg/BatteryState](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/BatteryState.msg) | Chassis battery state. |

#### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html\#ros-topics-published "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `directory_info` | [isaac\_ros\_nova\_recorder/DirectoryInfo](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova) | Directory information. |

#### ROS Services Advertised [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html\#ros-services-advertised "Link to this heading")

| ROS Service | Interface | Description |
| --- | --- | --- |
| `delete_rosbag` | [isaac\_ros\_nova\_recorder/DeleteRosbag](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova) | Deletes a rosbag from disk. |

#### ROS Actions Advertised [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html\#ros-actions-advertised "Link to this heading")

| ROS Action | Interface | Description |
| --- | --- | --- |
| `upload_rosbag` | [isaac\_ros\_nova\_recorder/UploadRosbag](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova) | Uploads a rosbag to S3. |

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/index.html)