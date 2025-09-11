- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Reference Workflows](https://nvidia-isaac-ros.github.io/reference_workflows/index.html)
- [Isaac Perceptor](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/index.html)
- Troubleshooting
- [View page source](https://nvidia-isaac-ros.github.io/_sources/reference_workflows/isaac_perceptor/troubleshooting.rst.txt)

* * *

# Troubleshooting [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/troubleshooting.html\#troubleshooting "Link to this heading")

## NITROS Warning: Failed to get the rectified camera model [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/troubleshooting.html\#nitros-warning-failed-to-get-the-rectified-camera-model "Link to this heading")

When running an app using the HAWK stereo cameras, a warning is printed
concerning the rectified camera model not being found. It is safe to ignore this
warning.

### Symptom [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/troubleshooting.html\#symptom "Link to this heading")

```
[component_container_mt-3] [WARN] [1716418145.724108453] [NitrosCameraInfo]: [convert_to_ros_message] Failed to get the Rectified CameraModel object: Falling back to raw camera model
[component_container_mt-3] [WARN] [1716418145.724193797] [NitrosCameraInfo]: [convert_to_ros_message] Failed to get the target extrinsics delta Pose3D object: GXF_ENTITY_COMPONENT_NOT_FOUND Falling back to extrinsics

```

Copy to clipboard

### Solution [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/troubleshooting.html\#solution "Link to this heading")

This warning can be ignored safely.

## The robot starts navigating but stops moving after approximately 5 seconds [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/troubleshooting.html\#the-robot-starts-navigating-but-stops-moving-after-approximately-5-seconds "Link to this heading")

### Symptom [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/troubleshooting.html\#id1 "Link to this heading")

When setting a navigation goal for the robot, it initially starts moving, but
stops after approximately 5 seconds. Setting a new navigation goal results in
the same behavior.

### Solution [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/troubleshooting.html\#id2 "Link to this heading")

This issue occurs when the goal pose is not sent in the same frame that the
global planner is using, i.e., in most cases the `map` frame.
This can happen if the “Display Frame” in Foxglove’s [3D panel](https://docs.foxglove.dev/docs/visualization/panels/3d/) (or the “Fixed
Frame” in RVIZ2) is not set to the `map` frame.

Please ensure that this frame is set to the `map` frame.

## Not all topics are visible in Foxglove for visualization [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/troubleshooting.html\#not-all-topics-are-visible-in-foxglove-for-visualization "Link to this heading")

### Symptom [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/troubleshooting.html\#id3 "Link to this heading")

Some topics are visible locally when using `ros2 topic list` or
`ros2 topic echo /my/topic` but they are not visible in Foxglove.

### Solution [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/troubleshooting.html\#id4 "Link to this heading")

Per default, we use Foxglove’s `topic_whitelist` parameter to disable
visualization of all topics. This is to keep users from accidentally visualizing
bandwidth-heavy topics such as raw camera images, which can decrease the
quality of the visualization.

Additionally, visualizing raw images will trigger a GPU-to-CPU conversion of the
corresponding NITROS messages which will increase the load on the system.

If a specific use case requires streaming all topics to Foxglove the whitelist
can be disabled with the launch parameter `use_foxglove_whitelist:=False`.

## Frame drops when starting up Isaac Perceptor [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/troubleshooting.html\#frame-drops-when-starting-up-isaac-perceptor "Link to this heading")

When starting up Isaac Perceptor it can happen that the cameras are dropping
frames and therefore the VSLAM node also doesn’t receive enough frames. This is
only expected to happen during and shortly after (1-2 seconds) the type
negotiation period. This is not an issue as we expect the robot to be stationary
during startup. It is safe to ignore this warning.

### Symptom [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/troubleshooting.html\#id5 "Link to this heading")

```
[component_container_mt-3] 2024-05-24 21:20:46.457 WARN  extensions/hawk/components/argus_camera.cpp@1553: Frame drop detected in module_id 5 camera_id 0
[component_container_mt-3] [WARN] [1716578372.175037514] [visual_slam_node]: Delta between current and previous frame [66.676000 ms] is above threshold [34.000000 ms]

```

Copy to clipboard

### Solution [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/troubleshooting.html\#id6 "Link to this heading")

This warning can be ignored safely.

## Observed false positives people reconstruction results [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/troubleshooting.html\#observed-false-positives-people-reconstruction-results "Link to this heading")

When launching the Isaac Perceptor app with
stereo\_camera\_configuration:=front\_people\_configuration, people reconstruction identifies
false positives.

### Symptom [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/troubleshooting.html\#id7 "Link to this heading")

Wrong voxels are marked as red in Foxglove visualization.

### Solution [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/troubleshooting.html\#id8 "Link to this heading")

People are expected to be not too close to the front stereo camera, and to be visible to its field
of view. People shall appear in the camera view without much occlusion across a few frames.
The false positives shall not remain across multiple frames.
To learn more about how people reconstruction works, please refer to
[people reconstruction in nvblox](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/technical_details.html#people-reconstruction).

## Data recorder reports failure to shutdown a ROS adapter [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/troubleshooting.html\#data-recorder-reports-failure-to-shutdown-a-ros-adapter "Link to this heading")

When terminating the recording app as suggested by
[Recording Data for Isaac Perceptor](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorial_recording_and_playback.html#recording-data-for-isaac-perceptor),
it reports an error in the log.

### Symptom [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/troubleshooting.html\#id9 "Link to this heading")

```
[ERROR] [launch]: Caught exception in launch (see debug for traceback): Cannot shutdown a ROS adapter that is not running

```

Copy to clipboard

### Solution [](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/troubleshooting.html\#id10 "Link to this heading")

This warning can be ignored safely.

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/reference_workflows/isaac_perceptor/troubleshooting.html)[latest](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/troubleshooting.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/reference_workflows/isaac_perceptor/troubleshooting.html)