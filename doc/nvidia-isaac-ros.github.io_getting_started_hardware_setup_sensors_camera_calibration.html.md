- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Getting Started](https://nvidia-isaac-ros.github.io/getting_started/index.html)
- [Hardware Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/index.html)
- [Sensors Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/index.html)
- Monocular Camera Calibration
- [View page source](https://nvidia-isaac-ros.github.io/_sources/getting_started/hardware_setup/sensors/camera_calibration.rst.txt)

* * *

# Monocular Camera Calibration [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/camera_calibration.html\#monocular-camera-calibration "Link to this heading")

Note

These instructions are specifically for a monocular camera.
To calibrate a stereoscopic camera, see [the camera\_calibration\\
instructions](http://wiki.ros.org/camera_calibration).

01. Set up your development environment by following the
    [Developer Environment Setup](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html).

02. Print a large checkerboard with known dimensions.

    This tutorial uses a 6x8 checkerboard with 200mm squares.
    Calibration uses the interior vertex points of the checkerboard, so
    a “7x9” board uses the interior vertex parameter “6x8” as in the
    example below. Checkerboards with specific dimensions can be
    downloaded
    [here](https://calib.io/pages/camera-calibration-pattern-generator).

03. Clone the ROS 2 `usb_cam` package:





    ```
    cd ${ISAAC_ROS_WS}/src && \
      git clone -b ros2 https://github.com/ros-drivers/usb_cam

    ```

    Copy to clipboard





    Note



    Your camera vendor might offer a specific ROS
    2-compatible camera driver package that can be used in place of
    the `usb_cam` package.

04. Launch the Docker container using the `run_dev.sh` script:





    ```
    cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
      ./scripts/run_dev.sh

    ```

    Copy to clipboard

05. Inside the container, build, and source the workspace:





    ```
    cd /workspaces/isaac_ros-dev && \
      colcon build --symlink-install && \
      source install/setup.bash

    ```

    Copy to clipboard

06. Run the `usb_cam` image publisher:





    ```
    ros2 run usb_cam usb_cam_node_exe --remap __ns:=/my_camera --ros-args -p framerate:=30.0 -p image_height:=720 -p image_width:=1280

    ```

    Copy to clipboard

07. Attach another terminal to the Docker container using the
    `run_dev.sh` script:





    ```
    cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
      ./scripts/run_dev.sh

    ```

    Copy to clipboard

08. Run the camera calibrator:





    ```
    ros2 run camera_calibration cameracalibrator --size 6x8 --square 0.20 image:=/my_camera/image_raw camera:=/my_camera

    ```

    Copy to clipboard

09. Complete steps 6-8 from [this camera calibration\\
    tutorial](https://docs.nav2.org/tutorials/docs/camera_calibration.html).

10. Make sure you press **Calibrate** and then the **Save** button.

    You should see the following line in the second terminal:





    ```
    ('Wrote calibration data to', '/tmp/calibrationdata.tar.gz')

    ```

    Copy to clipboard



    The calibration file will be stored at
    `/tmp/calibrationdata.tar.gz`.

11. After the calibration file has been saved, enter `Ctrl+C` in each
    terminal to stop the nodes.

12. Move the calibration file to the required location:





    ```
    cd /workspaces/isaac_ros-dev/src/<isaac_ros_metapackage>/<isaac_ros_package>/config/ && \
    tar -xvf /tmp/calibrationdata.tar.gz -C ./ ost.yaml && \
    mv ost.yaml camera_info.yaml

    ```

    Copy to clipboard


Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/getting_started/hardware_setup/sensors/camera_calibration.html)[latest](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/camera_calibration.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/getting_started/hardware_setup/sensors/camera_calibration.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/getting_started/hardware_setup/sensors/camera_calibration.html)