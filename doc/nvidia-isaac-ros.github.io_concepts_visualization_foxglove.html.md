- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Concepts](https://nvidia-isaac-ros.github.io/concepts/index.html)
- [Visualization](https://nvidia-isaac-ros.github.io/concepts/visualization/index.html)
- Foxglove Visualization
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/visualization/foxglove.rst.txt)

* * *

# Foxglove Visualization [](https://nvidia-isaac-ros.github.io/concepts/visualization/foxglove.html\#foxglove-visualization "Link to this heading")

[Foxglove](https://foxglove.dev/) is a popular framework to visualize ROS
running applications similar to [Rviz](https://github.com/ros2/rviz). It
provides a web-based visualization client and a desktop app.

Many Isaac ROS tutorials use Foxglove for the visualization.

# Foxglove Setup [](https://nvidia-isaac-ros.github.io/concepts/visualization/foxglove.html\#foxglove-setup "Link to this heading")

Use the following steps to visualize a ROS application in Foxglove.

1. On your visualization computer install
[Foxglove Studio](https://foxglove.dev/download) and launch it:


[![foxglove_studio](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/robots/nova_carter/foxglove_studio.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/robots/nova_carter/foxglove_studio.png/)

2. Click on the Open connection button. If you are running the ROS application
on a different machine than the one that is running Foxglove you’ll have to
adjust the Websocket URL. In that case, replace `localhost` with the IP of
the ROS machine.


[![foxglove_connect](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/robots/nova_carter/foxglove_connect.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/robots/nova_carter/foxglove_connect.png/)

3. You can now add a 3D panel and select which topics to visualize in the left bar.

4. Some tutorials also provide a pre-configured Foxglove layout in the form of a
JSON file. To use those make sure you have the repository containing the
tutorial cloned on your visualization machine. Import the layout file by
clicking on the `Import from file...` in the `layout` drop-down menu and
choosing the layout file from the correct folder.


[![foxglove_import_layout](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/robots/nova_carter/foxglove_import_layout.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/robots/nova_carter/foxglove_import_layout.png/)

# Installing Additional Extensions [](https://nvidia-isaac-ros.github.io/concepts/visualization/foxglove.html\#installing-additional-extensions "Link to this heading")

Some visualizations require additional extensions. You can install those
extensions by clicking on the user icon in the top right corner and then opening
the extensions menu. There you will find a list of all available extensions. An
extension can be installed by clicking on it and pressing the install button.

We provide the following extensions:

- Nvblox Foxglove: Allows to visualize Nvblox meshes in Foxglove.


# Rosbag Visualization [](https://nvidia-isaac-ros.github.io/concepts/visualization/foxglove.html\#rosbag-visualization "Link to this heading")

To visualize a rosbag, images need to be converted from
[sensor\_msgs/msg/CompressedImage](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CompressedImage.msg) to
[foxglove\_msgs/msg/CompressedVideo](https://github.com/foxglove/schemas/blob/main/ros_foxglove_msgs/ros2/CompressedVideo.msg).

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


3. Launch the Docker container using the `run_dev.sh` script:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
./scripts/run_dev.sh

```

Copy to clipboard

4. Install the prebuilt Debian package:


> ```
> sudo apt-get update
>
> ```
>
> Copy to clipboard






```
sudo apt-get install -y ros-humble-isaac-ros-data-replayer

```

Copy to clipboard

5. Convert the rosbag.





```
ros2 run isaac_ros_data_replayer foxglove_converter.py <input> <output>

```

Copy to clipboard

6. Launch Foxglove Studio and click on the **Open local file** button.


[![foxglove_studio](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/robots/nova_carter/foxglove_studio.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/robots/nova_carter/foxglove_studio.png/)

7. Navigate to the converted rosbag and select the MCAP file to visualize it in Foxglove.


[![foxglove_rosbag](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/robots/nova_carter/foxglove_rosbag.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/robots/nova_carter/foxglove_rosbag.png/)

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/visualization/foxglove.html)[latest](https://nvidia-isaac-ros.github.io/concepts/visualization/foxglove.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/visualization/foxglove.html)