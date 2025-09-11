- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Concepts](https://nvidia-isaac-ros.github.io/concepts/index.html)
- [Visual SLAM](https://nvidia-isaac-ros.github.io/concepts/visual_slam/index.html)
- cuVSLAM
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/visual_slam/cuvslam/index.rst.txt)

* * *

# cuVSLAM [](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/index.html\#cuvslam "Link to this heading")

cuVSLAM is a GPU-accelerated library for stereo-visual-inertial SLAM
and odometry.

All SLAM-related operations work in parallel to visual odometry in a
separate thread. Input images get copied into GPU and then cuVSLAM
starts tracking.

cuVSLAM uses the following:

- 2D features on the input images

- Landmarks: The patch of pixels in the source images with coordinates
of the feature associated with the position of the camera from where
that feature is visible.

- PoseGraph: A graph that tracks the poses of the camera from where the
landmarks are viewed.


Landmarks and PoseGraph are stored in a data structure that does not
increase its size in the case where the same landmark is visited more
than once.

Imagine a robot moving around and returning to the same place. Because
odometry always has some accumulated drift, a deviation in
trajectory from the ground truth can be seen. SLAM can be used to solve this
problem.

During the robot’s motion, SLAM stores all landmarks and it continuously
verifies whether each landmark has been seen before. When cuVSLAM recognizes
several landmarks, it determines their position from the data structure.

At the moment of recognition, a connection is added to the PoseGraph, which makes a
loop from the free trail of the poses. This event is called a ‘loop
closure’. Immediately after that, cuVSLAM performs a graph optimization
that leads to the correction of the current odometry pose and all
previous poses in the graph.

The procedure for adding landmarks is designed such that if a landmark is not
in the expected location, then it is marked for eventual deletion. This allows
you to use cuVSLAM over a changing terrain.

cuVSLAM does not just work with a single stereo camera. It is possible to use
up to 32 cameras (=16 stereo cameras) as input. Using multiple cameras can
significantly increase robustness, especially in featureless environments.

Along with visual data, cuVSLAM can use Inertial Measurement Unit (IMU)
measurements. It automatically switches to IMU when VO is unable to
estimate a pose. For example, when there is dark lighting or long solid
surfaces in front of a camera. Typically, using an IMU leads to a
significant performance improvement in poor visual conditions.

For severe degradation of image input (such as lights being turned off,
dramatic motion blur on a bump while driving), you can explore the
following motion estimation algorithms to ensure acceptable quality
for pose tracking:

- The IMU readings integrator provides acceptable pose tracking quality
for about ~< 1 seconds.

- For IMU failure, the constant velocity integrator continues to
provide the last linear and angular velocities reported by Stereo VIO
before failure. This provides acceptable pose tracking quality for
~0.5 seconds.


## List of Useful Visualizations [](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/index.html\#list-of-useful-visualizations "Link to this heading")

- `visual_slam/vis/observations_cloud` \- Point cloud for 2D Features

- `visual_slam/vis/landmarks_cloud` \- All mapped landmarks

- `visual_slam/vis/loop_closure_cloud` \- Landmarks visible in last loop closure

- `visual_slam/vis/pose_graph_nodes` \- Pose graph nodes

- `visual_slam/vis/pose_graph_edges` \- Pose graph edges


## Saving a Map [](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/index.html\#saving-a-map "Link to this heading")

Optionally, save the stored landmarks and pose graph in a map using the ROS 2
`SaveMap` service that saves the map to the disk.

## Loading and Localizing in a Map [](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/index.html\#loading-and-localizing-in-a-map "Link to this heading")

After the map has been saved to the disk, it can be used to
localize the robot. To load the map into the memory, you can use the ROS
2 service called `LocalizeInMap`. It requires a map file path and
a prior pose, which is an initial guess of where the robot is in the
map. Given the prior pose and current set of camera frames, cuVSLAM
tries to find the pose of the landmarks in the requested map that
matches the current set. If the localization is successful, cuVSLAM will
load the map in the memory. Otherwise, it will continue building a new
map.

Both `SaveMap` and `LocalizeInMap` can take some time to
complete. Hence, they are designed to be asynchronous to avoid
interfering with odometry calculations.

## Coordinate Frames [](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/index.html\#coordinate-frames "Link to this heading")

This section describes the coordinate frames that are involved in the
`VisualSlamNode`:

1. `base_frame`: The base frame of the robot or camera rig. It is assumed that
all cameras are rigidly mounted to this frame, such that the transforms between
the `base_frame` and the `camera_optical_frames` can assumed to be static.
The estimated odometry and SLAM pose will represent the pose of this frame.

2. `map_frame`: The frame corresponding to the origin of the map that cuVSLAM
creates or localizes in. The SLAM pose reported by cuVSLAM corresponds with
the transform `map_frame` -\> `base_frame`.

3. `odom_frame`: The frame corresponding to the origin of the odometry that
cuVSLAM estimates. The odometry pose reported by cuVSLAM corresponds with the
transform `odom_frame` -\> `base_frame`.

4. `camera_optical_frames`: The frames corresponding to the optical center of
every camera. The frame is expected to lie at the center of the camera lens
and oriented with the z-axis pointing in the view direction and the y-axis
pointing to the floor.


## Repositories and Packages [](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/index.html\#repositories-and-packages "Link to this heading")

The Isaac ROS implementations of this technology are available here:

- [Isaac ROS Visual SLAM](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_visual_slam/index.html)


## Examples [](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/index.html\#examples "Link to this heading")

To get started with cuVSLAM, review the following examples:

- [Tutorial for Visual SLAM with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_isaac_sim.html)
- [Tutorial for Visual SLAM Using a RealSense Camera with Integrated IMU](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_realsense.html)
- [Tutorial for Visual SLAM Using a HAWK Camera](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_hawk.html)
- [Tutorial for Multi-camera Visual SLAM Using HAWK Cameras](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/tutorial_multi_hawk.html)

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/visual_slam/cuvslam/index.html)[latest](https://nvidia-isaac-ros.github.io/concepts/visual_slam/cuvslam/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/concepts/visual_slam/cuvslam/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/visual_slam/cuvslam/index.html)