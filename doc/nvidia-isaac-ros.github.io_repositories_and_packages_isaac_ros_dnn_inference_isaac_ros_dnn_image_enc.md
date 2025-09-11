- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS DNN Inference](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_inference/index.html)
- `isaac_ros_dnn_image_encoder`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_dnn_inference/isaac_ros_dnn_image_encoder/index.rst.txt)

* * *

# `isaac_ros_dnn_image_encoder` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_inference/isaac_ros_dnn_image_encoder/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_dnn_inference/blob/main/isaac_ros_dnn_image_encoder).

Note

This package mainly exists to preserve functionality from prior releases. Instead, we recommend individually including each
node that is required currently. An example can be found in the launch files found in [Isaac ROS RT-DETR](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_object_detection/blob/main/isaac_ros_rtdetr/launch).

## Migration Guide [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_inference/isaac_ros_dnn_image_encoder/index.html\#migration-guide "Link to this heading")

As of Isaac ROS 3.0, the `DnnImageEncoderNode` is deprecated and replaced with a launch file that performs the same functionality
as the original node, with some minor caveats. This decision was made to allow for better composition of nodes that each perform
some preprocessing. For example, for one model, the preprocessing sequence may involve resizing and normalizing an image,
whereas another model may expect only resizing.

This guide explains how to migrate a DNN from our old `DNNImageEncoderNode` to our new launch file.

> 1. Open a launch file where the `DNNImageEncoderNode` is used. We assume that your launch file has code that looks along the lines of:
>
>
>
>
>
> ```
> encoder_node = ComposableNode(
>      name='encoder_node',
>      package='isaac_ros_dnn_image_encoder',
>      plugin='nvidia::isaac_ros::dnn_inference::DnnImageEncoderNode',
>      parameters=[{\
>        'input_image_width': input_image_width,\
>        'input_image_height': input_image_height,\
>        'network_image_width': network_image_width,\
>        'network_image_height': network_image_height,\
>        'image_mean': encoder_image_mean,\
>        'image_stddev': encoder_image_stddev,\
>        'num_blocks': num_blocks,\
>      }],
>      remappings=[\
>        ('encoded_tensor', 'tensor_pub'),\
>        ('image', 'img'),\
>      ],
> )
>
> ```
>
> Copy to clipboard
>
> 2. Import relevant `launch` libraries to allow for including the `dnn_image_encoder.launch.py` launch description:
>
>
>
>
>
> ```
> from ament_index_python.packages import get_package_share_directory
> from launch.actions import IncludeLaunchDescription
> from launch.launch_description_sources import PythonLaunchDescriptionSource
>
> ```
>
> Copy to clipboard
>
> 3. Go to the section where the `encoder_node` is defined and replace it with:
>
>
>
>
>
> ```
> encoder_dir = get_package_share_directory('isaac_ros_dnn_image_encoder')
> encoder_launch = IncludeLaunchDescription(
>      PythonLaunchDescriptionSource(
>        [os.path.join(encoder_dir, 'launch', 'dnn_image_encoder.launch.py')]
>      ),
>      launch_arguments={
>        'input_image_width': input_image_width,
>        'input_image_height': input_image_height,
>        'network_image_width': network_image_width,
>        'network_image_height': network_image_height,
>        'image_mean': encoder_image_mean,
>        'image_stddev': encoder_image_stddev,
>        'attach_to_shared_component_container': 'True',
>        'component_container_name': 'my_container',
>        'dnn_image_encoder_namespace': 'my_encoder_namespace',
>        'image_input_topic': '/image',
>        'camera_info_input_topic': '/camera_info',
>        'tensor_output_topic': '/tensor_pub',
>      }.items(),
> )
>
> ```
>
> Copy to clipboard
>
>
> > The topic names contain a / in front of them
> > to ensure that they are **not** apart of the namespace passed in. Additionally, we configure the encoder to attach
> > to a shared component container called `my_container`. Ensure that the correct global name is passed to the `component_container_name`,
> > otherwise the launch will silently fail.
>
> 4. Finally, launch the included launch description. An example is shown below:
>
>
>
>
>
> ```
> final_launch_container = launch_args + [rclcpp_container, encoder_launch]
> return LaunchDescription(final_launch_container)
>
> ```
>
> Copy to clipboard

Note

The new DNN image encoder expects to subscribe to a CameraInfo topic in addition to an Image topic.

## API [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_inference/isaac_ros_dnn_image_encoder/index.html\#api "Link to this heading")

### `dnn_image_encoder.launch.py` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_inference/isaac_ros_dnn_image_encoder/index.html\#dnn-image-encoder-launch-py "Link to this heading")

#### Launch Arguments [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_inference/isaac_ros_dnn_image_encoder/index.html\#launch-arguments "Link to this heading")

| Launch Argument | Type | Default | Description |
| --- | --- | --- | --- |
| `input_image_width` | `uint16_t` | `0` | The input image width. |
| `input_image_height` | `uint16_t` | `0` | The input image height. |
| `network_image_width` | `uint16_t` | `0` | The image width that the network expects. This will be used to crop the input `image` width. |
| `network_image_height` | `uint16_t` | `0` | The image height that the network expects. This will be used to crop the input `image` height. |
| `image_mean` | `double list` | `[0.5, 0.5, 0.5]` | The mean of the images per channel that will be used for normalization. |
| `image_stddev` | `double list` | `[0.5, 0.5, 0.5]` | The standard deviation of the images per channel that will be used for normalization. |
| `num_blocks` | `int` | `40` | The number of pre-allocated GPU memory blocks |
| `enable_padding` | `bool` | `true` | Whether to enable padding or not |
| `keep_aspect_ratio` | `bool` | `true` | Whether to maintain the aspect ratio or not while resizing |
| `crop_mode` | `string` | `CENTER` | The crop mode to crop the image using |
| `encoding_desired` | `string` | `rgb8` | The desired image format encoding |
| `final_tensor_name` | `string` | `input_tensor` | The tensor name of the output of the image encoder |
| `dnn_image_encoder_namespace` | `string` | `dnn_image_encoder` | The namespace to launch the DNN image encoder under |

#### Launch Configuration Arguments [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_inference/isaac_ros_dnn_image_encoder/index.html\#launch-configuration-arguments "Link to this heading")

| Launch Configuration | Type | Default | Description |
| --- | --- | --- | --- |
| `image_input_topic` | `string` | `image` | The input image topic that the encoder will subscribe to. By default, this will<br>be under the `dnn_image_encoder_namespace` unless if a global topic name is given. |
| `camera_info_input_topic` | `string` | `camera_info` | The input camera\_info topic that the encoder will subscribe to. By default, this will<br>be under the `dnn_image_encoder_namespace` unless if a global topic name is given. |
| `tensor_output_topic` | `string` | `encoded_tensor` | The output tensor topic that the encoder will publish to. By default, this will<br>be under the `dnn_image_encoder_namespace` unless if a global topic name is given. |
| `attach_to_shared_component_container` | `bool` | `false` | Whether to attach to an existing shared container or not |
| `component_container_name` | `string` | `dnn_image_encoder_container` | If `attach_to_shared_component_container` is true, the encoder will attach<br>to a shared container with this name. Otherwise, it will launch a container<br>with this name. |

#### Included Nodes [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_inference/isaac_ros_dnn_image_encoder/index.html\#included-nodes "Link to this heading")

| Component Name |
| --- |
| [ResizeNode](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html) |
| [CropNode](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html) |
| [ImageFormatConverterNode](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html) |
| [ImageToTensorNode](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_inference/isaac_ros_tensor_proc/index.html) |
| [ImageTensorNormalizeNode](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_inference/isaac_ros_tensor_proc/index.html) |
| [InterleavedToPlanarNode](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_inference/isaac_ros_tensor_proc/index.html) |
| [ReshapeNode](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_inference/isaac_ros_tensor_proc/index.html) |

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_dnn_inference/isaac_ros_dnn_image_encoder/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_inference/isaac_ros_dnn_image_encoder/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_dnn_inference/isaac_ros_dnn_image_encoder/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_dnn_inference/isaac_ros_dnn_image_encoder/index.html)