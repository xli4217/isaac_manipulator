- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Reference Workflows](https://nvidia-isaac-ros.github.io/reference_workflows/index.html)
- [Isaac Manipulator](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/index.html)
- Tutorial for Pick and Place from Isaac Cloud Service
- [View page source](https://nvidia-isaac-ros.github.io/_sources/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place_from_cloud_service.rst.txt)

* * *

# Tutorial for Pick and Place from Isaac Cloud Service [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place_from_cloud_service.html\#tutorial-for-pick-and-place-from-isaac-cloud-service "Link to this heading")

## Overview [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place_from_cloud_service.html\#overview "Link to this heading")

This tutorial walks through the process of triggering pick and place workflow from Isaac cloud service using Mission Dispatch APIs. In this tutorial, you will setup Mission Dispatch on one x86\_64 machine, launch mission client along with other ROS2 nodes on the robot and trigger pick and place workflow by cloud APIs.

## Prerequisites [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place_from_cloud_service.html\#prerequisites "Link to this heading")

- Complete the [pick and place tutorial](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place.html) for pick and place using ROS2 action servers first.

- If you are using Isaac Sim, complete the following sections in the [pick and place tutorial for Isaac Sim](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_isaac_sim.html):

1. Set Up Development Environment

2. Build the Code

3. Set Up Perception Deep Learning Models
- If you have a RealSense manipulator, complete all sections except “Run Pick and Place with Isaac Sim Platform.”


## Tutorial Walkthrough [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place_from_cloud_service.html\#tutorial-walkthrough "Link to this heading")

### Setup Mission Dispatch [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place_from_cloud_service.html\#setup-mission-dispatch "Link to this heading")

The following steps should be done on an x86\_64 machine.

1. Start MQTT broker on the x86\_64 machine.


> The MQTT broker is used for communication between the Mission Dispatch and the robots. There are many ways to run an MQTT broker, including as a system daemon, a standalone application, or a Docker container. The following example uses mosquitto as the MQTT broker.
>
> 1. Create a file `~/mosquitto.sh` with the following contents outside the container:
>
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
> 2. Run the command outside the container:
>
>
> > ```
> > docker run -it --network host -v ~/mosquitto.sh:/mosquitto.sh -d eclipse-mosquitto:latest sh mosquitto.sh 1883 9001
> >
> > ```
> >
> > Copy to clipboard

2. Start [Mission Dispatch](https://github.com/nvidia-isaac/isaac_mission_dispatch#getting-started-with-deployment-recommended) with Docker.


> On the Postgres DatabaseLaunch the Mission Database Microservice
>
> 1. Set the following environment variable:
>
>
> > ```
> > export POSTGRES_PASSWORD=<Any password>
> >
> > ```
> >
> > Copy to clipboard
>
> 2. Start the Postgres database by running the following:
>
>
>
>
>
> ```
> docker run --rm --name postgres \
>       --network host \
>       -p 5432:5432 \
>       -e POSTGRES_USER=postgres \
>       -e POSTGRES_PASSWORD \
>       -e POSTGRES_DB=mission \
>      -d postgres:14.5
>
> ```
>
> Copy to clipboard
>
>
> Read [this tutorial](https://github.com/nvidia-isaac/isaac_mission_dispatch#getting-started-with-deployment-recommended) for more deployment options for Mission Dispatch.

3. Open `http://localhost:5000/docs` in a web browser.


### Launch ROS2 Nodes [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place_from_cloud_service.html\#launch-ros2-nodes "Link to this heading")

Complete the following steps on the robot.

1. Complete [Set Up Development Environment](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mission_client/isaac_ros_mission_client/index.html#mission-client-set-up-env)
and [Build isaac\_ros\_mission\_client](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mission_client/isaac_ros_mission_client/index.html#mission-client-build) to build the packages.

2. Run the following launch files within the container to spin up `mission_client`:


> ```
> ros2 launch isaac_ros_vda5050_nav2_client_bringup isaac_ros_vda5050_client.launch.py \
>     robot_type:=arm \
>     launch_ws_bridge:=True \
>     serial_number:=arm01 \
>     mqtt_host_name:=<mission_dispatch_ip>
>
> ```
>
> Copy to clipboard

3. Complete the required sections in the [pick and place tutorial](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place.html) or the [pick and place tutorial for Isaac Sim](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_isaac_sim.html) first as described in [Overview](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place_from_cloud_service.html#overview).

4. Launch pick and place servers:


> On RealSense RobotIn Isaac Sim
>
> > 1. Inside the container, launch the tool\_communication.py script:
> >
> >
> > ```
> > ros2 run ur_robot_driver tool_communication.py --ros-args -p robot_ip:=<ROBOT_IP_ADDRESS>
> >
> > ```
> >
> > Copy to clipboard
>
> 1. Open a new terminal inside the container and run:
>
>
> > ```
> > cd ${ISAAC_ROS_WS} && \
> >    source install/setup.bash
> >
> > ```
> >
> > Copy to clipboard
> >
> > HawkRealSense
> >
> > ```
> > ros2 launch isaac_manipulator_pick_and_place ur_pick_and_place.launch.py \
> >     ur_type:=<UR_TYPE> robot_ip:=<ROBOT_IP_ADDRESS> \
> >     gripper_type:=<GRIPPER_TYPE> camera_type:=hawk setup:=<SETUP_NAME> \
> >     ess_engine_file_path:=${ISAAC_ROS_WS}/isaac_ros_assets/models/dnn_stereo_disparity/dnn_stereo_disparity_v4.1.0_onnx/ess.engine
> >
> > ```
> >
> > Copy to clipboard
> >
> > This tutorial was validated using `ur_type:=ur5e`, `ur_type:=ur10e` with gripper types
> > of `robotiq_2f_140` and `robotiq_2f_85`.


### Trigger Pick and Place Workflow [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place_from_cloud_service.html\#trigger-pick-and-place-workflow "Link to this heading")

1. Open `http://localhost:5000/docs` in a web browser.

2. Use the `POST /robot` endpoint to create robot with name `arm01`.


> ```
> {
>     "labels": [],
>     "heartbeat_timeout": 30,
>     "name": "arm01"
> }
>
> ```
>
> Copy to clipboard
>
> When using the interactive documentation page, the default value for the the robot object
> `name` in the `spec` is ‘string’, you must change it from ‘string’ to another name that has
> more meaning, like ‘arm01’. Delete the `prefix` entry as shown in the video.

3. Trigger object detection using `POST /mission` endpoint. Here is an example input:


> ```
> {
>     "robot": "arm01",
>     "mission_tree": [\
>         {\
>             "name": "get_objects",\
>             "parent": "root",\
>             "action": {\
>                 "action_type": "get_objects",\
>                 "action_parameters": {}\
>             }\
>\
>         }\
>     ],
>     "timeout": 300,
>     "needs_canceled": false
> }
>
> ```
>
> Copy to clipboard

4. If the mission completes successfully, use `GET /detection_results` endpoint to get object detection results. The detection results contain `object_id` and `class_id`, which are needed for pick and place workflow.


> If the `get_objects` mission fails or `GET /detection_results` endpoint returns an empty list, repeat step 2 to send the mission again.

5. Trigger pick and place using `POST /mission` endpoint. `object_id` and `class_id` come from the detection results. `place_pose` is a comma separated string with 7 floats, representing the position and quaternion of where the gripper drops the object in the manipulator `base_link` frame. For the example scene in Isaac Sim, set `place_pose` as `"-0.35,0.2,0.4,0.996,0.066,0.042,0.034"` to drop the object into the bin.


> ```
> {
>     "robot": "arm01",
>     "mission_tree": [\
>         {\
>             "name": "pick_place",\
>             "parent": "root",\
>             "action": {\
>                 "action_type": "pick_and_place",\
>                 "action_parameters": {\
>                     "object_id": "<object_id>",\
>                     "class_id": "<class_id>",\
>                     "place_pose": "<position_x>, <position_y>, <position_z>, <Quaternion_x>, <Quaternion_y>, <Quaternion_z>, <Quaternion_w>"\
>                 }\
>             }\
>\
>         }\
>     ],
>     "timeout": 300,
>     "needs_canceled": false
> }
>
> ```
>
> Copy to clipboard
>
> Note
>
> - If the `pick_and_place` mission fails, repeat steps 2-4 to try again.
>
> - The pick and place server will cache object poses. If you rerun the pipeline and the object position is different, clear the cache using the `POST /mission` endpoint. The `object_ids` parameter can be either empty to clear all objects or a comma separated string to clear specific objects.
>
>
> ```
> {
>     "robot": "arm01",
>     "mission_tree": [\
>         {\
>             "name": "clear_objects",\
>             "parent": "root",\
>             "action": {\
>                 "action_type": "clear_objects",\
>                 "action_parameters": {\
>                     "object_ids": ""\
>                 }\
>             }\
>\
>         }\
>     ],
>     "timeout": 300,
>     "needs_canceled": false
> }
>
> ```
>
> Copy to clipboard
>
> The steps above shows how to trigger pick and place workflow using Mission Dispatch APIs. We provide a basic UI as an example showing how you can develop applications using our cloud APIs.
>
> You can find the source code in `mission_dispatch/scripts/pick_and_place_ui.py`.
>
> To start the UI on your x86\_64 machine:
>
> > 1. Clone the code, navigate to the start script, and start the UI:
> >
> >
> > > ```
> > > git clone --recurse-submodules https://github.com/nvidia-isaac/isaac_mission_dispatch.git
> > > cd mission_dispatch && scripts/run_dev.sh
> > >
> > > ```
> > >
> > > Copy to clipboard
> >
> > 2. Run the UI in the Docker container
> >
> >
> > > ```
> > > bazel run //scripts:pick_and_place_ui -- http://localhost:5000 ws://<robot_ip>:9090 /resize/image
> > >
> > > ```
> > >
> > > Copy to clipboard
> > >
> > > If you are using Isaac Sim, `robot_ip` must be `localhost`. If you are running on a RealSense manipulator, it is the IP address of your Jetson machine.

6. Enter the robot name `arm01` and click **Run Object Detection** to trigger object detection. The UI will show an image with bounding boxes for detected objects.

7. Click on the image to select the object to pick and enter the place pose.

8. Click **Submit** to trigger pick and place pipeline.

9. To run clear objects mission, click **Clear Objects**.


Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place_from_cloud_service.html)[latest](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_pick_and_place_from_cloud_service.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/index.html)