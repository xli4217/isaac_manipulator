- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Getting Started](https://nvidia-isaac-ros.github.io/getting_started/index.html)
- [Hardware Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/index.html)
- [Sensors Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/index.html)
- Camera Calibration for Manipulator
- [View page source](https://nvidia-isaac-ros.github.io/_sources/getting_started/hardware_setup/sensors/manipulator_camera_calibration.rst.txt)

* * *

# Camera Calibration for Manipulator [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/manipulator_camera_calibration.html\#camera-calibration-for-manipulator "Link to this heading")

Note

These instructions are for extrinsics calibration of a Hawk or RealSense camera with a UR e-Series robot (tested with UR5e and UR10e).
For dual-camera setups, please follow the [Calibration Procedure](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/manipulator_camera_calibration.html#calibration-procedure) twice (once for each camera),
with only one camera plugged into the system each time.

## Calibration Target [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/manipulator_camera_calibration.html\#calibration-target "Link to this heading")

### Supported Patterns [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/manipulator_camera_calibration.html\#supported-patterns "Link to this heading")

Both ArUco and ChArUco calibration targets are supported with this tutorial. For best results, we recommend a ChArUco
pattern with an odd number of rows and a high number of markers, printed on the largest sized board that is suitable for the robot being calibrated.

The following ArUco pattern dictionaries are supported.

|     |
| --- |
| DICT\_4X4\_250 |
| DICT\_5X5\_250 |
| DICT\_6X6\_250 |
| DICT\_7X7\_250 |
| DICT\_ARUCO\_ORIGINAL |

### Target Printing [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/manipulator_camera_calibration.html\#target-printing "Link to this heading")

The calibration targets must be rigid without any noticeable bending when flat and during operation.
We recommend using aluminum/LDPE composite for the required flatness and stiffness.

Note

We recommend using [calib.io](https://calib.io/) to obtain high quality calibration targets. The targets used in this tutorial
were purchased from calib.io.

If printing from a different source, ensure that the printed targets are:

> - Non-glossy to minimize light reflections and glare.
>
> - Black and white in color.

The calibration targets used in this tutorial are from [calib.io](https://calib.io/) and shown below: left - UR5e target, right - UR10e target.

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/calibration_targets.jpg/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/calibration_targets.jpg/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/calibration_targets.jpg/)

The specifications for the calibration targets used in this tutorial are as follows.

| Robot type | Board dimension | ChArUco resolution | ArUco dictionary |
| --- | --- | --- | --- |
| UR5e | 200x300 (mm) | 14x9 | DICT\_5X5\_250 |
| UR10e | 300x400 (mm) | 12x9 | DICT\_5X5\_250 |

## Physical Setup [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/manipulator_camera_calibration.html\#physical-setup "Link to this heading")

The calibration board should be mounted firmly to the end of the manipulator arm such that the board
does not shift its position when the arm is moved. Any relative movement between the board and arm during
calibration will result in inaccurate results. We recommend using a mount such as the one shown
[here](https://calib.io/products/robot-flange-mount).

A calibration target attached to a UR10e robot using the mount is shown below.

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/ur10_w_calibration_target.jpg/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/ur10_w_calibration_target.jpg/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/ur10_w_calibration_target.jpg/)[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/calibration_target_mount_rear.jpg/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/calibration_target_mount_rear.jpg/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/calibration_target_mount_rear.jpg/)

Note

The calibration board can also be held by the robot via a gripper. If using this method to hold the target and a portion of the
ChAuCo pattern is obstructed, ensure that the calibration tool is able to successfully detect the target. Refer to **Step 11** of
[Calibration Procedure](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/manipulator_camera_calibration.html#calibration-procedure) for instructions
on how to visualize the detection of the calibration target.

## Software Setup [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/manipulator_camera_calibration.html\#software-setup "Link to this heading")

1. Set up your development environment by following the instructions
here: [Developer Environment Setup](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html).

2. Set up the UR Robot by following the instructions
here: [Set Up UR Robot](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_e2e.html#set-up-ur-robot).

3. Complete the [Hawk setup tutorial](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/hawk_setup.html)
if using a Hawk stereo camera.
Complete the [RealSense setup tutorial](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/realsense_setup.html)
if using a RealSense camera.

4. Clone `isaac_ros_common` under `${ISAAC_ROS_WS}/src`.





```
cd ${ISAAC_ROS_WS}/src && \
       git clone -b release-3.2 https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common.git isaac_ros_common

```

Copy to clipboard

5. Clone `isaac_manipulator` under `${ISAAC_ROS_WS}/src`.





```
cd ${ISAAC_ROS_WS}/src && \
       git clone --recursive -b release-3.2 https://github.com/NVIDIA-ISAAC-ROS/isaac_manipulator.git isaac_manipulator

```

Copy to clipboard

6. Launch the Docker container using the `run_dev.sh` script.





```
cd $ISAAC_ROS_WS && ./src/isaac_ros_common/scripts/run_dev.sh

```

Copy to clipboard

7. Build from source and install the [moveit\_calibration package](https://github.com/illumo-robotics/moveit_calibration).





```
cd ${ISAAC_ROS_WS}
git clone https://github.com/illumo-robotics/moveit_calibration.git src/third_party
rosdep update && rosdep install -r --from-paths src/third_party/moveit_calibration --ignore-src --rosdistro ${ROS_DISTRO} -y
colcon build --symlink-install --packages-up-to-regex moveit_calibration* --cmake-args -DCMAKE_BUILD_TYPE=Release
source install/setup.bash

```

Copy to clipboard

8. Install the Isaac Manipulator packages. There are two options: installation
from source, or installation from Debian.



Installation from sourceInstallation from Debian





1. Use `rosdep` to install the package’s dependencies:


> > ```
> > sudo apt-get update
> >
> > ```
> >
> > Copy to clipboard
>
> ```
> rosdep update && \
>     rosdep install -i -r \
>     --from-paths ${ISAAC_ROS_WS}/src/isaac_manipulator/isaac_manipulator_bringup/ \
>     --rosdistro humble -y
>
> ```
>
> Copy to clipboard

2. Build and source the ROS workspace


> ```
> cd ${ISAAC_ROS_WS}
> colcon build --symlink-install --packages-up-to isaac_manipulator_bringup
> source install/setup.bash
>
> ```
>
> Copy to clipboard


## Calibration Procedure [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/manipulator_camera_calibration.html\#calibration-procedure "Link to this heading")

Note

Before proceeding, complete the steps in [Software Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/manipulator_camera_calibration.html#software-setup).

1. Open a new terminal window and launch the Docker container using the `run_dev.sh` script.





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
./scripts/run_dev.sh

```

Copy to clipboard


Note

We recommend setting a `ROS_DOMAIN_ID` via `export ROS_DOMAIN_ID=<ID_NUMBER>` for every
new terminal you run ROS commands to avoid interference
with other computers in the same network ( [ROS Guide](https://docs.ros.org/en/humble/Concepts/Intermediate/About-Domain-ID.html)).

1. Inside the container, source the workspace.





```
cd ${ISAAC_ROS_WS} && \
source install/setup.bash

```

Copy to clipboard

2. Launch the UR robot driver:





```
ros2 launch ur_robot_driver ur_control.launch.py ur_type:=<ROBOT_TYPE> robot_ip:=<ROBOT_IP_ADDRESS> launch_rviz:=false

```

Copy to clipboard



The supported robot types are: \[ `ur3e`, `ur5e`, `ur10e`, `ur16e`\]

3. Open a second terminal window and launch the Docker container using the `run_dev.sh` script.





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
./scripts/run_dev.sh

```

Copy to clipboard

4. Inside the container, source the workspace and launch the camera feed.



HawkRealSense





1. Install the Hawk stereo camera package.





```
sudo apt-get install -y ros-humble-isaac-ros-hawk

```

Copy to clipboard

2. Launch the camera feed.





```
source install/setup.bash
ros2 launch isaac_manipulator_bringup camera_calibration.launch.py camera_type:=hawk

```

Copy to clipboard

3. This launch file launches the camera pipeline for Hawk. After launch you can verify that camera topics are publishing data, e.g., `/left/rect/image`.


5. Open a third terminal window and launch the Docker container using the `run_dev.sh` script.





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
./scripts/run_dev.sh

```

Copy to clipboard


Note

If the RViz window with the **HandEyeCalibration** crashes, restart the window by rerunning the commands below.

01. Inside the container, source the workspace and launch the robot with RViz.





    ```
    source install/setup.bash
    ros2 launch isaac_ros_cumotion_examples ur.launch.py ur_type:=<ROBOT_TYPE> robot_ip:=<ROBOT_IP_ADDRESS> launch_rviz:=true

    ```

    Copy to clipboard



    The supported robot types are: \[ `ur3e`, `ur5e`, `ur10e`, `ur16e`\]

02. Close the `MotionPlanning` panel on the left side of the window. This must be done to allow for image panels to be added to RViz in a later step. **If this step is not done, the application may crash**.

03. In the RViz window that opens, click on **Add** \> **By display type** \> **moveit\_calibration\_gui** \> **HandEyeCalibration**.
    Click on **OK** to add the calibration panel to the window. You should see a new panel added to the left side of the RViz
    window titled **HandEye Calibration**.
    [![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/hawk_add.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/hawk_add.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/hawk_add.png/)
04. In the **HandEye Calibration** panel, click on the **Target** tab and set the following parameters.



    |     |     |
    | --- | --- |
    | Target Type | HandEyeTarget/Charuco |
    | squares, X | _Number of squares in calibration target along X direction_ |
    | squares, Y | _Number of squares in calibration target along Y direction_ |
    | marker size (px) | 1500 |
    | square size (px) | 2400 |
    | margin size (px) | 60 |
    | marker border (bits) | _Number of bits around marker border_ |
    | ArUco dictionary | DICT\_5X5\_250 |
    | longest board side (m) | _Measured length of longest side of the board pattern in meters_ |
    | measured marker size (m) | _Measured length of marker in meters_ |



    The following values may be used directly if using the [calib.io](https://calib.io/) calibration targets that were recommended in the
    [Calibration Target](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/manipulator_camera_calibration.html#calibration-target) section above.



    |     |     |
    | --- | --- |
    | Target Type | `HandEyeTarget/Charuco` |
    | squares, X | `12` (for UR10e target), `14` (for UR5e target) |
    | squares, Y | `9` |
    | marker size (px) | `1500` |
    | square size (px) | `2400` |
    | margin size (px) | `60` |
    | marker border (bits) | `1` |
    | ArUco dictionary | `DICT_5X5_250` |
    | longest board side (m) | `0.36` (for UR10e target), `0.28` (for UR5e target) |
    | measured marker size (m) | `0.022` (for UR10e target), `0.015` (for UR5e target) |



    Once the above values have been set, click on **Create Target**.

    Lastly, click on the **Camera Image Topic** drop down menu and select the appropriate image topic.
    For Hawk the topic is `/left/rect/image`, for RealSense the topic is `/camera_1/color/image_raw`.
    [![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/hawk_target_small.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/hawk_target_small.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/hawk_target_small.png/)
05. In the **HandEye Calibration** panel, click on the **Context** tab and set the following parameters.



    |     |     |
    | --- | --- |
    | Sensor Configuration | Eye-to-hand |
    | Sensor frame | `hawk` (Hawk); `camera_1_link` (RealSense) |
    | Object frame | `handeye_target` |
    | End-effector frame | `wrist_3_link` |
    | Robot base frame | `world` |





    Note



    After setting the `Sensor frame`, the UI may freeze for a few seconds before becoming responsive again.


    [![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/hawk_context_small.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/hawk_context_small.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/hawk_context_small.png/)
06. Add a panel to visualize the calibration target detection by clicking
    **Add** \> **By topic** \> **/handeye\_calibration** \> **/target\_detection** \> **Image**. Click on **OK** to add.
    [![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/calibration_target_vis_rviz_add.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/calibration_target_vis_rviz_add.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/calibration_target_vis_rviz_add.png/)
    You should see a new panel added to the left side of the RViz
    window titled **Image** which shows the detected pose of the calibration target.
    [![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/calibration_target_detection.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/calibration_target_detection.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/calibration_target_detection.png/)
07. Add TF visualizations for the camera and target. In the RViz window, click on **Add** \> **By display type** \> **rviz\_default\_plugins** \> **TF**.
    Click on **OK** to add the visualization to the window.
    You should now see an entry titled **TF** in the **Displays** panel on the left side of the window. Expand
    the **TF** entry and then click on **Frames** and deselect the option titled **All Enabled**. Scroll through the
    list of frames and re-enable `hawk` (for Hawk), `camera_1_link` (for RealSense), and `handeye_target`
    (for both Hawk or RealSense). These are the camera frame and target frame, respectively.

08. In the **HandEye Calibration** panel, click on the **Context** tab and use the sliders under **Camera Pose Initial Guess**
    to provide an initial pose estimation. Adjust the translation and rotation values until the TF visualizations on the
    right side of the window roughly correspond with the position of the camera and target board in real life with respect
    to the manipulator arm.

09. In the **HandEye Calibration** panel, click on the **Calibrate** tab. Make sure the calibration board is visible in the camera
    image. Move the manipulator arm to a desired pose and click **Take sample**. If the sample is successfully taken, it will
    it appear in the box titled **Pose samples**. After a minimum of 5 samples are taken, the tool will compute a camera pose
    together with a corresponding reprojection error (it appears at the bottom-left of the **HandEyeCalibration** window). Continue to move the manipulator arm and collect samples until the desired number
    of samples is achieved.



    Note



    Adding more diverse samples will typically lead to a more robust calibration. It is recommended that a minimum of **2** translational poses are used with approximately **10** rotations for each translation.
    The rotations should be made by moving the board along all three axes (roll, pitch, yaw) at the same time to ensure each rotation
    is sufficiently different from the prior. Additional rotations can be added to improve the accuracy of the calibration.
    The translational poses should be positioned within the workspace of the manipulator arm.





    Note



    Verify that reprojection error is low enough for your application, and that the camera
    position in RViz looks reasonable and aligns with the simulated robot.
    The procedure described in these instructions have been tested to provide reprojection error
    under 0.002 m and 0.002 rad.



    Once the desired number of samples are taken, save the calibration results by clicking **Save camera pose**.
    [![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/hawk_calibrate_small.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/hawk_calibrate_small.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/manipulator_camera_calibration/hawk_calibrate_small.png/)


    Note



    Different solvers yield different calibration results, we recommend experimenting with different solvers to see which yields the
    best results for your use case. To select a different solver, click on the drop down menu next to **AX=XB Solver** and click
    on the desired solver. Then click on the **Solve** button to recompute the calibration results with the selected solver.



    The `calibration.launch.py` output from the calibration tool will include a code snippet similar to the following example (values included for illustration only as they will differ for different setups):





    ```
    """ Static transform publisher acquired via MoveIt 2 hand-eye calibration """
    """ EYE-TO-HAND: world -> camera """
    from launch import LaunchDescription
    from launch_ros.actions import Node


    def generate_launch_description() -> LaunchDescription:
        nodes = [\
            Node(\
                package="tf2_ros",\
                executable="static_transform_publisher",\
                output="log",\
                arguments=[\
                    "--frame-id",\
                    "world",\
                    "--child-frame-id",\
                    "camera",\
                    "--x",\
                    "1.77278",\
                    "--y",\
                    "0.939827",\
                    "--z",\
                    "-0.0753478",\
                    "--qx",\
                    "-0.128117",\
                    "--qy",\
                    "-0.0317539",\
                    "--qz",\
                    "0.955077",\
                    "--qw",\
                    "-0.265339",\
                    # "--roll",\
                    # "0.132507",\
                    # "--pitch",\
                    # "-0.229891",\
                    # "--yaw",\
                    # "-2.5843",\
                ],\
            ),\
        ]
        return LaunchDescription(nodes)

    ```

    Copy to clipboard

10. The calibrated pose values can now be used with the rest of the [Isaac Manipulator](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/index.html) suite
    by updating the values in [static\_transforms.launch.py](https://github.com/NVIDIA-ISAAC-ROS/isaac_manipulator/blob/main/isaac_manipulator_bringup/launch/include/static_transforms.launch.py)
    as described [here](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_e2e.html#set-up-cameras-for-robot), and specifying the frame name defined
    during [Camera Setup](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_manipulator/tutorials/tutorial_e2e.html#set-up-cameras-for-robot) in the `--child-frame-id` field.
    For dual-camera setups, make sure to use a different frame name for each calibrated camera.


Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/getting_started/hardware_setup/sensors/manipulator_camera_calibration.html)[latest](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/manipulator_camera_calibration.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/index.html)