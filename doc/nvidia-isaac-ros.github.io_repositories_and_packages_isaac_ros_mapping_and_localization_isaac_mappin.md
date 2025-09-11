- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Mapping And Localization](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/index.html)
- Isaac Mapping
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/isaac_mapping/index.rst.txt)

* * *

# Isaac Mapping [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/isaac_mapping/index.html\#package-name "Link to this heading")

## Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/isaac_mapping/index.html\#overview "Link to this heading")

This package includes the core implementations for visual localization and mapping, which are:

- cuVGL, short for CUDA-accelerated Visual Global Localization, details see: [Visual Global Localization](https://nvidia-isaac-ros.github.io/concepts/visual_global_localization/index.html).

- cuSFM, short for CUDA-accelerated Structure from Motion.


This is a list of tools provided by the package:

- `feature_extractor_main`: a tool to extract image features from raw images.

- `generate_bow_vocabulary_main`: a tool to create, refine or incrementally build a BoW vocabulary.

- `generate_bow_index_main`: a tool to generate BoW image retrieval index for images.


It is a CMake package inside [Isaac Mapping ROS](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/index.html), and it doesn’t depend on ROS.

## cuVGL [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/isaac_mapping/index.html\#cuvgl "Link to this heading")

This is an example of creating cuVGL and calling localization with images:

```
using namespace nvidia;

isaac::visual::cuvgl::cuVGL cuvgl;

std::string glmap_dir = "<glmap dir>";
std::string localizer_config_dir = "<localizer config dir>";

// Initialize
cuvgl.Init(glmap_dir, localizer_config_dir);

// Add camera sensors
std::vector<isaac::common::transform::Transform3D> cam_to_vehicles(2);
std::vector<isaac::common::image::MonoCameraCalibrationParams> cam_params(2);

cuvgl.AddCamera(0, cam_to_vehicles[0], cam_params[0]);
cuvgl.AddCamera(1, cam_to_vehicles[1], cam_params[1]);

// Specify the camera id for stereo pairs.
cuvgl.SetStereoCameraParamsIDFromString("0,1");

// Localize with camera images
std::vector<isaac::visual::cuvgl::CameraImage> camera_images(2);
isaac::common::transform::SE3TransformD vehicle_to_map;
cuvgl.Localize(camera_images, vehicle_to_map);

```

Copy to clipboard

## Tools [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/isaac_mapping/index.html\#tools "Link to this heading")

You can use `-help` to find the command line flags.

### feature\_extractor\_main [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/isaac_mapping/index.html\#feature-extractor-main "Link to this heading")

It extracts image feature for raw images in a directory and saves the extracted feature to the output directory.
The feature type is sift. It supports sampling images based on a translation and rotation distance.

```
feature_extractor_main \
    --input_image_directory <input_image_dir> \
    --output_dir <output_keyframe_dir> \
    --keypoint_creation_config configs/keypoint_creation_config.pb.txt

```

Copy to clipboard

These are the available options:

- `input_image_directory`: required, the input raw image directory which should include raw images and `frames_metadata.pb.txt` file for the image metadata. Default value: “”.

- `output_dir`: required, the output directory for the extracted keyframes. Default value: “”.

- `num_thread`: optional, number of threads for feature extraction. Default value: 1.

- `frames_metadata_file`: optional, the overridden input metadata file. Default value: “”.


For configuring feature extraction:

- `keypoint_creation_config`: optional, in `KeypointCreationParams` text protobuf. Default value: `configs/keypoint_creation_config.pb.txt`.


For associating synchronized images:

- `extrinsic_timestamp_threshold_in_microseconds`: optional, if the value is greater than 0, specifying the max allowable timestamp difference of two keyframes to be considered the same sample. Default value: 0.

- `extrinsic_translation_distance`: optional, if the value is greater than 0, specifying the max allowable initial guess pose difference of two keyframes to be considered the same sample. Default value: 0.


For sampling images:

- `min_inter_frame_distance`: optional, the translation distance threshold to select the next keyframe. Default value: 0.5 meters.

- `min_inter_frame_rotation_degrees`: optional, the rotation distance threshold to select the next keyframe. Default value: 5 degrees.


For saving debug data:

- `debug_dir`: optional, the output directory for saving debug data. Default value: “”.

- `debug_image_interval`: optional, the number of interval for saving debug images. Default value: 100.


### generate\_bow\_vocabulary\_main [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/isaac_mapping/index.html\#generate-bow-vocabulary-main "Link to this heading")

It supports building vocabulary using `KMeans` or `BIRCH` (Balanced Iterative Reducing and
Clustering using Hierarchies). The recommended way for creating a new vocabulary is to run `KMeans`
clustering, and for incrementally building a vocabulary is to run `BIRCH` clustering + `KMeans` refinement.

An example for creating a new vocabulary with `KMeans` clustering:

```
generate_bow_vocabulary_main \
    --keyframe_directory <input_keyframe_dir> \
    --output_voc_dir <output_voc_dir> \
    --image_retrieval_config configs/image_retrieval_config.pb.txt

```

Copy to clipboard

These are the available options:

- `output_voc_dir`: required, the output directory for saving the vocabulary. Default value: “”.

- `input_voc_dir`: optional, the input directory for loading an existing vocabulary. Default value: “”.

- `keyframe_directory`: required, the input directory for loading keyframe features to build a vocabulary.

- `image_retrieval_config`: required, in `ImageRetrievalConfig` text protobuf. Default value: `configs/image_retrieval_config.pb.txt`.


The binary runs different algorithms depending on the parameters:

- `input_voc_dir` is empty or not specified : create a new vocabulary from keyframe features using `KMeans` clustering.

- `input_voc_dir` is not empty: incrementally build a vocabulary by adding new keyframe features to the existing vocabulary using `BIRCH` clustering.


### generate\_bow\_index\_main [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/isaac_mapping/index.html\#generate-bow-index-main "Link to this heading")

It generates image retrieval index for images in the input directory. It first looks up the closest
visual words in the vocabulary for all features in the image, generates the global descriptor for the
image, and saves it in a loop up table for fast image retrieval.

```
generate_bow_index_main \
    --keyframe_directory <input_keyframe_dir> \
    --output_map_dir <output_map_dir> \
    --image_retrieval_config configs/image_retrieval_config.pb.txt

```

Copy to clipboard

These are the available options:

- `keyframe_directory`: required, the input directory for keyframe features and keyframe metadata file. Default value: “”.

- `output_map_dir`: required, the output directory for saving the image retrieval index. Default value: “”.

- `image_retrieval_config`: required, in `ImageRetrievalConfig` text protobuf. Default value: `configs/image_retrieval_config.pb.txt`.


Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/isaac_mapping/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_mapping_and_localization/isaac_mapping_ros/isaac_mapping/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/index.html)