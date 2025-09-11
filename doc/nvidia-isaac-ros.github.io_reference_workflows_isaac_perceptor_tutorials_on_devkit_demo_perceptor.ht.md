- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Reference Workflows](https://nvidia-isaac-ros.github.io/reference_workflows/index.html)
- [Isaac Perceptor](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/index.html)
- [Tutorial: Running Isaac Perceptor on Nova Orin Developer Kit](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/run_perceptor_on_devkit.html)
- Tutorial: Running Camera-based 3D Perception with Isaac Perceptor with the Nova Orin Developer Kit
- [View page source](https://nvidia-isaac-ros.github.io/_sources/reference_workflows/isaac_perceptor/tutorials_on_devkit/demo_perceptor.rst.txt)

* * *

# Tutorial: Running Camera-based 3D Perception with Isaac Perceptor with the Nova Orin Developer Kit [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorials_on_devkit/demo_perceptor.html\#tutorial-running-camera-based-3d-perception-with-isaac-perceptor-with-the-nova-orin-developer-kit "Link to this heading")

This tutorial walks you through using Isaac Perceptor on the Nova Orin Developer Kit.
Isaac Perceptor uses Isaac ROS components, including Visual SLAM to localize the robot, ESS and
Nvblox to reconstruct 3D environments.

For this tutorial, you must have successfully completed the
[Tutorial: running all sensors on the Nova Orin Developer Kit](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorials_on_devkit/demo_sensors.html).

## Running the Application [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorials_on_devkit/demo_perceptor.html\#running-the-application "Link to this heading")

1. SSH into the Nova Orin Developer Kit ( [instructions](https://nvidia-isaac-ros.github.io/robots/nova_developer_kit/getting_started.html#nova-developer-kit-ssh-setup)).

2. Build/install the required packages and run the app:


> Docker ImageBinary PackageBuild from Source
>
> 1. Pull the Docker image:
>
>
> ```
> docker pull nvcr.io/nvidia/isaac/nova_developer_kit_bringup:release_3.2-aarch64
>
> ```
>
> Copy to clipboard
>
> 2. Run the Docker image:
>
>
> ```
> docker run --privileged --network host \
>     -v /dev/*:/dev/* \
>     -v /tmp/argus_socket:/tmp/argus_socket \
>     -v /etc/nova:/etc/nova \
>     nvcr.io/nvidia/isaac/nova_developer_kit_bringup:release_3.2-aarch64 \
>     ros2 launch nova_developer_kit_bringup perceptor.launch.py
>
> ```
>
> Copy to clipboard

Note

Launch the application following [the mounting guide](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/run_perceptor_on_devkit.html)
for correct mounting procedures and configuration changes.

## Customizing Sensor Configurations [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorials_on_devkit/demo_perceptor.html\#customizing-sensor-configurations "Link to this heading")

By default, as specified in the launch file `perceptor.launch.py`, all 3 stereo
cameras available on the Nova Orin Developer Kit (front, left, right) are used in
Isaac Perceptor algorithms.
You may use the `stereo_camera_configuration` launch argument to customize camera
configurations when running Isaac Perceptor.

For example, to use only front stereo camera for 3D reconstruction and visual SLAM
you could run the following launch command:

```
ros2 launch nova_developer_kit_bringup perceptor.launch.py \
    stereo_camera_configuration:=front_configuration

```

Copy to clipboard

For example, to use only front stereo camera for 3D reconstruction, visual SLAM
and people reconstruction, you could run the following launch command:

```
ros2 launch nova_developer_kit_bringup perceptor.launch.py \
    stereo_camera_configuration:=front_people_configuration

```

Copy to clipboard

For a detailed description of all available configurations refer to
[Tutorial: Stereo Camera Configurations for Isaac Perceptor](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorial_stereo_camera_configurations.html).

## Mapping and Localization [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorials_on_devkit/demo_perceptor.html\#mapping-and-localization "Link to this heading")

In order to create maps and localize, refer to [Tutorial: Mapping and Localization with Isaac Perceptor](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorial_mapping_and_localization.html).

## Visualizing the Outputs [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorials_on_devkit/demo_perceptor.html\#visualizing-the-outputs "Link to this heading")

1. Make sure you complete [Visualization Setup](https://nvidia-isaac-ros.github.io/robots/nova_developer_kit/getting_started.html#visualization-setup-devkit).
This is required to visualize the Isaac nvblox mesh in a recommended layout configuration.

2. Open the Foxglove studio on your remote machine.

1. If you are **not** running a configuration with people reconstruction,
      open the `nova_developer_kit_perceptor.json` layout file downloaded in the previous step.

2. If you are running a configuration with people reconstruction,
      open the `nova_developer_kit_perceptor_with_people.json` layout file
      downloaded in the previous step.
3. In Foxglove, it shows a visualization of the Nova Orin Developer Kit platform and
Isaac nvblox mesh visualization of surrounding environments.
Verify that you see a visualization similar to the image below.
In the mesh, it shows the reconstructed colored voxels,
the computed distance map from Isaac nvblox outputs.
The colored voxels are uniformly reconstructed with a resolution of 5cm.
The rainbow color spectrum reflects the proximity of each region
to nearest obstacles. Regions closer to obstacle surfaces are marked in warmer colors
(red, orange), while regions further away from obstacle surfaces are marked in cooler
colors (blue, violet).


[![Foxglove visualization of the `Isaac Perceptor` outputs](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/robots/nova_developer_kit/foxglove_perceptor.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/robots/nova_developer_kit/foxglove_perceptor.png/)

4. If running a configuration with people reconstruction, in addition,
you can see highlighted red voxels shown in the reconstructed mesh.
It visualizes people perceived in the field of view of one or more cameras. In the default
`nova_developer_kit_perceptor_with_people.json` layout file,
only `/nvblox_node/dynamic_occupancy_layer` is visualized and shown as the red voxels.


[![Foxglove visualization of the `Isaac Perceptor` people mapping outputs](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_perceptor/perceptor_foxglove_people_mapping.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_perceptor/perceptor_foxglove_people_mapping.png/)

Note

For a better visualization experience, some topics requiring a large bandwidth are not available
to Foxglove studio. You can set `use_foxglove_whitelist:=False` as additional argument
when running the app. Most likely the image stream will be fairly choppy given its large
bandwidth. To learn more about topics published by Isaac nvblox, see
[nvblox ROS messages](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox/api/topics_and_services.html).
For topics published by Isaac visual SLAM, see
[cuvslam ROS messages](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_visual_slam/isaac_ros_visual_slam/index.html).

## Evaluating Isaac Perceptor [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorials_on_devkit/demo_perceptor.html\#evaluating-isaac-perceptor "Link to this heading")

Follow these instructions to assert that Isaac Perceptor is performing as
expected.

1. Isaac nvblox continuously measures computation efficiency metrics and will report
them in the terminal on request. To turn on rate output change the following
configurations from `false` to `true` in the file
`$ISAAC_ROS_WS/src/isaac_perceptor/isaac_ros_perceptor_bringup/params/nvblox_perceptor.yaml`:





```
print_rates_to_console: true
print_delays_to_console: true

```

Copy to clipboard



Verify that you see metrics similar to the following:


1. `nvblox Rates` reports how the frequencies of specific events happening
      in nvblox. For the **3-camera** configuration, you are expected to see
      `ros/depth_image_callback` at 60Hz, and `ros/color_image_callback`
      at 90Hz. For the **1-camera** configuration, you are expected to see
      `ros/depth_image_callback` at 30Hz, and `ros/color_image_callback`
      at 30Hz.


> ```
> nvblox Rates (in Hz)
> namespace/tag - NumSamples (Window Length) - Mean
> -----------
> ros/color                   100              13.2
> ros/depth                   100              60.4
> ros/depth_image_callback    100              60.8
> ros/color_image_callback    100              90.8
> ros/update_esdf             100              7.2
> mapper/stream_mesh          100              4.5
> ros/tick                    100              16.7
> -----------
>
> ```
>
> Copy to clipboard

2. `nvblox Delays` reports the average compute time a node takes in second
      based on a certain number of samples. You are expected to see
      `ros/esdf_integration` at 100 ~ 200 ms.


> ```
> nvblox Delays
> namespace/tag - NumSamples (Window Length) - Mean Delay (seconds)
> -----------
> ros/esdf_integration        100              0.174
> ros/depth_image_integration 100              0.152
> ros/color_image_integration 100              0.129
> ros/depth_image_callback    100              0.106
> ros/color_image_callback    100              0.068
> -----------
>
> ```
>
> Copy to clipboard

2. To further evaluate the quality of Isaac Perceptor, you can perform the following tests.


> 1. Choose objects above 10cm with various heights, and place them in front of
> any camera specified in the `stereo_camera_configuration` launch argument.
> Vary the distance to the camera (e.g. 1m, 3m, 5m, 7m). Verify that you see the
> object in the distance map visualization.
> Additionally, you may also use the Measure Tool in the top panel to measure
> the distance between the object surface and the camera center.
>
> For instance, a pallet at the height of 10cm and a box at the height of 50cm are placed
> in front of the front stereo camera. We vary its distance to the robot
> at 0.4m, 1.0m, 3.0m, 5.0m. In the 3D view, you could see colored voxels visualizing
> the pallet and the box (illustrated by bounding boxes in green and blue). The proximity value
> measured by Measure Tool shows the distance between the front camera and perceived objects.
> You are expected to obtain similar proximity value with the distance you place objects at.
>
>
> > ![Evaluating `Isaac Perceptor` voxels at 0.4m.](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/robots/nova_developer_kit/perceptor_devkit_eval_0.4m.png/)![Evaluating `Isaac Perceptor` voxels at 1m.](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/robots/nova_developer_kit/perceptor_devkit_eval_1m.png/)![Evaluating `Isaac Perceptor` voxels at 3m.](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/robots/nova_developer_kit/perceptor_devkit_eval_3m.png/)![Evaluating `Isaac Perceptor` voxels at 5m.](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/robots/nova_developer_kit/perceptor_devkit_eval_5m.png/)
>
>
> You are advised to follow [Technical Details](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/technical_details.html)
> to understand how scene reconstruction works.
>
>
>
> Note
>
>
>
> Objects detection range shown in the reconstructed 3D mesh is sensitive to the
> sensor calibration accuracy.
> Ensure you follow [the mounting guide](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/run_perceptor_on_devkit.html)
> for correct mounting procedures, and configuration changes.
>
> 2. Ensure Nova Orin Developer Kit is stationary. You are expected to see no drift
> from the Nova Orin Developer Kit visualization with reference to the odometry frame.
> You are advised to follow [cuVSLAM](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/index.html)
> to understand how visual SLAM works.


After you complete this tutorial, return to
[Camera-based Perception with Isaac Perceptor on Nova Orin Developer Kit](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/run_perceptor_on_devkit.html)
to proceed with other tutorials.

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/reference_workflows/isaac_perceptor/tutorials_on_devkit/demo_perceptor.html)[latest](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorials_on_devkit/demo_perceptor.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/reference_workflows/isaac_perceptor/tutorials_on_devkit/demo_perceptor.html)