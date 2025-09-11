- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Repositories and Packages](https://nvidia-isaac-ros.github.io/repositories_and_packages/index.html)
- Isaac ROS Common
- [View page source](https://nvidia-isaac-ros.github.io/_sources/repositories_and_packages/isaac_ros_common/index.rst.txt)

* * *

# Isaac ROS Common [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/index.html\#repo-name "Link to this heading")

## Overview [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/index.html\#overview "Link to this heading")

The [Isaac ROS Common](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common)
repository contains a number of scripts and Dockerfiles to help
streamline development and testing with the Isaac ROS suite.

![Isaac ROS DevOps tools](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/repositories_and_packages/isaac_ros_common/isaac_ros_common_tools.png/)

Docker containers allow you to quickly set up a sensitive set of frameworks
and dependencies to ensure a smooth experience with Isaac ROS packages.
The Dockerfiles for x86\_64 are based on the version 23.10 image from [Deep Learning\\
Frameworks Containers](https://docs.nvidia.com/deeplearning/frameworks/support-matrix/index.html).
On Jetson platforms, JetPack manages all of these dependencies for you.

Use of Docker images enables CI\|CD systems to scale with DevOps work and
run automated testing in cloud native platforms on Kubernetes.

For solutions to known issues, see the [Troubleshooting](https://nvidia-isaac-ros.github.io/troubleshooting/index.html) section.

## Isaac ROS Dev Scripts [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/index.html\#isaac-ros-dev-scripts "Link to this heading")

### `run_dev.sh` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/index.html\#run-dev-sh "Link to this heading")

Builds and launches development environment Docker container with host ROS workspace files mounted, or attaches to an existing running container.

#### Arguments [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/index.html\#arguments "Link to this heading")

| Arg | Long argument | Default | Description |
| --- | --- | --- | --- |
| `-a` | `--docker_arg` | `(none)` | Adds arguments to the `docker run` invocation of the dev container. For example, `-a "-v /host_directory:/container_directory"` |
| `-b` | `--skip_image_build` | `0` (do not skip image build) | Avoids building the base image to reuse the existing image if one exists. Useful for running offline after base image has been built once already. |
| `-d` | `--isaac_ros_dev_dir` | The first directory that exists in order: `~/workspaces/isaac_ros-dev`, `/workspaces/isaac_ros-dev`, or `$SCRIPT_ROOT/../` | The location of the host directory to mount into the dev container as /workspaces/isaac\_ros-dev |
| `-i` | `--image_key` | `ros2_humble` | Image key suffix to be used for preparing base image. Platform `aarch64` or `x86_64` will be prepended and `user` appended for the final image key. |
| `-v` | `--verbose` | `0` (not verbose) | Echoes several key commands to the console for debugging. |

#### Config Keys [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/index.html\#config-keys "Link to this heading")

Shell values read from `~/.isaac_ros_common-config` if exists.

| Key | Type | Example(s) | Description |
| --- | --- | --- | --- |
| `BASE_DOCKER_REGISTRY_NAMES` | Array of string registry names | `("nvcr.io/isaac/ros" "nvcr.io/nvidian/isaac-ros/ros")` | Docker image names to search for tags for matching prebuilt images. |
| `CONFIG_CONTAINER_NAME_SUFFIX` | string | `(blank)` | Suffix to append for container name launched by script. |
| `CONFIG_IMAGE_KEY` | Period-delimited string for base image key | `ros2_humble` | Equivalent of `-i/--image_key` argument. |
| `CONFIG_DOCKER_SEARCH_DIRS` | Array of directory paths | `$SCRIPT_ROOT/../docker` | Array of directories to search for Dockerfiles matches for image keys. |

### `build_image_layers.sh` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/index.html\#build-image-layers-sh "Link to this heading")

Builds a Docker image by parsing an image key (period-delimited string of keys), matching local Dockerfiles corresponding to those keys, and building them in sequence, feeding the result of one as the base image of the next.

#### Arguments [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/index.html\#id1 "Link to this heading")

| Arg | Long argument | Default | Description |
| --- | --- | --- | --- |
| `-a` | `--build_arg` | `(none)` | Adds arguments to the `docker build` invocation of the dev container. For example, `-a "USER_ARG=0"` |
| `-b` | `--base_image` | `(none)` | Base image name to use as the root, usually not needed. If not specified, the first Dockerfile must have a default `BASE_IMAGE` argument value. |
| `-c` | `--context_dir` | `(none)` | Directory to use for docker context for the final layer and included as a docker search directory for matching Dockerfiles. |
| `-i` | `--image_key` | `(required)` | The image key to build. Platform key ( `aarch64` or `x86_64`) will get prepended and `user` will be appended if not included already. |
| `-k` | `--disable_buildkit` | `0` (not disabling BuildKit) | Disables BuildKit when invoking `docker build`. |
| `-n` | `--image_name` | Derived from image key replacing periods with dashes and appending `-image` | Name to use for the Docker image being built. |
| `-r` | `--skip_registry_check` | `0` (not skipping registry check) | If `1`, skips checking any remote registries for prebuilt images with matching hashes of Dockerfiles locally (useful for offline mode if the image is built and just want cached Docker image). |
| `-y` | `--ignore_composite_keys` | `0` (not ignoring composite keys) | If `1`, image key matching only looks at Dockerfiles with a single image key. Dockerfiles with multiple keys will be ignored, such as Dockerfile.key1.key2. |

#### Config Keys [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/index.html\#id2 "Link to this heading")

Shell values read from `~/.isaac_ros_common-config` if exists.

| Key | Type | Example(s) | Description |
| --- | --- | --- | --- |
| `ADDITIONAL_BUILD_ARGS` | Array of strings | `$SCRIPT_ROOT/../docker` | Build arguments to pass to `docker build`. |
| `BASE_DOCKER_REGISTRY_NAMES` | Array of string registry names | `("nvcr.io/isaac/ros" "nvcr.io/nvidian/isaac-ros/ros")` | Docker image names to search for tags for matching prebuilt images. |
| `CONFIG_DOCKER_SEARCH_DIRS` | Array of directory paths | `$SCRIPT_ROOT/../docker` | Array of directories to search for Dockerfiles matches for image keys. |
| `DOCKER_BUILDKIT` | integer boolean | `0` or `1` | Enables or disables Docker BuildKit environment. |
| `IGNORE_COMPOSITE_KEYS` | integer boolean | `0` or `1` | Enables or disables matching Dockerfiles with composite keys, such as Dockerfile.key1.key2. |
| `SKIP_REGISTRY_CHECK` | integer boolean | `0` or `1` | Enables or disables checking remote registries for prebuilt images. |

### `docker_deploy.sh` [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/index.html\#docker-deploy-sh "Link to this heading")

Packages prebuilt build artifacts from a ROS workspace and/or set of Debians into a runnable Docker image.

#### Arguments [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/index.html\#id3 "Link to this heading")

| Arg | Long argument | Default | Description |
| --- | --- | --- | --- |
| `-a` | `--custom_apt_source` | `(none)` | Adds your own `apt-get` sources so that Debian installations can pull from your Debian repositories. |
| `-b` | `--base_image_key` | `${PLATFORM}.ros2_humble` | Image key to use as base image before layering on build products. |
| `-d` | `--include_dir` | `(none)` | Host directory path that will be layered in as a root overlay into the final Docker image. Repeated. |
| `-f` | `--launch_file` | `(none)` | Launch file name to run by default on container entrypoint. Must be specified with `-p`. |
| `-i` | `--install_debians` | `(none)` | Comma-delimited string of Debians to installed with apt-get install on the base image. |
| `-n` | `--name` | `isaac_ros_deploy` | Name to use for the Docker image being built. |
| `-p` | `--launch_package` | `(none)` | ROS package name that hosts the target launch file. Must be specified with `-f`. |
| `-s` | `--suffix_image_key` | `(none)` | Image key to use as additional layers after the build products have been added. |
| `-t` | `--include_tarball` | `(none)` | Tarball to be staged as a root overlay on top of the base image. Repeatable. |
| `-w` | `--ros_ws` | `(none)` | Path to ROS workspace to layer in `install/` directory from and add a `rosdep install` when building Docker image. |

#### Config Keys [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/index.html\#id4 "Link to this heading")

Shell values read from `~/.isaac_ros_common-config` if exists.

| Key | Type | Example(s) | Description |
| --- | --- | --- | --- |
| `BASE_IMAGE_KEY` | Period-delimited string | `release_base_aarch64.ros2_humble` | Base image to layer build products on top of. |
| `CUSTOM_APT_SOURCES` | Array of string `apt` sources | `("deb https://mycool-apt-get-server.com/ros-debian-local focal main")` | Custom `apt` sources to add for Debian installs. |
| `DEPLOY_IMAGE_NAME` | string | `myimage` | Name of Docker image built. |
| `INSTALL_DEBIANS_CSV` | Comma-delimited string | `libnvvpi3,tensorrt,ros-humble-isaac-ros-all` | List of Debians to install on top of base image. |
| `INCLUDE_DIRS` | Array of directory absolute paths | `("/workspaces/myfiles" "/some/other/dir")` | Host directories to include as a root overlay in final image. |
| `INCLUDE_TARBALLS` | Array of directory absolute file paths | `("/workspaces/myfiles.tar.gz" "/some/other/files.tar.gz")` | Paths to `.tar.gz` files containing root overlays. |
| `ROS_WS` | String path | `/workspaces/isaac_ros-dev/ros_ws` | Path to ROS workspace with built `install/` directory to include. |
| `LAUNCH_PACKAGE` | string | `isaac_ros_image_pipeline` | ROS package name hosting the launch file to run by default in container. |
| `LAUNCH_FILE` | string | `isaac_ros_image_flip.launch.py` | Launch file to run by default in container. |

## Supported Platforms [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/index.html\#supported-platforms "Link to this heading")

This package is designed and tested to be compatible with ROS 2 Humble running on [Jetson](https://developer.nvidia.com/embedded-computing) or an x86\_64 system with an NVIDIA GPU.

Note

Versions of ROS 2 other than Humble are **not** supported. This package depends on specific ROS 2 implementation features that were introduced beginning with the Humble release. ROS 2 versions after Humble have not yet been tested.

| Platform | Hardware | Software | Notes |
| --- | --- | --- | --- |
| Jetson | [Jetson Orin](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/) | [JetPack 6.1 and 6.2](https://developer.nvidia.com/embedded/jetpack) | For best performance, ensure that [power settings](https://docs.nvidia.com/jetson/archives/r34.1/DeveloperGuide/text/SD/PlatformPowerAndPerformance.html) are configured appropriately.<br>Jetson Orin Nano 4GB may not have enough memory to run many of the Isaac ROS packages and is not recommended. |
| x86\_64 | `Ampere` or higher NVIDIA GPU Architecture with 8 GB RAM or higher | [Ubuntu 22.04+](https://releases.ubuntu.com/22.04/) | [CUDA 12.6+](https://developer.nvidia.com/cuda-downloads) |

## Docker [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/index.html\#docker "Link to this heading")

To simplify development, we strongly recommend leveraging the Isaac ROS Dev Docker images by following [these steps](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html).
This streamlines your development environment setup with the correct versions of dependencies on both Jetson and x86\_64 platforms.

Note

All Isaac ROS Quickstarts, tutorials, and examples have been designed with the Isaac ROS Docker images as a prerequisite.

## Customize your Dev Environment [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/index.html\#customize-your-dev-environment "Link to this heading")

To customize your development environment, reference [this guide](https://nvidia-isaac-ros.github.io/concepts/docker_devenv/index.html#development-environment).

## Packages [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/index.html\#packages "Link to this heading")

- [`isaac_ros_rosbag_utils`](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/isaac_ros_rosbag_utils/index.html)
  - [Overview](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/isaac_ros_rosbag_utils/index.html#overview)
    - [EDEX Extraction](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/isaac_ros_rosbag_utils/index.html#edex-extraction)

## Updates [](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/index.html\#updates "Link to this heading")

| Date | Changes |
| --- | --- |
| 2024-12-10 | Refactored Dockerfiles |
| 2024-09-26 | Updated for Isaac ROS 3.1 |
| 2024-05-30 | Update to be compatible with JetPack 6.0 |
| 2023-10-18 | Updated for Isaac ROS 2.0.0 |
| 2023-05-25 | Refreshed ROS 2 Humble |
| 2023-04-05 | Experimental support for WSL2, merged image keys `humble` and `nav2` into `ros2_humble` |
| 2022-10-19 | Minor updates and bugfixes |
| 2022-08-31 | Update to be compatible with JetPack 5.0.2 |
| 2022-06-30 | Support ROS 2 Humble and miscellaneous bug fixes. |
| 2022-06-16 | Update `run_dev.sh` and removed `isaac_ros_nvengine` |
| 2021-10-20 | Migrated to [NVIDIA-ISAAC-ROS](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common), added `isaac_ros_nvengine` and `isaac_ros_nvengine_interfaces` packages |
| 2021-08-11 | Initial release to [NVIDIA-AI-IOT](https://github.com/NVIDIA-AI-IOT/isaac_ros_common) |

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/repositories_and_packages/isaac_ros_common/index.html)[latest](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_common/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/repositories_and_packages/isaac_ros_common/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/repositories_and_packages/isaac_ros_common/index.html)