- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS NITROS](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/index.html)
- [Isaac ROS NITROS Bridge](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_bridge/index.html)
- Tutorial for DNN Image Segmentation with NITROS Bridge
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/nitros_bridge/unet_with_nitros_bridge.rst.txt)

* * *

# Tutorial for DNN Image Segmentation with NITROS Bridge [](https://nvidia-isaac-ros.github.io/concepts/nitros_bridge/unet_with_nitros_bridge.html\#tutorial-for-dnn-image-segmentation-with-nitros-bridge "Link to this heading")

## Overview [](https://nvidia-isaac-ros.github.io/concepts/nitros_bridge/unet_with_nitros_bridge.html\#overview "Link to this heading")

This tutorial walks you through how to run the `isaac_ros_unet` on ROS 2 Humble, while playing rosbag and getting the results from ROS Noetic through `isaac_ros_nitros_bridge`.
The tutorial is based on the [ROS1 Bridge documentation](https://github.com/ros2/ros1_bridge).

Note

ROS actions are not supported by the [ROS1 Bridge](https://github.com/ros2/ros1_bridge).

## Steps [](https://nvidia-isaac-ros.github.io/concepts/nitros_bridge/unet_with_nitros_bridge.html\#steps "Link to this heading")

01. Complete the [Isaac ROS Nitros Bridge quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_nitros_bridge/index.html#quickstart).

02. Complete the [Isaac ROS Unet quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_unet/index.html#quickstart).

03. Launch the Docker container using the run\_dev.sh script:





    ```
    cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
    ./scripts/run_dev.sh -d ${HOME}/workspaces --docker_arg "--pid=host"

    ```

    Copy to clipboard

04. Launch the image converter launch file of `isaac_ros_nitros_bridge`:





    ```
    cd /workspaces/isaac_ros-dev/ && \
       source install/setup.bash && \
       ros2 launch isaac_ros_nitros_bridge_ros2 isaac_ros_nitros_bridge_image_converter.launch.py sub_image_name:=unet/colored_segmentation_mask pub_image_name:=image

    ```

    Copy to clipboard

05. **Attach a second terminal** to the Docker container:





    ```
    cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
    ./scripts/run_dev.sh -d ${HOME}/workspaces

    ```

    Copy to clipboard

06. Run the following launch files to spin up `isaac_ros_unet`, (you may need to re-download the `unet models` and convert them since now you are in a new container):





    ```
    cd /workspaces/isaac_ros-dev/ && \
       source install/setup.bash && \
       ros2 launch isaac_ros_unet isaac_ros_unet_tensor_rt.launch.py engine_file_path:=${ISAAC_ROS_WS}/isaac_ros_assets/models/peoplesemsegnet/deployable_quantized_vanilla_unet_onnx_v2.0/1/model.plan input_binding_names:=['input_1:0'] output_binding_names:=['argmax_1'] network_output_type:='argmax' input_image_width:=1200 input_image_height:=632

    ```

    Copy to clipboard

07. Open another terminal, launch the **Noetic** Docker container:





    ```
    docker run -it --cap-add=SYS_PTRACE --privileged --network host --pid host --ipc host --runtime nvidia --name nitros_bridge -e FASTRTPS_DEFAULT_PROFILES_FILE=/usr/local/share/middleware_profiles/rtps_udp_profile.xml --rm nitros_bridge:latest nitros_bridge_image_converter.yaml nitros_bridge_image_converter.launch /image /ros1_output_image

    ```

    Copy to clipboard

08. **Attach a second terminal** to the **Noetic** Docker container:





    ```
    docker exec -it nitros_bridge /bin/bash

    ```

    Copy to clipboard

09. Play the rosbag under ROS Noetic:





    ```
    cd /workspaces/isaac_ros_1-dev/ && \
      source install_isolated/setup.bash && \
      rosbag play -l "src/isaac_ros_nitros_bridge/resources/unet_sample_data_ros1.bag"

    ```

    Copy to clipboard

10. **Attach a third terminal** to the **Noetic** Docker container:





    ```
    docker exec -it nitros_bridge /bin/bash

    ```

    Copy to clipboard

11. Use `rostopic` to echo in the container:





    ```
    cd /workspaces/isaac_ros_1-dev && \
     source install_isolated/setup.bash && \
     rostopic echo /ros1_output_image

    ```

    Copy to clipboard

12. Example output use `rostopic hz`:
    [![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nitros_bridge/nitros_bridge_unet.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nitros_bridge/nitros_bridge_unet.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_nitros_bridge/nitros_bridge_unet.png/)

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/nitros_bridge/unet_with_nitros_bridge.html)[latest](https://nvidia-isaac-ros.github.io/concepts/nitros_bridge/unet_with_nitros_bridge.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/concepts/nitros_bridge/unet_with_nitros_bridge.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/nitros_bridge/unet_with_nitros_bridge.html)