- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Reference Workflows](https://nvidia-isaac-ros.github.io/reference_workflows/index.html)
- [Isaac Perceptor](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/index.html)
- Tutorial: Running Isaac Perceptor on Nova Orin Developer Kit
- [View page source](https://nvidia-isaac-ros.github.io/_sources/reference_workflows/isaac_perceptor/run_perceptor_on_devkit.rst.txt)

* * *

# Tutorial: Running Isaac Perceptor on Nova Orin Developer Kit [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/run_perceptor_on_devkit.html\#tutorial-running-isaac-perceptor-on-nova-orin-developer-kit "Link to this heading")

To run and evaluate camera-based perception on your robot with the Nova Orin Developer Kit,
please follow suggested mounting guidelines, and power on the Nova Orin Developer Kit.

Note

The Nova Orin Developer Kit has been unboxed and set up as expected by following
[Getting Started](https://nvidia-isaac-ros.github.io/robots/nova_developer_kit/getting_started.html).

## Mounting the Nova Orin Developer Kit on a Robot [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/run_perceptor_on_devkit.html\#mounting-the-nova-orin-developer-kit-on-a-robot "Link to this heading")

To use the Nova Orin Developer Kit as the perception module on an
existing robot, you must mount it on your robot. It is essential that the
developer kit is mounted rigidly and cannot move.

It is recommended to mount the Nova Orin Developer Kit such that the camera’s field of
views are largely unobstructed. Make sure to orient the Nova Orin Developer Kit such that
the front camera is pointing to the robot’s main direction of travel,
and the bottom plate is parallel to the ground.

The developer kit can be mounted using either a tripod mount or the general
purpose hole pattern on its bottom. Recommended mounting hole positions can be found
in the Nova Orin DevKit Quick Start Guide from Segway.

To account for the mounting location when running Isaac Perceptor, a robot URDF file needs to be
provided. Otherwise the [nova\_developer\_kit.urdf.xacro](https://github.com/NVIDIA-ISAAC-ROS/nova_developer_kit/blob/main/nova_developer_kit_description/urdf/nova_developer_kit.urdf.xacro)
is used, which assumes that the Nova Orin Developer Kit is initialized on the ground plane.

It is **recommended to follow the** [extrinsic calibration instructions](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/amr_extrinsic_calibration.html).
After the output of calibration is
stored in the default location in the robot ( `/etc/nova/calibration/isaac_calibration.urdf`),
Isaac Perceptor will use it without requiring any additional arguments:

```
ros2 launch nova_developer_kit_bringup perceptor.launch.py

```

Copy to clipboard

Alternatively, the [Nominal values and URDF file generation](https://nvidia-isaac-ros.github.io/robots/isaac_perceptor_mounting_guide/sensor_arch_general.html#robot-urdf-with-nova-developer-kit)
section details the process to create a robot nominals URDF file ( `urdf_nominals_file_path`). The
robot nominals URDF file can then be passed with the urdf\_override\_file argument to Isaac Perceptor:

```
ros2 launch nova_developer_kit_bringup perceptor.launch.py \
    urdf_override_file:=<"urdf_nominals_file_path">

```

Copy to clipboard

## Powering On the Nova Orin Developer Kit [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/run_perceptor_on_devkit.html\#powering-on-the-nova-orin-developer-kit "Link to this heading")

Use the power splitter cable to distribute power to the barrel jack ports found on the Jetson AGX Orin and GMSL Camera Board. For the input side of the splitter cable, provide a 12V (90W capable) power source. The male barrel jack input has an outer diameter of 5.5mm and inner diameter of 2.5mm.
Verify that the system turns on when power is applied. Alternatively, power on the Nova Orin Developer Kit by pressing the power button (the left-most button) in the AGX Orin.

## Tutorials [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/run_perceptor_on_devkit.html\#tutorials "Link to this heading")

You are recommended to go through following tutorials to running and evaluating
Isaac Perceptor on your robot equipped with the Nova Orin Developer Kit.

### Visualize Sensors on the Nova Orin Developer Kit [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/run_perceptor_on_devkit.html\#visualize-sensors-on-the-nova-orin-developer-kit "Link to this heading")

Follow the [Tutorial: Run all Sensors on the Nova Orin Developer Kit](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorials_on_devkit/demo_sensors.html) to visualize all the sensors on your Nova Orin Developer Kit.

### Running Isaac Perceptor on a Robot [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/run_perceptor_on_devkit.html\#running-isaac-perceptor-on-a-robot "Link to this heading")

Follow the [Tutorial: Running Camera-based 3D Perception with Isaac Perceptor with the Nova Orin Developer Kit](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorials_on_devkit/demo_perceptor.html)
on how to launch the application, how to visualize, and evaluate the results.

### Running Isaac Perceptor on a Robot with the Navigation Stack [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/run_perceptor_on_devkit.html\#running-isaac-perceptor-on-a-robot-with-the-navigation-stack "Link to this heading")

To support running Isaac Perceptor with navigation stacks on your robot,
we provide reference design examples for
sensor mounting [Isaac Perceptor Sensor Mounting Guide](https://nvidia-isaac-ros.github.io/robots/isaac_perceptor_mounting_guide/index.html),
and for software integration [Tutorial: Integrating Isaac Perceptor with Nav2 with the Nova Orin Developer Kit](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorials_on_devkit/demo_navigation.html).

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/reference_workflows/isaac_perceptor/run_perceptor_on_devkit.html)[latest](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/run_perceptor_on_devkit.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/reference_workflows/isaac_perceptor/run_perceptor_on_devkit.html)