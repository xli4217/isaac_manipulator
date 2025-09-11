- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Image Pipeline](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/index.html)
- `isaac_ros_image_proc`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.rst.txt)

* * *

# `isaac_ros_image_proc` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_image_pipeline/blob/main/isaac_ros_image_proc).

![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/image_proc/image_proc/image_proc.gif/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/image_proc/image_proc/image_proc.gif/)

## Quickstart [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#quickstart "Link to this heading")

### Set Up Development Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#set-up-development-environment "Link to this heading")

1. Set up your development environment by following the instructions in [getting started](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html).

2. Clone `isaac_ros_common` under `${ISAAC_ROS_WS}/src`.





```
cd ${ISAAC_ROS_WS}/src && \
      git clone -b release-3.2 https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common.git isaac_ros_common

```

Copy to clipboard

3. (Optional) Install dependencies for any sensors you want to use by following the [sensor-specific guides](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/index.html).



Note



We strongly recommend installing all sensor dependencies **before** starting any quickstarts.
Some sensor dependencies require restarting the Isaac ROS Dev container during installation, which will interrupt the quickstart process.


### Datasets [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#datasets "Link to this heading")

#### Download Quickstart Assets [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#download-quickstart-assets "Link to this heading")

1. Download quickstart data from NGC:

Make sure required libraries are installed.





```
sudo apt-get install -y curl jq tar

```

Copy to clipboard



Then, run these commands to download the asset from NGC:





```
NGC_ORG="nvidia"
NGC_TEAM="isaac"
PACKAGE_NAME="isaac_ros_image_proc"
NGC_RESOURCE="isaac_ros_image_proc_assets"
NGC_FILENAME="quickstart.tar.gz"
MAJOR_VERSION=3
MINOR_VERSION=2
VERSION_REQ_URL="https://catalog.ngc.nvidia.com/api/resources/versions?orgName=$NGC_ORG&teamName=$NGC_TEAM&name=$NGC_RESOURCE&isPublic=true&pageNumber=0&pageSize=100&sortOrder=CREATED_DATE_DESC"
AVAILABLE_VERSIONS=$(curl -s \
       -H "Accept: application/json" "$VERSION_REQ_URL")
LATEST_VERSION_ID=$(echo $AVAILABLE_VERSIONS | jq -r "
       .recipeVersions[]
       | .versionId as \$v
       | \$v | select(test(\"^\\\\d+\\\\.\\\\d+\\\\.\\\\d+$\"))
       | split(\".\") | {major: .[0]|tonumber, minor: .[1]|tonumber, patch: .[2]|tonumber}
       | select(.major == $MAJOR_VERSION and .minor <= $MINOR_VERSION)
       | \$v
       " | sort -V | tail -n 1
)
if [ -z "$LATEST_VERSION_ID" ]; then
       echo "No corresponding version found for Isaac ROS $MAJOR_VERSION.$MINOR_VERSION"
       echo "Found versions:"
       echo $AVAILABLE_VERSIONS | jq -r '.recipeVersions[].versionId'
else
       mkdir -p ${ISAAC_ROS_WS}/isaac_ros_assets && \
       FILE_REQ_URL="https://api.ngc.nvidia.com/v2/resources/$NGC_ORG/$NGC_TEAM/$NGC_RESOURCE/\
versions/$LATEST_VERSION_ID/files/$NGC_FILENAME" && \
       curl -LO --request GET "${FILE_REQ_URL}" && \
       tar -xf ${NGC_FILENAME} -C ${ISAAC_ROS_WS}/isaac_ros_assets && \
       rm ${NGC_FILENAME}
fi

```

Copy to clipboard


### Build `isaac_ros_image_proc` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#build-package-name "Link to this heading")

Binary PackageBuild from Source

1. Launch the Docker container using the `run_dev.sh` script:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
./scripts/run_dev.sh

```

Copy to clipboard

2. Install the prebuilt Debian package:


> ```
> sudo apt-get update
>
> ```
>
> Copy to clipboard






```
sudo apt-get install -y ros-humble-isaac-ros-image-proc

```

Copy to clipboard


### Run Launch File [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#run-launch-file "Link to this heading")

Note

The quickstart for the alpha blending node has been integrated into `isaac_ros_unet` image segmentation.
To run and visualize the results of alpha blending, see [here](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_unet/index.html).

RosbagRealSense CameraHawk CameraZED Camera

1. Continuing inside the Docker container, install the following dependencies:


> > ```
> > sudo apt-get update
> >
> > ```
> >
> > Copy to clipboard
>
> ```
> sudo apt-get install -y ros-humble-isaac-ros-examples
>
> ```
>
> Copy to clipboard


2. Run the following launch file to spin up a demo of this package using the quickstart rosbag:
Resize:





```
ros2 launch isaac_ros_examples isaac_ros_examples.launch.py launch_fragments:=resize

```

Copy to clipboard



Color Conversion:





```
ros2 launch isaac_ros_examples isaac_ros_examples.launch.py launch_fragments:=color_conversion interface_specs_file:=${ISAAC_ROS_WS}/isaac_ros_assets/isaac_ros_image_proc/quickstart_interface_specs.json

```

Copy to clipboard



Crop:





```
ros2 launch isaac_ros_examples isaac_ros_examples.launch.py launch_fragments:=crop interface_specs_file:=${ISAAC_ROS_WS}/isaac_ros_assets/isaac_ros_image_proc/quickstart_interface_specs.json

```

Copy to clipboard



Rectify:





```
ros2 launch isaac_ros_examples isaac_ros_examples.launch.py launch_fragments:=rectify_mono interface_specs_file:=${ISAAC_ROS_WS}/isaac_ros_assets/isaac_ros_image_proc/quickstart_interface_specs.json

```

Copy to clipboard



Flip:





```
ros2 launch isaac_ros_examples isaac_ros_examples.launch.py launch_fragments:=flip

```

Copy to clipboard


2. Open a **second** terminal inside the Docker container:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
./scripts/run_dev.sh

```

Copy to clipboard

3. Run the rosbag file to simulate an image stream:





```
ros2 bag play --loop ${ISAAC_ROS_WS}/isaac_ros_assets/isaac_ros_image_proc/quickstart --remap /hawk_0_left_rgb_image:=/image_raw /hawk_0_left_rgb_camera_info:=/camera_info

```

Copy to clipboard


Note

For RealSense camera package issues, please refer to the section
[here](https://nvidia-isaac-ros.github.io/troubleshooting/hardware_setup.html#realsense-driver-does-not-work-with-ros-2-humble).

Other supported cameras can be found
[here](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_argus_camera/index.html#reference-cameras).

For camera calibration, please refer to
[this guide](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/camera_calibration.html).

### Visualize Results [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#visualize-results "Link to this heading")

1. Open a **new** terminal inside the Docker container:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
      ./scripts/run_dev.sh

```

Copy to clipboard

2. Observe the image\_proc output on the terminal with the command:
Resize:





```
ros2 run image_view image_view --ros-args --remap image:=resize/image

```

Copy to clipboard



Color Conversion:





```
ros2 run image_view image_view --ros-args --remap image:=image_mono

```

Copy to clipboard



Crop:





```
ros2 run image_view image_view --ros-args --remap image:=crop/image

```

Copy to clipboard



Rectify:





```
ros2 run image_view image_view --ros-args --remap image:=image_rect

```

Copy to clipboard



Flip:





```
ros2 run image_view image_view --ros-args --remap image:=image_flipped

```

Copy to clipboard


### Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#overview "Link to this heading")

The `isaac_ros_image_proc` package offers functionality for
rectifying/undistorting images from a monocular camera setup, resizing
the image, changing the image format, and alpha blending two images. It largely replaces the
`image_proc` package, though the image format conversion facility also
functions as a way to replace the CPU-based image format conversion in
`cv_bridge`. The rectify node can also resize the image; if resizing
is not needed, specify the output width/height same as input.

### Available Components [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#available-components "Link to this heading")

#### AlphaBlendNode [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#alphablendnode "Link to this heading")

### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#ros-parameters "Link to this heading")

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `alpha` | `double` | `0.5` | The alpha value, where 0 represents the image input being fully transparent and the mask input fully opaque.<br>This is a mandatory parameter and needs to be between 0 and 1 inclusive. |
| `image_queue_size` | `int` | `10` | The queue size of the image input. This is a mandatory parameter and needs to be set to a non-zero positive number. |
| `mask_queue_size` | `int` | `10` | The queue size of the mask input. This is a mandatory parameter and needs to be set to a non-zero positive number. |
| `sync_queue_size` | `int` | `10` | The queue size of the time synchronizer. This is a mandatory parameter and needs to be set to a non-zero positive number. |

### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#ros-topics-subscribed "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `image_input` | [NitrosImage](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_image_type/include/isaac_ros_nitros_image_type/nitros_image.hpp) | The input image to blend. |
| `mask_input` | [NitrosImage](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_image_type/include/isaac_ros_nitros_image_type/nitros_image.hpp) | The mask image to blend. |

#### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#ros-topics-published "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `blended_image` | [NitrosImage](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_image_type/include/isaac_ros_nitros_image_type/nitros_image.hpp) | Blended image. |

#### CropNode [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#cropnode "Link to this heading")

### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id1 "Link to this heading")

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `input_width` | `int64_t` | `0` | The width of the input image. This is a mandatory parameter and needs to be set to a non-zero positive number. |
| `input_height` | `int64_t` | `0` | The height of the input image. This is a mandatory parameter and needs to be set to a non-zero positive number. |
| `crop_width` | `int64_t` | `0` | The width of the output image. This is a mandatory parameter and needs to be set to a non-zero positive number. |
| `crop_height` | `int64_t` | `0` | The height of the output image. This is a mandatory parameter and needs to be set to a non-zero positive number. |
| `crop_mode` | `string` | `""` | Mode of operation to perform cropping. Valid values are - `CENTER`, `LEFT`, `RIGHT`, `TOP`, `BOTTOM`, `TOPLEFT`, `TOPRIGHT`, `BOTTOMLEFT`, `BOTTOMRIGHT`, and `BBOX`. Except `BBOX`, region of interest calculation is performed based on input and cropped image dimensions. |
| `roi_top_left_x` | `int64_t` | `0` | X value of the top left corner of a region of interest. This is only used when `crop_mode` is set to `BBOX`. |
| `roi_top_left_y` | `int64_t` | `0` | Y value of the top left corner of a region of interest. This is only used when `crop_mode` is set to `BBOX`. |
| `num_blocks` | `int` | `40` | The number of pre-allocated memory output blocks, should not be less than `40`. |

### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id2 "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `image` | [NitrosImage](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_image_type/include/isaac_ros_nitros_image_type/nitros_image.hpp) | The input image that needs to be cropped. |
| `camera_info` | [NitrosCameraInfo](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_camera_info_type/include/isaac_ros_nitros_camera_info_type/nitros_camera_info.hpp) | The corresponding camera\_info of the input image. |

#### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id3 "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `crop/image` | [NitrosImage](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_image_type/include/isaac_ros_nitros_image_type/nitros_image.hpp) | Cropped image. |
| `crop/camera_info` | [NitrosCameraInfo](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_camera_info_type/include/isaac_ros_nitros_camera_info_type/nitros_camera_info.hpp) | The corresponding camera\_info of the cropped image. |

#### ImageFlipNode [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#imageflipnode "Link to this heading")

### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id4 "Link to this heading")

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `flip_mode` | `string` | `BOTH` | Axis to be use for flip operation. Valid values are - `HORIZONTAL`, `VERTICAL`, and `BOTH`. |

### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id5 "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `image` | [NitrosImage](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_image_type/include/isaac_ros_nitros_image_type/nitros_image.hpp) | The input image that needs to be flipped. |

#### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id6 "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `image_flipped` | [NitrosImage](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_image_type/include/isaac_ros_nitros_image_type/nitros_image.hpp) | Flipped image. |

#### ImageFormatConverterNode [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#imageformatconverternode "Link to this heading")

### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id7 "Link to this heading")

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `image_width` | `int64_t` | `1280` | The width of the input image. This is a mandatory parameter and needs to be set to a non-zero positive number. |
| `image_height` | `int64_t` | `720` | The height of the input image. This is a mandatory parameter and needs to be set to a non-zero positive number. |
| `encoding_desired` | `string` | `""` | Target color encoding to convert to. Valid values are - `img_encodings::RGB8`, `img_encodings::RGB16`, `img_encodings::BGR8`, `img_encodings::BGR16`, `img_encodings::MONO8`, `img_encodings::MONO16`, `img_encodings::NV24`, and `nv12`. |
| `num_blocks` | `int` | `40` | The number of pre-allocated memory output blocks, should not be less than `40`. |

### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id8 "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `image_raw` | [NitrosImage](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_image_type/include/isaac_ros_nitros_image_type/nitros_image.hpp) | The input image data. |

#### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id9 "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `image` | [NitrosImage](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_image_type/include/isaac_ros_nitros_image_type/nitros_image.hpp) | The output image data. |

#### ImageNormalizeNode [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#imagenormalizenode "Link to this heading")

### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id10 "Link to this heading")

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `mean` | `std::vector<double>` | `{0.5, 0.5, 0.5}` | The mean of the image pixels per channel that will be used for normalization. |
| `stddev` | `std::vector<double>` | `{0.5, 0.5, 0.5}` | The standard deviation of the image pixels per channel that will be used for normalization. |

### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id11 "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `image` | [NitrosImage](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_image_type/include/isaac_ros_nitros_image_type/nitros_image.hpp) | The input image data. |

#### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id12 "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `normalized_image` | [NitrosImage](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_image_type/include/isaac_ros_nitros_image_type/nitros_image.hpp) | The output image data. |

#### PadNode [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#padnode "Link to this heading")

### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id13 "Link to this heading")

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `output_image_width` | `uint16_t` | `1200` | The width of the output image. This is a mandatory parameter and needs to be set to a non-zero positive number. |
| `output_image_height` | `uint16_t` | `1024` | The height of the output image. This is a mandatory parameter and needs to be set to a non-zero positive number. |
| `padding_type` | `string` | `CENTER` | Describes the padding mode. Valid values are - `CENTER`, `TOP_LEFT`, `TOP_RIGHT`, `BOTTOM_LEFT`, and `BOTTOM_RIGHT`. |
| `border_type` | `string` | `CONSTANT` | Describes how border values are calculated. Valid values are - `CONSTANT`, `REPLICATE`, `REFLECT`, `WRAP`, and `REFLECT101`. More info can be found [here](https://cvcuda.github.io/CV-CUDA/_python_api/_cvcuda_api/_aux_border.html). |
| `border_pixel_color_value` | `std::vector<double>` | `{0.0, 0.0, 0.0, 0.0}` | RGBA color value for border color. |

### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id14 "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `image` | [NitrosImage](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_image_type/include/isaac_ros_nitros_image_type/nitros_image.hpp) | The input image data. |

#### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id15 "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `padded_image` | [NitrosImage](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_image_type/include/isaac_ros_nitros_image_type/nitros_image.hpp) | The output image data. |

#### RectifyNode [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#rectifynode "Link to this heading")

### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id16 "Link to this heading")

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `output_width` | `uint16_t` | `1200` | The width of the output image. This is a mandatory parameter and needs to be set to a non-zero positive number. |
| `output_height` | `uint16_t` | `1024` | The height of the output image. This is a mandatory parameter and needs to be set to a non-zero positive number. |
| `horizontal_interval` | `uint16_t` | `1` | The horizontal interval used for generating rectify warp grid points. It must be power-of-two and must be greater than or equal to 1. |
| `vertical_interval` | `uint16_t` | `1` | The vertical interval used for generating rectify warp grid points. It must be power-of-two and must be greater than or equal to 1. |
| `interpolation` | `string` | `cubic_catmullrom` | The interpolation type. Choices are: `nearest`, `linear`, `cubic_catmullrom`. |
| `border_type` | `string` | `zero` | The boarder type specifies how the pixel values outside of the image should be constructed.<br>Choices are: `zero`, `repeat`, `reverse`, `mirror`. |
| `num_blocks` | `int` | `40` | The number of pre-allocated memory output blocks, should not be less than `40`. |

### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id17 "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `image` | [NitrosImage](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_image_type/include/isaac_ros_nitros_image_type/nitros_image.hpp) | The input image that needs to be rectified. |
| `camera_info` | [NitrosCameraInfo](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_camera_info_type/include/isaac_ros_nitros_camera_info_type/nitros_camera_info.hpp) | The corresponding camera\_info of the input image. |

#### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id18 "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `crop/image` | [NitrosImage](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_image_type/include/isaac_ros_nitros_image_type/nitros_image.hpp) | Rectified image. |
| `crop/camera_info` | [NitrosCameraInfo](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_camera_info_type/include/isaac_ros_nitros_camera_info_type/nitros_camera_info.hpp) | The corresponding camera\_info of the rectified image. |

#### ResizeNode [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#resizenode "Link to this heading")

### ROS Parameters [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id19 "Link to this heading")

| ROS Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `input_width` | `int64_t` | `0` | The width of the input image. This is a mandatory parameter and needs to be set to a non-zero positive number. |
| `input_height` | `int64_t` | `0` | The height of the input image. This is a mandatory parameter and needs to be set to a non-zero positive number. |
| `output_width` | `int64_t` | `1080` | The width of the output image. This is a mandatory parameter and needs to be set to a non-zero positive number. |
| `output_height` | `int64_t` | `720` | The height of the output image. This is a mandatory parameter and needs to be set to a non-zero positive number. |
| `keep_aspect_ratio` | `bool` | `false` | When enabled, aspect ratio of the original image will be preserved by adding black border. |
| `encoding_desired` | `string` | `""` | This value is used to mark the preference of output color format. |
| `disable_padding` | `bool` | `false` | When set to true, `keep_aspect_ratio` is ignored and no padding is added to the output image. |
| `num_blocks` | `int` | `40` | The number of pre-allocated memory output blocks, should not be less than `40`. |

### ROS Topics Subscribed [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id20 "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `image` | [NitrosImage](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_image_type/include/isaac_ros_nitros_image_type/nitros_image.hpp) | The input image that needs to be resized. |
| `camera_info` | [NitrosCameraInfo](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_camera_info_type/include/isaac_ros_nitros_camera_info_type/nitros_camera_info.hpp) | The corresponding camera\_info of the input image. |

#### ROS Topics Published [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html\#id21 "Link to this heading")

| ROS Topic | Interface | Description |
| --- | --- | --- |
| `resize/image` | [NitrosImage](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_image_type/include/isaac_ros_nitros_image_type/nitros_image.hpp) | Resized image. |
| `resize/camera_info` | [NitrosCameraInfo](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_nitros_type/isaac_ros_nitros_camera_info_type/include/isaac_ros_nitros_camera_info_type/nitros_camera_info.hpp) | The corresponding camera\_info of the resized image. |

**Limitation**: `isaac_ros_image_proc` nodes require even number dimensions for
images.

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_image_pipeline/isaac_ros_image_proc/index.html)