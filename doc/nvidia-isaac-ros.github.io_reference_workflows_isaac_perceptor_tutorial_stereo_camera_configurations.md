- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Reference Workflows](https://nvidia-isaac-ros.github.io/reference_workflows/index.html)
- [Isaac Perceptor](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/index.html)
- Tutorial: Stereo Camera Configurations for Isaac Perceptor
- [View page source](https://nvidia-isaac-ros.github.io/_sources/reference_workflows/isaac_perceptor/tutorial_stereo_camera_configurations.rst.txt)

* * *

# Tutorial: Stereo Camera Configurations for Isaac Perceptor [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorial_stereo_camera_configurations.html\#tutorial-stereo-camera-configurations-for-isaac-perceptor "Link to this heading")

Isaac Perceptor supports various stereo camera configurations. They configure
how the different camera images are processed in the Isaac Perceptor algorithms.

## Configuration Description [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorial_stereo_camera_configurations.html\#configuration-description "Link to this heading")

The following table lists all stereo camera configurations and specifies the corresponding downstream pipeline:

| Configuration Name | ESS + Nvblox | cuVSLAM | People |
| --- | --- | --- | --- |
| `front_configuration` | **Front**: Full at 30 Hz | **Front** | ✗ |
| `front_people_configuration` | **Front**: Full at 30 Hz | **Front** | **Front** |
| `front_left_right_configuration` | **Front**: Full at 30 Hz<br>**Left/Right**: Light at 15 Hz | **Front/Left/Right** | ✗ |

Additional information:

- **Front/Left/Right** denote the stereo cameras mounted on either Nova Orin Developer Kit or Nova Carter.

- The terms Full and Light correspond to different versions of the [ESS DNN model](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/isaac/models/dnn_stereo_disparity).

- The Light ESS DNN model is expected to perform worse than the Full ESS DNN model, example including flickering detection of 10cm high pallet at 3 meters away.

- The **People** column refers to the [people reconstruction mode of nvblox](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/technical_details.html#people-reconstruction).


Note

When enabling wheel odometry with the `enable_wheel_odometry` on Nova
Carter, visual SLAM is automatically disabled on all cameras.

## Selecting a Configuration for Isaac Perceptor [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorial_stereo_camera_configurations.html\#selecting-a-configuration-for-isaac-perceptor "Link to this heading")

When running Isaac Perceptor, the stereo camera configuration can be specified using the
stereo\_camera\_configuration argument:

```
ros2 launch <"platform name">_bringup perceptor.launch.py \
    stereo_camera_configuration:=<"configuration name">

```

Copy to clipboard

where “platform name” is either nova\_developer\_kit or nova\_carter, depending on the platform
you are running Isaac Perceptor on:

- [Tutorial: Isaac Perceptor on Nova Orin Developer Kit](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorials_on_devkit/demo_perceptor.html)

- [Tutorial: Isaac Perceptor on Nova Carter](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorials_on_carter/demo_perceptor.html)


To access all available sensor configurations, use the `--show-args` argument, for example:

```
ros2 launch <"platform name">_bringup perceptor.launch.py --show-args

```

Copy to clipboard

The following table lists the stereo camera configuration compatibility for the Isaac Perceptor tutorials on Nova Carter and Nova Orin Developer Kit platforms:

| Configuration Name | Nova Carter | Nova Orin Developer Kit |
| --- | --- | --- |
| `front_configuration` | ✓ | ✓ |
| `front_people_configuration` | ✓ | ✓ |
| `front_left_right_configuration` | ✓ | ✓ |

## Selecting a Configuration for Autonomous Navigation [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorial_stereo_camera_configurations.html\#selecting-a-configuration-for-autonomous-navigation "Link to this heading")

The stereo\_camera\_configuration argument can be used similarly in the navigation tutorials:

```
ros2 launch <"platform name">_bringup navigation.launch.py \
    stereo_camera_configuration:=<"configuration name">

```

Copy to clipboard

Tutorials for autonomous navigation are available for Nova Carter, Nova Orin Developer Kit and Isaac Sim:

- [Tutorial: Autonomous Navigation with Isaac Perceptor and Nav2 on Nova Carter](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorials_on_carter/demo_navigation.html)

- [Tutorial: Integrating Isaac Perceptor with Nav2 on Nova Orin Developer Kit](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorials_on_devkit/demo_navigation.html)

- [Tutorial: Autonomous Navigation in Isaac Sim](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/run_perceptor_in_sim.html)


The following table lists the stereo camera configuration compatibility for the Autonomous Navigation tutorials with Nova Carter, Nova Orin Developer Kit and Isaac Sim:

| Configuration Name | Nova Carter | Nova Orin Developer Kit | Isaac Sim |
| --- | --- | --- | --- |
| `front_configuration` | ✓ | ✓ | ✓ |
| `front_people_configuration` | ✗ | ✓ | ✗ |
| `front_left_right_configuration` | ✓ | ✓ | ✓ |
| `no_camera` (see note below) | ✓ | ✓ | ✗ |

Note

If you want to run Autonomous Navigation with lidar perception only,
you can disable Isaac Perceptor by setting the configuration to `no_camera`.
This will also require that you set enable\_wheel\_odometry:=True. Ensure you have access
to lidar and wheel odometry while running with `no_camera`, and topic names are configured
correctly.

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/reference_workflows/isaac_perceptor/tutorial_stereo_camera_configurations.html)[latest](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorial_stereo_camera_configurations.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/index.html)