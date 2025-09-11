- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS Image Segmentation](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/index.html)
- `isaac_ros_segformer`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.rst.txt)

* * *

# `isaac_ros_segformer` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_image_segmentation/blob/main/isaac_ros_segformer).

## Quickstart [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html\#quickstart "Link to this heading")

### Set Up Development Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html\#set-up-development-environment "Link to this heading")

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


### Download Quickstart Assets [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html\#download-quickstart-assets "Link to this heading")

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
PACKAGE_NAME="isaac_ros_segformer"
NGC_RESOURCE="isaac_ros_segformer_assets"
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


### Build `isaac_ros_segformer` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html\#build-package-name "Link to this heading")

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
sudo apt-get install -y ros-humble-isaac-ros-segformer

```

Copy to clipboard


### Prepare PeopleSemSegFormer Model [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html\#prepare-peoplesemsegformer-model "Link to this heading")

1. Open a new terminal and attach to the container.





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
     ./scripts/run_dev.sh

```

Copy to clipboard

2. Download the `PeopleSemSegFormer` ONNX file:


> ```
> mkdir -p ${ISAAC_ROS_WS}/isaac_ros_assets/models/peoplesemsegformer/1 && \
>  cd ${ISAAC_ROS_WS}/isaac_ros_assets/models/peoplesemsegformer/1 && \
>  wget --content-disposition 'https://api.ngc.nvidia.com/v2/models/org/nvidia/team/tao/peoplesemsegformer/deployable_v1.0/files?redirect=true&path=peoplesemsegformer.onnx' -O model.onnx
>
> ```
>
> Copy to clipboard

3. Convert the ONNX file to a TensorRT plan file:





```
/usr/src/tensorrt/bin/trtexec --onnx=${ISAAC_ROS_WS}/isaac_ros_assets/models/peoplesemsegformer/1/model.onnx --saveEngine=${ISAAC_ROS_WS}/isaac_ros_assets/models/peoplesemsegformer/1/model.plan

```

Copy to clipboard





Note



The model conversion time varies across different platforms. On Jetson AGX Orin, the engine conversion process takes ~10-15 minutes to complete.

4. Create a file called `/tmp/models/peoplesemsegformer/config.pbtxt` by copying the sample config file:


> ```
> cp ${ISAAC_ROS_WS}/isaac_ros_assets/isaac_ros_segformer/peoplesemsegformer_config.pbtxt ${ISAAC_ROS_WS}/isaac_ros_assets/models/peoplesemsegformer/config.pbtxt
>
> ```
>
> Copy to clipboard


### Run Launch File [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html\#run-launch-file "Link to this heading")

RosbagRealSense CameraHawk CameraZED Camera

1. Continuing inside the Docker container, install the following dependencies:


> ```
> sudo apt-get update
>
> ```
>
> Copy to clipboard






```
sudo apt-get install -y ros-humble-isaac-ros-examples

```

Copy to clipboard

2. Run the following launch file to spin up a demo of this package using the quickstart rosbag:





```
ros2 launch isaac_ros_examples isaac_ros_examples.launch.py launch_fragments:=segformer interface_specs_file:=${ISAAC_ROS_WS}/isaac_ros_assets/isaac_ros_segformer/quickstart_interface_specs.json model_name:=peoplesemsegformer model_repository_paths:=[${ISAAC_ROS_WS}/isaac_ros_assets/models]

```

Copy to clipboard

3. Open **another** terminal and play the rosbag:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
      ./scripts/run_dev.sh

```

Copy to clipboard







```
ros2 bag play -l isaac_ros_assets/isaac_ros_segformer/segformer_sample_data

```

Copy to clipboard


### Visualize Results [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html\#visualize-results "Link to this heading")

1. Open a **new** terminal inside the Docker container:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
      ./scripts/run_dev.sh

```

Copy to clipboard

2. Visualize and validate the output of the package by launching `rqt_image_view`:





```
ros2 run rqt_image_view rqt_image_view /segformer/colored_segmentation_mask

```

Copy to clipboard


[![https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/peoplesemsegformer_output_rqt.png/](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/peoplesemsegformer_output_rqt.png/)](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/peoplesemsegformer_output_rqt.png/)


Note



The raw segmentation mask is also published to `/segformer/raw_segmentation_mask`. However, the raw pixels correspond to the class labels and so the output is unsuitable for human visual inspection.


## Try More Examples [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html\#try-more-examples "Link to this heading")

To continue your exploration, check out the following suggested examples:

- [Tutorial with Isaac Sim](https://nvidia-isaac-ros.github.io/concepts/segmentation/segformer/tutorial_isaac_sim.html)
- [Tutorial with TensorRT](https://nvidia-isaac-ros.github.io/concepts/segmentation/segformer/tutorial_tensorrt.html)

## Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html\#troubleshooting "Link to this heading")

### Isaac ROS Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html\#isaac-ros-troubleshooting "Link to this heading")

For solutions to problems with Isaac ROS, please check [here](https://nvidia-isaac-ros.github.io/troubleshooting/index.html).

### Deep Learning Troubleshooting [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html\#deep-learning-troubleshooting "Link to this heading")

For solutions to problems with using DNN models, please check [here](https://nvidia-isaac-ros.github.io/troubleshooting/deep_learning.html).

## API [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html\#api "Link to this heading")

### Usage [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html\#usage "Link to this heading")

Two launch files are provided to use this package. The first launch file launches `isaac_ros_tensor_rt`, whereas another one uses `isaac_ros_triton`, along with
the necessary components to perform encoding on images and decoding of `Segformer`’s output. Please note, Segformer re-utilizes U-Net decoder for decoding the network output.

Warning

For your specific application, these launch files may need to be modified. Please consult the available components to see
the configurable parameters.

| Launch File | Components Used |
| --- | --- |
| `isaac_ros_people_sem_segformer_tensor_rt.launch.py` | [DnnImageEncoderNode](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_inference/isaac_ros_dnn_image_encoder/index.html), [TensorRTNode](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_inference/isaac_ros_tensor_rt/index.html), [UNetDecoderNode](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_unet/index.html) |
| `isaac_ros_people_sem_segformer_triton.launch.py` | [DnnImageEncoderNode](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_inference/isaac_ros_dnn_image_encoder/index.html), [TritonNode](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_inference/isaac_ros_triton/index.html), [UNetDecoderNode](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_unet/index.html) |

Note

Isaac ROS Segformer uses UNetDecoderNode for postprocessing and doesn’t have any nodes of its own. Refer [Isaac ROS UNet Package](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_unet/index.html) for more details.

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_image_segmentation/isaac_ros_segformer/index.html)