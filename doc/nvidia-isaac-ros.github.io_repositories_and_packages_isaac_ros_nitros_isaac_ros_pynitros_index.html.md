- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- [Isaac ROS NITROS](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/index.html)
- `isaac_ros_pynitros`
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.rst.txt)

* * *

# `isaac_ros_pynitros` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html\#package-name "Link to this heading")

Source code on [GitHub](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros/blob/main/isaac_ros_pynitros).

## Creating PyNITROS-Accelerated Nodes [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html\#creating-pynitros-accelerated-nodes "Link to this heading")

The following steps outline how to create PyNITROS-accelerated nodes using the PyNITROS API. This example
demonstrates how to use PyNITROS to publish and subscribe image messages. PyNITROS also supports tensor list messages,
which can be used in a similar manner. You can find the source code of this example under the `isaac_ros_pynitros/examples` directory.

1. Instantiate the `PyNitrosSubscriber`. Developers can instantiate the `PyNitrosSubscriber` class with the following
five parameters:


   - ROS node

   - NITROS Bridge message type

   - NITROS Bridge message topic name

   - raw ROS message topic name

   - enable ROS subscribe flag


After instantiation, use the `create_subscription` function to create a subscription with a user-defined callback function.

```
pynitros_subscriber = PyNitrosSubscriber(Node, NitrosBridgeImage, "pynitros_image", "pynitros_image_raw", enable_ros_subscribe=False)

```

Copy to clipboard

```
pynitros_subscriber.create_subscription(sub_callback)

```

Copy to clipboard

2. Create the `PyNitrosPublisher` by specifying ROS node, NITROS Bridge message type, NITROS Bridge topic name, and raw ROS topic name respectively.





```
pynitros_publisher = PyNitrosPublisher(Node, NitrosBridgeImage, "pynitros_image", "pynitros_image_raw")

```

Copy to clipboard

3. Create the PyNITROS image builder. This builder will create a memory pool
with a number of buffers to hold the IPC GPU data of messages pending to publish.
To instantiate the builder, you need to specify the number of buffers and timeout
for reclaiming the buffer back to the pool.





```
pynitros_image_builder = PyNitrosImageBuilder(num_buffer (optional), timeout (optional))

```

Copy to clipboard

4. When a subscribed message arrives, the callback function is triggered with a `PyNITROSView` of the message.
You can convert this view to another data type of your choice, such as a PyTorch tensor,
by passing in `pynitros_image_view` into the `torch.as_tensor()` function:





```
def sub_callback(pynitros_image_view):
      # (Optional) If user has async GPU operations in the fly, pass stream back to the view
      stream = cudart.cudaStreamCreate()
      pynitros_image_view.set_stream(stream)

      # Get data as tensor, or any other data type supports __cuda_array_interface__
      image_tensor = torch.as_tensor(pynitros_image_view, dtype=torch.uint8, device=gpu)

```

Copy to clipboard

5. After processing the received image, pass the device pointer of the output and metadata
into the `PyNitrosBuilder` build function. This generates a `NitrosBridgeImage`
that can then be published using the `PyNitrosPublisher`. If the `enable_ros_publish` flag is
set to `True`, the publisher also sends the raw image (that is, an `Image` message)
to the second topic specified during instantiation.





```
built_image = pynitros_image_builder.build(image_data_ptr,
                                              image_height,
                                              image_width,
                                              image_step,
                                              image_encoding,
                                              image_header,
                                              device_id=0,
                                              enable_ros_publish=False)

```

Copy to clipboard

6. Publish the built image using the publisher:





```
pynitros_publisher.publish(built_image)

```

Copy to clipboard


Note

PyNITROS requires the permission of `PTRACE_ATTACH` for IPC. You can
set the `ptrace_scope` by running the following command:

```
echo 0 | sudo tee /proc/sys/kernel/yama/ptrace_scope

```

Copy to clipboard

## Quickstart [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html\#quickstart "Link to this heading")

This quickstart uses the `image_forward` node built with PyNITROS to demonstrate the zero-copy inter-op between PyNITROS nodes.

### Set Up Development Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html\#set-up-development-environment "Link to this heading")

1. Set up your development environment by following the instructions in [getting started](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html).

2. Clone `isaac_ros_common` under `${ISAAC_ROS_WS}/src`.





```
cd ${ISAAC_ROS_WS}/src && \
      git clone -b release-3.2 https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common.git isaac_ros_common

```

Copy to clipboard


#### Download Quickstart Assets [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html\#download-quickstart-assets "Link to this heading")

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
PACKAGE_NAME="isaac_ros_pynitros"
NGC_RESOURCE="isaac_ros_pynitros_assets"
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


### Build `isaac_ros_pynitros` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html\#build-isaac-ros-pynitros "Link to this heading")

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
sudo apt-get install -y ros-humble-isaac-ros-pynitros

```

Copy to clipboard


### Run Launch File [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html\#run-launch-file "Link to this heading")

Rosbag

1. Set the `ptrace_scope`:


> ```
> echo 0 | sudo tee /proc/sys/kernel/yama/ptrace_scope
>
> ```
>
> Copy to clipboard

2. Run the following launch files to spin up a demo of this package:





```
cd /workspaces/isaac_ros-dev && \
     ros2 launch isaac_ros_pynitros isaac_ros_pynitros_quickstart.launch.py rosbag_path:=${ISAAC_ROS_WS}/isaac_ros_assets/isaac_ros_pynitros/quickstart.bag

```

Copy to clipboard

3. **Attach a second terminal** to the Docker container:





```
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
     ./scripts/run_dev.sh -d ${HOME}/workspaces

```

Copy to clipboard

4. Visualize the output topic `/pynitros2_output_msg_ros` in RViz:





```
cd /workspaces/isaac_ros-dev/ && \
     rviz2

```

Copy to clipboard


## Try More Examples [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html\#try-more-examples "Link to this heading")

To continue your exploration, explore the following examples:

- [Tutorial to use PyNITROS with NITROS](https://nvidia-isaac-ros.github.io/concepts/nitros/pynitros/tutorial_pynitros.html)

## API Reference [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html\#api-reference "Link to this heading")

_class_ PyNitrosPublisher( _node_, _nitros\_bridge\_msg\_type_, _nitros\_bridge\_topic\_name_, _raw\_ros\_topic\_name_) [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosPublisher "Link to this definition")

Publisher to publish PyNITROS messages.

Parameters:

- **node** ( _rclpy.node.Node_) – The ROS node.

- **nitros\_bridge\_msg\_type** ( _MsgType_) – The type of NITROS Bridge message the publisher will publish.

- **nitros\_bridge\_topic\_name** ( _str_) – The name of NITROS Bridge topic the publisher will publish to.

- **raw\_ros\_topic\_name** ( _str_) – The name of raw topic the publisher will publish to when applicable.


publish( _nitros\_bridge\_msg_) [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosPublisher.publish "Link to this definition")

Publish a PyNITROS message.

Parameters:

**nitros\_bridge\_msg** ( _isaac\_ros\_nitros\_bridge\_interfaces.msg_) – The NITROS Bridge message to publish.

Returns:

None

_class_ PyNitrosSubscriber( _node_, _message\_type_, _sub\_topic\_name_, _enable\_ros\_subscribe_) [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosSubscriber "Link to this definition")

Subscriber subscribe to PyNITROS messages.

Parameters:

- **node** ( _rclpy.node.Node_) – The ROS node.

- **message\_type** ( _MsgType_) – The type of NITROS Bridge message the subscriber will subscribe to.

- **sub\_topic\_name** ( _str_) – The name of the topic the subscriber will subscribe to.

- **enable\_ros\_subscribe** ( _bool_) – A flag to enable subscribing to raw message exclusively.


create\_subscription( _input\_callback_) [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosSubscriber.create_subscription "Link to this definition")

Create a subscription with a user-defined callback function. The callback function will be
triggered with a PyNITROS view of the arrived message.

Parameters:

**input\_callback** – A user-defined callback for the subscriber.

Returns:

None

_class_ PyNITROSMessageFilter( _node_, _subscribers_, _synchronizer\_type_, _callback_, _queue\_size=10_, _slop=0.1_) [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNITROSMessageFilter "Link to this definition")

Message filters to synchronize multiple NITROS Bridge or ROS2 raw messages.

Parameters:

- **node** ( _rclpy.node.Node_) – The ROS node.

- **subscribers** ( _list_) – A list of PyNITROS or `rclpy` subscribers.

- **synchronizer\_type** ( _SynchronizerType_) – The type of synchronizer to use. Select from `TimeSynchronizer` or `ApproximateTimeSynchronizer`

- **callback** ( _Callable_) – A user-defined callback for the message filter.

- **queue\_size** ( _int_) – The size of the queue for the message filter.

- **slop** ( _float_) – The delay (in seconds) with which messages can be synchronized.


_class_ PyNitrosImageBuilder( _num\_buffer_, _timeout_) [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosImageBuilder "Link to this definition")

A class to build PyNITROS images.

Parameters:

- **num\_buffer** ( _int_) – The number of CUDA IPC buffers to allocate.

- **timeout** ( _int_) – The timeout for reclaiming the buffer back to the pool.


build( _image\_data_, _image\_height_, _image\_width_, _image\_step_, _image\_encoding_, _image\_header_, _device\_id_, _enable\_ros\_publish_, _event=None_) [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosImageBuilder.build "Link to this definition")

Build a PyNITROS image.

Parameters:

- **image\_data** ( _int_) – Device data pointer of the image.

- **image\_height** ( _int_) – Height of the image.

- **image\_width** ( _int_) – Width of the image.

- **image\_step** ( _int_) – Step of the image.

- **image\_encoding** ( _str_) – Encoding of the image.

- **image\_header** ( _str_) – Header of the image.

- **device\_id** ( _int_) – Device ID on which the image data is stored.

- **enable\_ros\_publish** ( _bool_) – A flag to enable publishing ROS message in the meantime.

- **event** ( _cuda.cudart.cudaEvent\_t_) – CUDA event to synchronize on.


Returns:

Tuple of the built NITROS Bridge image, and the raw ROS image if `enable_ros_publish` is set to `True`.

Return type:

tuple

_class_ PyNitrosTensorListBuilder( _num\_buffer_, _timeout_) [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorListBuilder "Link to this definition")

A class to build PyNITROS tensor list.

Parameters:

- **num\_buffer** ( _int_) – The number of CUDA IPC buffers to allocate.

- **timeout** ( _int_) – The minimum time allows reclaiming the buffer back to the pool.


build\_tensor( _tensor\_data_, _tensor\_name_, _tensor\_shape_, _tensor\_data\_type_) [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorListBuilder.build_tensor "Link to this definition")

Build a PyNITROS tensor.

Parameters:

- **tensor\_data** ( _int_) – Device data pointer of the tensor.

- **tensor\_name** – Name of the tensor.

- **tensor\_shape** ( _list_) – Shape of the tensor.

- **tensor\_data\_type** ( _int_) – Data type of the tensor.


Returns:

The built NITROS Bridge tensor.

Return type:

NitrosBridgeTensor

build( _built\_tensors_, _tensors\_header_, _device\_id_, _enable\_ros\_publish_, _event=None_) [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorListBuilder.build "Link to this definition")

Build a PyNITROS tensor list.

Parameters:

- **built\_tensors** ( _int_) – Built tensors returned by `build_tensor` function.

- **tensor\_header** ( _str_) – Header of the tensor list.

- **device\_id** ( _int_) – Device ID on which the tensor list is stored.

- **enable\_ros\_publish** ( _bool_) – A flag to enable publishing ROS message in the meantime.

- **event** ( _cuda.cudart.cudaEvent\_t_) – CUDA event to synchronize on.


Returns:

Tuple of the built NITROS Bridge tensor list, and the raw ROS tensor list if `enable_ros_publish` is set to `True`.

Return type:

tuple

_class_ PyNitrosImageView [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosImageView "Link to this definition")

A class to view PyNITROS image.

get\_size\_in\_bytes() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosImageView.get_size_in_bytes "Link to this definition")

Get the size of the PyNITROS image in bytes.

Returns:

The size of the PyNITROS image in bytes.

Return type:

int

get\_width() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosImageView.get_width "Link to this definition")

Get the width of the PyNITROS image.

Returns:

The width of the PyNITROS image.

Return type:

int

get\_height() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosImageView.get_height "Link to this definition")

Get the height of the PyNITROS image.

Returns:

The height of the PyNITROS image.

Return type:

int

get\_stride() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosImageView.get_stride "Link to this definition")

Get the stride of the PyNITROS image.

Returns:

The stride of the PyNITROS image.

Return type:

int

get\_buffer() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosImageView.get_buffer "Link to this definition")

Get the device data pointer of the PyNITROS image.

Returns:

The device data pointer of the PyNITROS image.

Return type:

int

get\_encoding() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosImageView.get_encoding "Link to this definition")

Get the encoding of the PyNITROS image.

Returns:

The encoding of the PyNITROS image.

Return type:

str

get\_frame\_id() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosImageView.get_frame_id "Link to this definition")

Get the frame ID of the PyNITROS image.

Returns:

Frame ID of the PyNITROS image.

Return type:

str

get\_timestamp\_seconds() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosImageView.get_timestamp_seconds "Link to this definition")

Get the seconds from the PyNITROS image header.

Returns:

Seconds from the PyNITROS image header.

Return type:

int

get\_timestamp\_nanoseconds() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosImageView.get_timestamp_nanoseconds "Link to this definition")

Get the nanoseconds from the PyNITROS image header.

Returns:

Nanoseconds from the PyNITROS image header.

Return type:

int

set\_stream( _stream_) [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosImageView.set_stream "Link to this definition")

If the subscriber callback contains asynchronous CUDA operations, set the stream for the PyNITROS image view.

Parameters:

**stream** ( _cuda.cudart.cudaStream\_t_) – CUDA stream.

_class_ PyNitrosTensorListView [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorListView "Link to this definition")

A class to view PyNITROS tensor list.

get\_size\_in\_bytes() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorListView.get_size_in_bytes "Link to this definition")

Get the total size of the PyNITROS tensor list in bytes.

Returns:

Total size of the PyNITROS tensor in bytes.

Return type:

int

get\_buffer() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorListView.get_buffer "Link to this definition")

Get the device pointer of the first PyNITROS tensor data.

Returns:

The device pointer of the first PyNITROS tensor.

Return type:

int

get\_tensor\_count() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorListView.get_tensor_count "Link to this definition")

Get the number of tensors in the PyNITROS tensor list.

Returns:

Number of tensors in the PyNITROS tensor list.

Return type:

int

get\_named\_tensor( _name_) [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorListView.get_named_tensor "Link to this definition")

Get the tensor with the specified name from the PyNITROS tensor list.

Parameters:

**name** ( _str_) – The name of the tensor.

Returns:

The tensor with the specified name.

Return type:

[PyNitrosTensorView](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorView "PyNitrosTensorView")

get\_all\_tensors() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorListView.get_all_tensors "Link to this definition")

Get all the tensors in the PyNITROS tensor list.

Returns:

All the tensors in the PyNITROS tensor list.

Return type:

list

get\_frame\_id() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorListView.get_frame_id "Link to this definition")

Get the frame ID of the PyNITROS tensor.

Returns:

Frame ID of the PyNITROS tensor.

Return type:

str

get\_timestamp\_seconds() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorListView.get_timestamp_seconds "Link to this definition")

Get the seconds from the PyNITROS image header.

Returns:

Seconds from the PyNITROS image header.

Return type:

int

get\_timestamp\_nanoseconds() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorListView.get_timestamp_nanoseconds "Link to this definition")

Get the nanoseconds from the PyNITROS image header.

Returns:

Nanoseconds from the PyNITROS image header.

Return type:

int

set\_stream( _stream_) [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorListView.set_stream "Link to this definition")

If the subscriber callback contains asynchronous CUDA operations, set the stream for the PyNITROS image view.

Parameters:

**stream** ( _cuda.cudart.cudaStream\_t_) – CUDA stream.

_class_ PyNitrosTensorView [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorView "Link to this definition")

A class to view PyNITROS tensors.

get\_name() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorView.get_name "Link to this definition")

Get the name of the PyNITROS tensor.

Returns:

The name of the PyNITROS tensor.

Return type:

str

get\_buffer() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorView.get_buffer "Link to this definition")

Get the device pointer of the PyNITROS tensor data.

Returns:

The device pointer of the PyNITROS tensor.

Return type:

int

get\_rank() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorView.get_rank "Link to this definition")

Get the rank of the PyNITROS tensor.

Returns:

The rank of the PyNITROS tensor.

Return type:

int

get\_element\_count() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorView.get_element_count "Link to this definition")

Get the element count of the PyNITROS tensor.

Returns:

The element count of the PyNITROS tensor.

Return type:

int

get\_tensor\_size() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorView.get_tensor_size "Link to this definition")

Get the size in bytes of the PyNITROS tensor.

Returns:

The size of the PyNITROS tensor.

Return type:

int

get\_shape() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorView.get_shape "Link to this definition")

Get the shape of the PyNITROS tensor.

Returns:

The shape of the PyNITROS tensor.

Return type:

list

get\_element\_type() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorView.get_element_type "Link to this definition")

Get the data type of the PyNITROS tensor.

Returns:

The data type of the PyNITROS tensor.

Return type:

str

get\_bytes\_per\_element() [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html#PyNitrosTensorView.get_bytes_per_element "Link to this definition")

Get the element size in bytes of the PyNITROS tensor.

Returns:

The element size of the PyNITROS tensor in bytes.

Return type:

int

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nitros/isaac_ros_pynitros/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/index.html)