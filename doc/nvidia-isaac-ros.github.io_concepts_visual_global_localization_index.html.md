- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Concepts](https://nvidia-isaac-ros.github.io/concepts/index.html)
- Visual Global Localization
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/visual_global_localization/index.rst.txt)

* * *

# Visual Global Localization [](https://nvidia-isaac-ros.github.io/concepts/visual_global_localization/index.html\#visual-global-localization "Link to this heading")

## Overview of Global Localization [](https://nvidia-isaac-ros.github.io/concepts/visual_global_localization/index.html\#overview-of-global-localization "Link to this heading")

Global localization is a process in robotics and computer vision to determine a device’s
or robot’s position in an environment when its initial location is unknown. This contrasts
with local localization, where the starting position is provided.

Global localization is essential for applications such as autonomous navigation,
where a robot needs to determine its position to navigate efficiently through its surroundings.
It is especially useful in environments where GPS signals are weak or unavailable,
such as indoors or in urban canyons.

By using visual cues and other sensor data,
global localization helps in building a comprehensive understanding of the environment,
enabling tasks like mapping, navigation, and interaction with the physical world.

## What is cuVGL? [](https://nvidia-isaac-ros.github.io/concepts/visual_global_localization/index.html\#what-is-cuvgl "Link to this heading")

cuVGL, short for CUDA-accelerated Visual Global Localization, is a library designed for efficient
and accurate global localization tasks using GPU acceleration.
The library focuses on two main components:

- **image retrieval via bag of words (BoW)**

- **relative pose estimation using stereo images**


cuVGL uses external poses, such as those provided by a SLAM system,
along with images to produce a keyframe database. This database is later used for global localization,
making cuVGL a flexible component for systems that do not have global localization capabilities.

### Map Creation Process [](https://nvidia-isaac-ros.github.io/concepts/visual_global_localization/index.html\#map-creation-process "Link to this heading")

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_global_localization/map_creation_process.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_global_localization/map_creation_process.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_global_localization/map_creation_process.png/)

The map creation process in cuVGL is unique as it consumes external poses and images to build a **keyframe database**,
rather than generating a traditional map like a sparse feature map.

The input for the map creation includes:

- **stereo camera images**

- **poses from an external source** (for example, a SLAM system)


The output map includes:

- **keyframe database**: storing the map keyframes and the associated poses.

- **BoW vocabulary**

- **BoW image retrieval index**: with BoW vocabulary, enabling fast image retrieval.


### Localization Process [](https://nvidia-isaac-ros.github.io/concepts/visual_global_localization/index.html\#localization-process "Link to this heading")

[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_global_localization/localization_process.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_global_localization/localization_process.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_global_localization/localization_process.png/)

Localization in cuVGL is performed using stereo images for relative pose estimation. The process involves:

- **image retrieval**: for the current input image, cuVGL retrieves the best matching candidates from the keyframe database using the BoW image retrieval index.

- **relative pose estimation**: after matching candidates are identified, the library computes the relative pose between the input stereo image and the map image candidate.

- **absolute pose calculation**: by applying the relative transform to the mapping pose of the map image (recorded during the mapping phase), cuVGL outputs an absolute pose for the current input image.


Note

Please follow these guidelines for good localization performance:

1. Map the entire environment to ensure the vocabulary contains a sufficient number of visual words for accurately describing the environment.

2. Keep the localization trajectory within 1 meter from the mapping trajectory.


## Metrics of cuVGL [](https://nvidia-isaac-ros.github.io/concepts/visual_global_localization/index.html\#metrics-of-cuvgl "Link to this heading")

The benchmark metrics include:

- **translation and rotation error**: pose errors computed against the ground-truth poses.

- **success rate**: defined as the ratio of times the localizer returns a pose to the total number of localization calls. It indicates how often the localizer generates a pose during operation.

- **runtime**: the average time taken by localization call.


We collected two rosbags in one of our lab (25m x 42m room) with the [Nova Carter](https://nvidia-isaac-ros.github.io/robots/nova_carter/index.html),
one for creating a localization map, and the other for running localization on that map. The two bags were collected along
similar routes to ensure the map provides good coverage for localization. The benchmarks were run on x86\_64.

[![Occupancy Map for the Hubble Lab](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_global_localization/hubble_lab_occupancy_map.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_global_localization/hubble_lab_occupancy_map.png/)

- For **map creation**, we extracted 951 samples from the rosbag based on translation and rotation distance threshold. Each sample is associated with four stereo pairs, so total 7608 keyframes are used for building the global localization map. For more details about map creation, refer to the [Tutorial: Mapping and Localization with Isaac Perceptor](https://nvidia-isaac-ros.github.io/reference_workflows/isaac_perceptor/tutorial_mapping_and_localization.html).

- For **localization**, there are total 7280 samples in the localization bag. We ran an offline tool that processed all samples with different stereo configurations.


| Metric | Front Stereo | Front, Left, Right Stereo | Front, Left, Right, Back Stereo |
| --- | --- | --- | --- |
| Mean Translation Error | 0.08 m | 0.07 m | 0.07 m |
| 99% Translation Error | 0.34 m | 0.20 m | 0.20 m |
| Max Translation Error | 2.68 m | 0.39 m | 0.42 m |
| Mean Rotation Error | 0.57 deg | 0.57 deg | 0.57 deg |
| 99% Rotation Error | 1.71 deg | 1.71 deg | 1.71 deg |
| Max Rotation Error | 12.0 deg | 2.86 deg | 2.86 deg |
| Success Rate | 72% | 75% | 85% |
| Runtime | 0.34 sec | 0.70 sec | 0.85 sec |

Stereo Configuration Comparison [](https://nvidia-isaac-ros.github.io/concepts/visual_global_localization/index.html#id1 "Link to this table")

|     |
| --- |
| **Trajectory of Ground-Truth Poses Shown in Gray, and Localization Poses Colored Based on Their Translation and Rotation Error** |
| ![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_global_localization/absolute_error_location_3_stereos.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_global_localization/absolute_error_location_3_stereos.png/) |
| **Histogram of Translation and Rotation Error for Localization Poses** |
| ![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_global_localization/absolute_error_hist_3_stereos.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_global_localization/absolute_error_hist_3_stereos.png/) |
| **Translation and Rotation Errors in the Localization Trajectory Over Time** |
| ![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_global_localization/absolute_error_line_3_stereos.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/visual_global_localization/absolute_error_line_3_stereos.png/) |

Localization Results Using Front, Left, Right Stereo Configuration [](https://nvidia-isaac-ros.github.io/concepts/visual_global_localization/index.html#id2 "Link to this table")

## Camera System Requirements [](https://nvidia-isaac-ros.github.io/concepts/visual_global_localization/index.html\#camera-system-requirements "Link to this heading")

cuVGL requires one or more stereo cameras. The camera system providing data must meet the following specifications:

| Specification | Required Specification |
| --- | --- |
| Minimum target image framerate | 5 Hertz |
| Timestamp synchronization threshold for images from the same stereo camera | \+/\- 10 microseconds |
| Timestamp synchronization threshold for images across stereo cameras | \+/\- 100 microseconds |

## Repositories and Packages [](https://nvidia-isaac-ros.github.io/concepts/visual_global_localization/index.html\#repositories-and-packages "Link to this heading")

The following are the major packages related to cuVGL:

- The core implementations of cuVGL in following repository, details see:

  - [Isaac Mapping](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/isaac_mapping/index.html)
- The localization ROS wrapper of cuVGL in following ROS package, details see:

  - [Isaac ROS Visual Global Localization](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_ros_visual_global_localization/index.html)

## Tutorials [](https://nvidia-isaac-ros.github.io/concepts/visual_global_localization/index.html\#tutorials "Link to this heading")

To get started with cuVGL, review the following examples:

- [Tutorial for cuVGL Map Creation](https://nvidia-isaac-ros.github.io/concepts/visual_global_localization/tutorials/tutorial_map_creation.html)
- [Tutorial for cuVGL Localization](https://nvidia-isaac-ros.github.io/concepts/visual_global_localization/tutorials/tutorial_localization.html)

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/visual_global_localization/index.html)[latest](https://nvidia-isaac-ros.github.io/concepts/visual_global_localization/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/index.html)