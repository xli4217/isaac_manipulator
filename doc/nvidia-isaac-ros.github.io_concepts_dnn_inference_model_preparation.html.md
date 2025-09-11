- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Concepts](https://nvidia-isaac-ros.github.io/concepts/index.html)
- [DNN Inference](https://nvidia-isaac-ros.github.io/concepts/dnn_inference/index.html)
- Preparing Deep Learning Models for Isaac ROS
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/dnn_inference/model_preparation.rst.txt)

* * *

# Preparing Deep Learning Models for Isaac ROS [](https://nvidia-isaac-ros.github.io/concepts/dnn_inference/model_preparation.html\#preparing-deep-learning-models-for-isaac-ros "Link to this heading")

## Obtaining a Pre-trained Model from NGC [](https://nvidia-isaac-ros.github.io/concepts/dnn_inference/model_preparation.html\#obtaining-a-pre-trained-model-from-ngc "Link to this heading")

The NVIDIA GPU Cloud hosts a
[catalog](https://catalog.ngc.nvidia.com/models) of Deep Learning
pre-trained models that are available for your development.

1. Use the **Search Bar** to find a pre-trained model that you are
interested in working with.

2. Click on the model’s card to view an expanded description, and then
click on the **File Browser** tab along the navigation bar.

3. Using the **File Browser**, find a deployable `.onnx` file for the
model you are interested in.

4. Under the **Actions** heading, click on the **…** icon for the file
you selected in the previous step, and then click **Copy** `wget` **command**.

5. **Paste** the copied command into a terminal to download the model in
the current working directory.


## Using `trtexec` to convert an ONNX Model ( `.onnx`) to a TensorRT Plan File ( `.plan`) [](https://nvidia-isaac-ros.github.io/concepts/dnn_inference/model_preparation.html\#using-trtexec-to-convert-an-onnx-model-onnx-to-a-tensorrt-plan-file-plan "Link to this heading")

`trtexec` is already included in the Docker images available as
part of the standard [Isaac ROS Development Environment](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html).
The version of `trtexec` matches the version of TensorRT installed from the same Docker image.

The per-platform installation paths are described below:

| Platform | Installation Path | Symlink Path |
| --- | --- | --- |
| `x86_64` | `/usr/src/tensorrt/bin/trtexec` | `/usr/src/tensorrt/bin/trtexec` |
| Jetson ( `aarch64`) | `/usr/src/tensorrt/bin/trtexec` | `/usr/src/tensorrt/bin/trtexec` |

Warning

Reading the documentation of [trtexec](https://docs.nvidia.com/deeplearning/tensorrt/developer-guide/index.html#trtexec) is highly recommended to
obtain best performance. In particular, we recommend pay attention to the quantization of the model (e.g. `fp32` vs `fp16` vs `int8`).

### Converting `.onnx` to a TensorRT Engine Plan [](https://nvidia-isaac-ros.github.io/concepts/dnn_inference/model_preparation.html\#converting-onnx-to-a-tensorrt-engine-plan "Link to this heading")

Here are some examples for generating the TensorRT engine file using
`trtexec`. In this example, we will use the
[PeopleSemSegnet Shuffleseg model](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/tao/models/peoplesemsegnet/files?version=deployable_shuffleseg_unet_onnx_v1.0):

#### Generate an engine file for the `fp16` data type [](https://nvidia-isaac-ros.github.io/concepts/dnn_inference/model_preparation.html\#generate-an-engine-file-for-the-fp16-data-type "Link to this heading")

```
mkdir -p /workspaces/isaac_ros-dev/models && \
   /usr/src/tensorrt/bin/trtexec --onnx=/workspaces/isaac_ros-dev/models/peoplesemsegnet_shuffleseg.onnx --saveEngine=/workspaces/isaac_ros-dev/models/peoplesemsegnet_shuffleseg.engine --fp16 --minShapes=input_2:0:1x3x544x960 --optShapes=input_2:0:1x3x544x960 --maxShapes=input_2:0:1x3x544x960

```

Copy to clipboard

Note

The input ONNX file is specified using the `--onnx` option. The output file is specified
using the `--saveEngine` option. The specific values used in the command above are retrieved
from the **PeopleSemSegnet** page under the **Overview** tab. The
model input node name can be found in
`peoplesemsegnet_shuffleseg_cache.txt` from `File Browser`. The tool needs
write permission to the output directory.

A detailed explanation of the input parameters is available
[here](https://docs.nvidia.com/deeplearning/tensorrt/developer-guide/index.html#trtexec).

#### Generate an engine file for the data type `int8` [](https://nvidia-isaac-ros.github.io/concepts/dnn_inference/model_preparation.html\#generate-an-engine-file-for-the-data-type-int8 "Link to this heading")

In this example, we will use the
[PeopleNet model](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/tao/models/peoplenet/files?version=deployable_quantized_onnx_v2.6.3):

Create the models directory:

```
mkdir -p /workspaces/isaac_ros-dev/models

```

Copy to clipboard

Download the calibration cache file:

Note

Check the model’s page on NGC for the latest `wget`
command.

```
wget --content-disposition 'https://api.ngc.nvidia.com/v2/models/org/nvidia/team/tao/peoplenet/deployable_quantized_onnx_v2.6.3/files?redirect=true&path=resnet34_peoplenet.onnx' -O /workspaces/isaac_ros-dev/models/resnet34_peoplenet.onnx && \
wget --content-disposition 'https://api.ngc.nvidia.com/v2/models/org/nvidia/team/tao/peoplenet/deployable_quantized_onnx_v2.6.3/files?redirect=true&path=resnet34_peoplenet_int8.txt' -O /workspaces/isaac_ros-dev/models/resnet34_peoplenet_int8.txt

```

Copy to clipboard

```
/usr/src/tensorrt/bin/trtexec --onnx=/workspaces/isaac_ros-dev/models/resnet34_peoplenet.onnx --saveEngine=/workspaces/isaac_ros-dev/models/resnet34_peoplenet.plan --int8 --calib=/workspaces/isaac_ros-dev/models/resnet34_peoplenet_int8.txt --minShapes=input_1:0:1x3x544x960 --optShapes=input_1:0:1x3x544x960 --maxShapes=input_1:0:1x3x544x960

```

Copy to clipboard

Note

The calibration cache file (specified using the `--calib`
option) is required to generate the `int8` engine file. This file
is provided in the **File Browser** tab of the model’s page on NGC.

## Inspecting The Input and Output Binding Names of a Model [](https://nvidia-isaac-ros.github.io/concepts/dnn_inference/model_preparation.html\#inspecting-the-input-and-output-binding-names-of-a-model "Link to this heading")

Deep learning models have `input_binding_names` and `output_binding_names`. These correspond to the model’s inputs and outputs
respectively. These are determined by the model itself during export. To determine this, [netron](https://netron.app/) can be used to visualize the ONNX model.

Note

In addition, the `TensorRTNode` and `TritonNode` have parameters called `input_tensor_names` and `output_tensor_names`,
these correspond to the expected tensor names within the ROS 2 `TensorList`.

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/dnn_inference/model_preparation.html)[latest](https://nvidia-isaac-ros.github.io/concepts/dnn_inference/model_preparation.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/concepts/dnn_inference/model_preparation.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/dnn_inference/model_preparation.html)