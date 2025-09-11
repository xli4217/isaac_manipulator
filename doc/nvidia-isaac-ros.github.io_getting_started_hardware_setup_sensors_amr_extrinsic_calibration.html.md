- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Getting Started](https://nvidia-isaac-ros.github.io/getting_started/index.html)
- [Hardware Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/index.html)
- [Sensors Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/index.html)
- Extrinsic Calibration for Mobile Robots
- [View page source](https://nvidia-isaac-ros.github.io/_sources/getting_started/hardware_setup/sensors/amr_extrinsic_calibration.rst.txt)

* * *

# Extrinsic Calibration for Mobile Robots [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/amr_extrinsic_calibration.html\#extrinsic-calibration-for-mobile-robots "Link to this heading")

You can use these instructions to calibrate the positions and orientations of sensors on a
mobile robot. This process is known as **Extrinsic Calibration** and it enables robotics functions
that combine information from multiple sensor modalities.

The extrinsic calibration results are encoded in a [URDF](https://wiki.ros.org/urdf)
calibration file, which contains the relative position of different sensors on the robot.

## Instructions [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/amr_extrinsic_calibration.html\#instructions "Link to this heading")

### Extrinsic Calibration of Sensors in Custom Locations [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/amr_extrinsic_calibration.html\#extrinsic-calibration-of-sensors-in-custom-locations "Link to this heading")

We suggest evaluating existing calibration solutions from companies such as
[Main Street Autonomy (MSA)](https://mainstreetautonomy.com/)
or [Tangram Vision](https://www.tangramvision.com/) for your particular needs.

Note

Make sure that the calibration solution supports the [Extrinsic Calibration Requirements](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/amr_extrinsic_calibration.html#extrinsic-calibration-requirements)
and that the resulting URDF calibration file follows the
[Isaac Requirements for URDF files](https://nvidia-isaac-ros.github.io/robots/isaac_urdf_requirements/index.html).

The usual process for most calibration solutions include:

1. Define a nominals URDF file and publish its transformations to the `/tf_static` topic, so that they can be recorded in the rosbag in the next step.

2. Recording a rosbag with your robot. This step may include driving the robot in a particular motion or placing a calibration target around the robot.

3. Processing the rosbag with a calibration tool or sharing it with the calibration provider. The calibration tool or service will generate a URDF calibration file.

4. If the chosen calibration solution does not not support camera - ground calibration, you can complement the extrinsics calibration URDF with the output of the [isaac\_ros\_ground\_calibration](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_ground_calibration/index.html) tool.

5. Placement of the URDF calibration file in the robot.


See the sections below for additional details.

**URDF nominals file:**

This step may be optional depending on the calibration solution and sensors that you are
calibrating. For example, camera-camera calibration with overlapping Field of View (FoV) may not
require nominals. On the other hand, IMU, 2D LIDAR, or robot odometry may not be fully observable
or reliable during calibration. In those cases, nominals allow to constrain the calibrated URDF
file to the best known measurements from a CAD model. Make sure to follow the requirements and
understand the limitations of the chosen calibration solution.

Unless nominals are provided for the robot that you are calibrating (see the [Using Nominals](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/amr_extrinsic_calibration.html#using-nominals)
section), in the general case it is **strongly** recommended that you define a nominals URDF file
for your robot and publish its transformations to the `/tf_static` topic, so that they can be
recorded in the rosbag that will be used for calibration.

The supported process to achieve this goal when using _Nova Sensors_ is:

1. Define the URDF nominals file following the [Isaac Requirements for URDF files](https://nvidia-isaac-ros.github.io/robots/isaac_urdf_requirements/index.html).

2. Place the URDF nominals file in the following path in the robot: `/etc/nova/calibration/isaac_nominals.urdf` (named as `isaac_nominals.urdf`).

3. Make sure to have a `calibration:` line in the robot configuration YAML file that will be used for recording in the next step (see [robot configuration YAML](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova/blob/main/isaac_ros_nova/config/) examples).

4. Use the [isaac\_ros\_nova\_recorder](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html) to record the calibration rosbag. If it finds a nominals URDF in the specified location, it will load it with a [robot\_state\_publisher](https://github.com/ros/robot_state_publisher) and record its contents in the `/tf` and `/tf_static` topics.


**Rosbag recordings used for calibration:**

Most calibration solutions expect a rosbag as input data. When calibrating _Nova Sensors_, it is
**strongly** recommended that you follow the two-step process below:

1. Record the calibration rosbag with the [isaac\_ros\_nova\_recorder](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_nova_recorder/index.html). It allows for efficient recording of multiple data streams by using H.264 compression.

2. Use the [rosbag\_converter](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nova/isaac_ros_data_replayer/index.html) tool to uncompress the messages in the rosbag to the types expected by most calibration solutions:


> - Camera streams: Uncompressed [sensor\_msgs/msg/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg) messages.
>
> - 3D LIDAR data: [sensor\_msgs/msg/PointCloud2](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/PointCloud2.msg) messages (with fields `x, y, z, intensity, ring, timestamp`).


**URDF calibration file:**

To use a URDF calibration file with Isaac applications, **it needs to be placed in**
**the following path in the robot:** `/etc/nova/calibration/isaac_calibration.urdf` (named
as `isaac_calibration.urdf`). This is the default URDF path used by the demo applications and
tutorials.

Note

Make sure that the URDF calibration file follows the
[Isaac Requirements for URDF files](https://nvidia-isaac-ros.github.io/robots/isaac_urdf_requirements/index.html).

### Extrinsic Calibration for Nova Carter or Nova Orin Developer Kit [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/amr_extrinsic_calibration.html\#extrinsic-calibration-for-nova-carter-or-nova-orin-developer-kit "Link to this heading")

Note

The Nova Calibration Tool is deprecated. See the section
[Extrinsic Calibration of Sensors in Custom Locations](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/amr_extrinsic_calibration.html#extrinsic-calibration-of-sensors-in-custom-locations).

The Nova Calibration Tool is a containerized application that guides you
through the process of calibrating the poses of the sensors on a
[Nova Carter](https://robotics.segway.com/nova-carter/) robot or a
[Nova Orin Developer Kit](https://robotics.segway.com/nova-dev-kit/).

1. SSH into the robot ( [instructions](https://nvidia-isaac-ros.github.io/robots/nova_carter/getting_started.html#nova-carter-ssh-setup)).

2. Follow the instructions to
[set up calibration](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/isaac/containers/nova_extrinsics_sensor_calibration_tool).

3. If the calibration is successful, you should see a URDF file generated at
`/etc/nova/calibration/isaac_calibration.urdf`.

This is also the default URDF path used by the demo applications and tutorials.


## Extrinsic Calibration Requirements [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/amr_extrinsic_calibration.html\#extrinsic-calibration-requirements "Link to this heading")

Note

Recommended prior reading:
[Isaac Requirements for URDF files](https://nvidia-isaac-ros.github.io/robots/isaac_urdf_requirements/index.html)

In order to be compatible with Isaac applications, extrinsic calibration tools must:

- Generate a URDF calibration file complying with the [Isaac Requirements for URDF files](https://nvidia-isaac-ros.github.io/robots/isaac_urdf_requirements/index.html).

- Perform extrinsic calibration for all sensors **without** optimizing intrinsic calibration or stereo calibration of any of the sensors (that is, by fixing the calibration from the sensor drivers).


> - In the event that it is necessary to re-calibrate intrinsics or stereo calibration, make sure that all the following are true:
>
>
> > - The re-calibrated sensors have a native a mechanism to overwrite their intrinsic calibration and/or stereo calibration (e.g., the calibration stored in the EEPROM).
> >
> > - The overwrite process is performed before using the extrinsic URDF calibration file with Isaac applications.
> >
> > - The resulting URDF still complies with the [Isaac Requirements for URDF files](https://nvidia-isaac-ros.github.io/robots/isaac_urdf_requirements/index.html) (stereo calibration is not included in the robot URDF).
>
> - Calibration transformations for all sensors must include 6 Degrees of Freedom (DoF), unless certain DoF are not observable during the data recording (see following points).

- Perform 6-DoF sensor-to-robot calibration and encode the result in the transformation to `base_link`, unless certain DoF are not observable during the data recording (see following points).

- Fix nominal transformations (from `/tf_static`) for the DoF that are not observable (or unreliable) during the data recording.


> - Unobservable DoF depend on the calibration process, robot motion (if any), and calibration target design and motion (if any), among others.
>
> - Some specific examples for wheeled robots include :
>
>
> > - _IMU_: Axes that are not excited during data recording.
> >
> > - `base_link`: X and Y-translation and yaw rotation for robots with unreliable odometry.

- Perform sensor-to-ground calibration. See clarifications below:


> - Ground calibration refers to the 3-DoF transformation between a sensor and the ground plane: Z-translation, and pitch and yaw rotations.
>
> - The ground plane should be computed from sensor data.
>
> - The transformation(s) between `base_link` and the robot sensors must be modified so that `base_link` is on the ground, with its X-Y axes representing the ground plane.
>
> - Ground calibration must not modify the transformation between individual sensors. It must only modify the transformation between all sensors (as a rigid body) and `base_link`, such that ground points perceived by sensors lie on the X-Y plane of the `base_link` frame.


## Using Nominals [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/amr_extrinsic_calibration.html\#using-nominals "Link to this heading")

It is recommended that you always use the `isaac_calibration.urdf` file generated in the calibration
instructions above, because it accurately reflects the true extrinsics of your physical robot.
However, as a backup option for Nova Carter and Nova Orin Developer Kit, nominal
[xacro](https://index.ros.org/p/xacro/) files are also available:

> - Nova Carter: [nova\_carter.urdf.xacro](https://github.com/NVIDIA-ISAAC-ROS/nova_carter/blob/main/nova_carter_description/urdf/nova_carter.urdf.xacro).
>
> - Nova Orin Developer Kit: [nova\_developer\_kit\_macro.urdf.xacro](https://github.com/NVIDIA-ISAAC-ROS/nova_developer_kit/blob/main/nova_developer_kit_description/urdf/nova_developer_kit_macro.urdf.xacro).

By default, these nominal values are used by Isaac applications, if a calibrated URDF file is not
present in the system.

Note

If the Nova Orin Developer Kit is mounted on a robot and you intend to use nominal values,
it is still necessary to provide a [URDF encoding its position on the robot](https://nvidia-isaac-ros.github.io/robots/isaac_perceptor_mounting_guide/sensor_arch_general.html#robot-urdf-with-nova-developer-kit).

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/getting_started/hardware_setup/sensors/amr_extrinsic_calibration.html)[latest](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/amr_extrinsic_calibration.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/index.html)