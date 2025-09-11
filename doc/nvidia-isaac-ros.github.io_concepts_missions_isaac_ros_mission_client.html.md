- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Mission Client](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mission_client/index.html)
- [`isaac_ros_mission_client`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mission_client/isaac_ros_mission_client/index.html)
- Isaac ROS Mission Client Tutorial
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/missions/isaac_ros_mission_client.rst.txt)

* * *

# Isaac ROS Mission Client Tutorial [](https://nvidia-isaac-ros.github.io/concepts/missions/isaac_ros_mission_client.html\#isaac-ros-mission-client-tutorial "Link to this heading")

## Docker Setup [](https://nvidia-isaac-ros.github.io/concepts/missions/isaac_ros_mission_client.html\#docker-setup "Link to this heading")

To streamline your development environment setup with the correct versions of dependencies on Jetson and x86\_64 platforms, it is recommended that you leverage the Isaac ROS Dev Docker images. To do this, follow [the dev environment setup steps](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html).

**Note**: All Isaac ROS quick start guides, tutorials, and examples have been designed with the Isaac ROS Docker images as a prerequisite.

## Tutorial with Isaac Sim [](https://nvidia-isaac-ros.github.io/concepts/missions/isaac_ros_mission_client.html\#tutorial-with-isaac-sim "Link to this heading")

[![Tutorial Visual](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/tutorial.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/tutorial.png/)

This tutorial walks you through steps to send missions from [Mission Dispatch](https://github.com/nvidia-isaac/isaac_mission_dispatch) to Isaac ROS Mission Client using the [VDA5050 protocol](https://github.com/VDA5050/VDA5050/blob/main/VDA5050_EN.md). The Mission Client is attached to an Isaac Sim environment running the [Nav2 stack](https://nav2.org/). The summary of steps are as follows:

1. Set up Isaac Sim

2. Run Mission Client

3. Run Mission Dispatch

4. Send a position setup to Nav2 using Mission Dispatch and Mission Client


### Tutorial Walkthrough [](https://nvidia-isaac-ros.github.io/concepts/missions/isaac_ros_mission_client.html\#tutorial-walkthrough "Link to this heading")

1. Install and launch Isaac Sim following the steps in the [Isaac ROS Isaac Sim Setup Guide](https://nvidia-isaac-ros.github.io/getting_started/isaac_sim/index.html).

2. Select `/World/Nova_Carter_ROS/ros_lidars/publish_front_2d_lidar_scan` in the _Stage_ pane and change the topic name for front 2D lidar as shown below.
[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/localization/lidar/isaac_sim_lidar_topic.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/localization/lidar/isaac_sim_lidar_topic.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/localization/lidar/isaac_sim_lidar_topic.png/)
3. Set render products:


- Select `/World/Nova_Carter_ROS/ros_lidars/front_2d_lidar_render_product` in the _Stage_ pane and set `enabled` to True (checked).

- Select `/World/Nova_Carter_ROS/ros_lidars/front_3d_lidar_render_product` in the _Stage_ pane and set `enabled` to False (unchecked).

- Select `/World/Nova_Carter_ROS/front_hawk/left_camera_render_product` in the _Stage_ pane and set `enabled` to False (unchecked).

- Select `/World/Nova_Carter_ROS/front_hawk/right_camera_render_product` in the _Stage_ pane and set `enabled` to False (unchecked).


4. Press **Play** to start publishing data from the Isaac Sim.
[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/isaac_sim_sample_scene.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/isaac_sim_sample_scene.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/isaac_sim_sample_scene.png/)
5. Complete [Set Up Development Environment](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mission_client/isaac_ros_mission_client/index.html#mission-client-set-up-env)
and [Build isaac\_ros\_mission\_client](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mission_client/isaac_ros_mission_client/index.html#mission-client-build) to build the packages.

6. Start MQTT broker.


> The MQTT broker is used for communication between the Mission Dispatch and the robots. There are many ways to run an MQTT broker, including as a system daemon, a standalone application, or a Docker container. The following example uses mosquitto as the MQTT broker.
>
> Create a file `~/mosquitto.sh` with the following contents outside the container:
>
> > ```
> > CONFIG_FILE=/mosquitto.conf
> > if [ $# != 2 ] ; then
> >     echo "usage: $0 <tcp_port> <websocket_port>"
> >     exit 1
> > fi
> > PORT=$1
> > PORT_WEBSOCKET=$2
> > echo "allow_anonymous true" >> $CONFIG_FILE
> > echo "listener $PORT 0.0.0.0" >> $CONFIG_FILE
> > echo "listener $PORT_WEBSOCKET" >> $CONFIG_FILE
> > echo "protocol websockets" >> $CONFIG_FILE
> > mosquitto -c $CONFIG_FILE
> >
> > ```
> >
> > Copy to clipboard
>
> Run the command outside the container:
>
> > ```
> > docker run -it --network host -v ~/mosquitto.sh:/mosquitto.sh -d eclipse-mosquitto:latest sh mosquitto.sh 1883 9001
> >
> > ```
> >
> > Copy to clipboard

7. Run the following launch files within the container to spin up `mission_client` and Nav2:


> ```
> ros2 launch isaac_ros_vda5050_nav2_client_bringup isaac_ros_vda5050_nav2_client.launch.py init_pose_x:=-2.0 init_pose_yaw:=3.14159
>
> ```
>
> Copy to clipboard

8. Start [Mission Dispatch](https://github.com/nvidia-isaac/isaac_mission_dispatch#getting-started-with-deployment-recommended) with Docker.


> - On the Postgres database:
>
> Set the following environment variable:
>
>
>
>
>
> ```
> export POSTGRES_PASSWORD=<Any password>
>
> ```
>
> Copy to clipboard
>
>
>
> Start the Postgres database by running the following:
>
>
>
>
>
> ```
> docker run --rm --name postgres \
>     --network host \
>     -p 5432:5432 \
>     -e POSTGRES_USER=postgres \
>     -e POSTGRES_PASSWORD \
>     -e POSTGRES_DB=mission \
>     -d postgres:14.5
>
> ```
>
> Copy to clipboard
>
> - Launch the Mission Database microservice:
>
> Start the API and database server with the official Docker container.
>
>
>
>
>
> ```
> docker run -it --network host nvcr.io/nvidia/isaac/mission-database:3.2.0
>
> ```
>
> Copy to clipboard
>
>
>
> To see what configuration options are, run:
>
>
>
>
>
> ```
> docker run -it --network host nvcr.io/nvidia/isaac/mission-database:3.2.0 --help
>
> ```
>
> Copy to clipboard
>
>
>
> \# For example, if you want to change the port for the user API from the default 5000 to 5002, add –port 5002 configuration option in the command.
>
> - Launch the Mission Dispatch microservice:
>
> Start the Mission Dispatch server with the official Docker container.
>
>
>
>
>
> ```
> docker run -it --network host nvcr.io/nvidia/isaac/mission-dispatch:3.2.0
>
> ```
>
> Copy to clipboard
>
>
>
> \# To see what the configuration options are, add –help option after the command.


Note

Read [this tutorial](https://github.com/nvidia-isaac/isaac_mission_dispatch#getting-started-with-deployment-recommended) for more deployment options for Mission Dispatch.

9. Open http://localhost:5000/docs in a web browser.

10. Use the POST /robots endpoint to create robot objects. See the video below for steps.

11. Get the status of the robots using the GET /robot
endpoint. If the robots are connected, the state should reflect the actual position of the robots.


[![Isaac ROS Mission Client Sample Isaac Sim Output](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/create_and_delete_robot.gif/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/create_and_delete_robot.gif/)

Note

When using the interactive documentation page, the default value for the the robot object
`name` in the `spec` is ‘string’, you must change it from ‘string’ to another name that has
more meaning, like ‘carter01’. Delete the `prefix` entry as shown in the video.

12. Send a mission to the robot using the POST /mission endpoint. See the video below for steps.


[![Isaac ROS Mission Client Sample Isaac Sim Output](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/create_and_query_mission.gif/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/create_and_query_mission.gif/)

Note

By default, the value for the `robot` is ‘string’, so make sure to change it to
the name you used for one of the robot objects you created earlier. For example, if you set the `name` of the robot
object to ‘carter01’, use that to fill in the `robot` field for the mission. Also, delete the `prefix`,
`selector`, `sequence`, and `action` entries as shown in the video.

1. Return to the Isaac Sim screen. Verify that you can see the robot move to the set goal position, as shown below.


[![Isaac ROS Mission Client Sample Isaac Sim Output](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/sim_output.gif/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/sim_output.gif/)

2. To cancel a mission, use the POST /mission/{name}/cancel endpoint. In the request body, ensure the `name` field is correctly set to the mission’s name that you intend to cancel. Subsequently, observe that the robot stops its motion within Isaac Sim, and that the mission’s state shifts to CANCELED on querying with the GET /mission/{name} endpoint. A mission that has been completed cannot be canceled.


### Customize Your Dev Environment [](https://nvidia-isaac-ros.github.io/concepts/missions/isaac_ros_mission_client.html\#customize-your-dev-environment "Link to this heading")

To further customize your development environment, check out [this guide](https://nvidia-isaac-ros.github.io/concepts/docker_devenv/index.html#development-environment).

## Troubleshooting [](https://nvidia-isaac-ros.github.io/concepts/missions/isaac_ros_mission_client.html\#troubleshooting "Link to this heading")

This tutorial is a computationally complex. If you experience robot collision or localization issues with this tutorial in a less capable development environment, please visit
[here](https://docs.omniverse.nvidia.com/isaacsim/latest/ros2_tutorials/tutorial_ros2_multi_navigation.html#troubleshooting) for further troubleshooting.
This may help improve sensor synchronization.

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/missions/isaac_ros_mission_client.html)[latest](https://nvidia-isaac-ros.github.io/concepts/missions/isaac_ros_mission_client.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/concepts/missions/isaac_ros_mission_client.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/missions/isaac_ros_mission_client.html)