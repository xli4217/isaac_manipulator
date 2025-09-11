- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Concepts](https://nvidia-isaac-ros.github.io/concepts/index.html)
- [Segmentation](https://nvidia-isaac-ros.github.io/concepts/segmentation/index.html)
- Tutorial for Segment Anything with Isaac Sim
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/segmentation/segment_anything/tutorial_isaac_sim.rst.txt)

* * *

# Tutorial for Segment Anything with Isaac Sim [](https://nvidia-isaac-ros.github.io/concepts/segmentation/segment_anything/tutorial_isaac_sim.html\#tutorial-for-segment-anything-with-isaac-sim "Link to this heading")

![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/segmentation/segment_anything/segment_anything_isaac_sim.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/segmentation/segment_anything/segment_anything_isaac_sim.png/)

## Overview [](https://nvidia-isaac-ros.github.io/concepts/segmentation/segment_anything/tutorial_isaac_sim.html\#overview "Link to this heading")

This tutorial demonstrates how to:

1. Setup and stream images using [Isaac Sim](https://nvidia-isaac-ros.github.io/getting_started/isaac_sim/index.html).

2. Segment objects using [Isaac ROS Segment Anything](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_image_segmentation/blob/main/isaac_ros_segment_anything).

3. Detect object bounding boxes using [Isaac ROS YoloV8 object detection](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_object_detection/blob/main/isaac_ros_yolov8).


## Tutorial Walkthrough [](https://nvidia-isaac-ros.github.io/concepts/segmentation/segment_anything/tutorial_isaac_sim.html\#tutorial-walkthrough "Link to this heading")

1. Complete the [quickstart](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segment_anything/index.html#quickstart) up until model preparation step.

2. Complete the [Isaac ROS YoloV8 tutorial](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_object_detection/isaac_ros_yolov8/index.html#quickstart) up until the build step.

3. Launch the Docker container using the `run_dev.sh` script:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
     ./scripts/run_dev.sh

```

Copy to clipboard

4. Install and launch Isaac Sim following the steps in the [Isaac ROS Isaac Sim Setup Guide](https://nvidia-isaac-ros.github.io/getting_started/isaac_sim/index.html).

5. Press **Play** to start publishing data from the Isaac Sim.
[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/isaac_sim_sample_scene.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/isaac_sim_sample_scene.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/getting_started/isaac_sim_sample_scene.png/)
6. Run the following launch files to start the inferencing:


> ```
> ros2 launch isaac_ros_segment_anything isaac_ros_segment_anything_isaac_sim.launch.py model_file_path:=${ISAAC_ROS_WS}/isaac_ros_assets/models/yolov8/yolov8s.onnx engine_file_path:=${ISAAC_ROS_WS}/isaac_ros_assets/models/yolov8/yolov8s.plan confidence_threshold:=0.25 nms_threshold:=0.45 model_repository_paths:=[${ISAAC_ROS_WS}/isaac_ros_assets/models]
>
> ```
>
> Copy to clipboard

7. Start a new terminal and attach to the container.


> ```
> cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
>  ./scripts/run_dev.sh
>
> ```
>
> Copy to clipboard
>
> Then run the Python script to generate the colored segmentation mask from raw mask.
>
> ```
> ros2 run isaac_ros_segment_anything visualize_mask.py
>
> ```
>
> Copy to clipboard

8. Visualize and validate the output of the package by launching `rqt_image_view`. In another terminal enter the Docker container:


> ```
> cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
>     ./scripts/run_dev.sh
>
> ```
>
> Copy to clipboard
>
> Then launch `rqt_image_view`:
>
> > ```
> > ros2 run rqt_image_view rqt_image_view
> >
> > ```
> >
> > Copy to clipboard
>
> Inside the `rqt_image_view` GUI, change the topic to `/segment_anything/colored_segmentation_mask` to view a colorized segmentation mask.

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/segmentation/segment_anything/tutorial_isaac_sim.html)[latest](https://nvidia-isaac-ros.github.io/concepts/segmentation/segment_anything/tutorial_isaac_sim.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/segmentation/segment_anything/tutorial_isaac_sim.html)