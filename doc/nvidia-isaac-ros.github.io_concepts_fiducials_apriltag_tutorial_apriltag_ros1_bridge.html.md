- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS AprilTag](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/index.html)
- [`isaac_ros_apriltag`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html)
- Isaac ROS AprilTag `ros1_bridge` Tutorial
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/fiducials/apriltag/tutorial_apriltag_ros1_bridge.rst.txt)

* * *

# Isaac ROS AprilTag `ros1_bridge` Tutorial [](https://nvidia-isaac-ros.github.io/concepts/fiducials/apriltag/tutorial_apriltag_ros1_bridge.html\#isaac-ros-apriltag-ros1-bridge-tutorial "Link to this heading")

## Overview [](https://nvidia-isaac-ros.github.io/concepts/fiducials/apriltag/tutorial_apriltag_ros1_bridge.html\#overview "Link to this heading")

This tutorial walks you through a graph to estimate the 6DOF pose of AprilTags
using [isaac\_ros\_apriltag](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_apriltag) running ROS 2 and a ROS 1 rosbag containing images.
The image data will be published from the ROS 1 bag and sent to ROS 2
for computation and the tag detections result will be visualized in ROS 1 using the command line
`rostopic echo` tool.

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/fiducials/apriltag/ros1-bridge-flow-chart.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/fiducials/apriltag/ros1-bridge-flow-chart.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/fiducials/apriltag/ros1-bridge-flow-chart.png/)

## Tutorial Walkthrough [](https://nvidia-isaac-ros.github.io/concepts/fiducials/apriltag/tutorial_apriltag_ros1_bridge.html\#tutorial-walkthrough "Link to this heading")

01. Complete the quickstart [here](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_apriltag/isaac_ros_apriltag/index.html#quickstart).

02. Complete the [Isaac ROS ros1\_bridge Setup Guide](https://nvidia-isaac-ros.github.io/concepts/nitros_bridge/setup_ros1_docker.html).

03. Start the `Noetic` container:





    ```
    docker run -it --cap-add=SYS_PTRACE --privileged --network host --pid host --runtime nvidia -e FASTRTPS_DEFAULT_PROFILES_FILE=/usr/local/share/middleware_profiles/rtps_udp_profile.xml --entrypoint /usr/local/bin/scripts/workspace-entrypoint.sh --name nitros_bridge --rm nitros_bridge:latest /bin/bash

    ```

    Copy to clipboard

04. Source `ros1_noetic` and run `roscore`:





    ```
    source /opt/ros/noetic/setup.bash && \
     roscore

    ```

    Copy to clipboard

05. Attach the second terminal to the `Noetic` docker container:





    ```
    docker exec -it nitros_bridge /bin/bash

    ```

    Copy to clipboard

06. Clone `isaac_ros_apriltag` into the `Noetic` container:





    ```
    cd /tmp && \
     git clone https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_apriltag.git

    ```

    Copy to clipboard

07. Pull down a ROS 1 rosbag of sample data:





    ```
    cd /tmp/isaac_ros_apriltag/ && \
     git lfs pull -X "" -I "resources/rosbags/ros1_bridge_apriltag.bag"

    ```

    Copy to clipboard

08. Inside the container, build and source the workspace:





    ```
    cd /workspaces/isaac_ros-dev/ && \
     source install/setup.bash && \
     export ROS_MASTER_URI=http://localhost:11311 && \
     ros2 run ros1_bridge dynamic_bridge --bridge-all-topics

    ```

    Copy to clipboard

09. Attach the third terminal to the `Noetic` docker container:





    ```
    docker exec -it nitros_bridge /bin/bash

    ```

    Copy to clipboard

10. Play the AprilTag ROS1 rosbag in a loop:





    ```
    source /opt/ros/noetic/setup.bash && \
     cd /tmp/isaac_ros_apriltag/resources/rosbags && \
     rosbag play -l ros1_bridge_apriltag.bag

    ```

    Copy to clipboard

11. Launch the `Isaac ROS Dev` Docker container using the `run_dev.sh` script:


> ```
> cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
>  ./scripts/run_dev.sh -d ${HOME}/workspaces -a --pid=host
>
> ```
>
> Copy to clipboard

12. Run the `isaac_ros_apriltag` node:





    ```
    cd /workspaces/isaac_ros-dev && \
     source install/setup.bash && \
     ros2 launch isaac_ros_apriltag isaac_ros_apriltag.launch.py

    ```

    Copy to clipboard

13. Attach the forth terminal to the `Noetic` docker container:





    ```
    docker exec -it nitros_bridge /bin/bash

    ```

    Copy to clipboard

14. Use `rostopic echo` to print the tag detections in the container:





    ```
    cd /workspaces/isaac_ros_1-dev && \
     source install_isolated/setup.bash && \
     rostopic echo /tag_detections

    ```

    Copy to clipboard


    [![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/fiducials/apriltag/ros1_tag_echo.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/fiducials/apriltag/ros1_tag_echo.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/fiducials/apriltag/ros1_tag_echo.png/)

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/fiducials/apriltag/tutorial_apriltag_ros1_bridge.html)[latest](https://nvidia-isaac-ros.github.io/concepts/fiducials/apriltag/tutorial_apriltag_ros1_bridge.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/concepts/fiducials/apriltag/tutorial_apriltag_ros1_bridge.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/fiducials/apriltag/tutorial_apriltag_ros1_bridge.html)