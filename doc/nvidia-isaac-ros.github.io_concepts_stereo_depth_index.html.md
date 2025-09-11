- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Concepts](https://nvidia-isaac-ros.github.io/concepts/index.html)
- Stereo Depth
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/stereo_depth/index.rst.txt)

* * *

# Stereo Depth [](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/index.html\#stereo-depth "Link to this heading")

Much like human eyes, stereo vision works by measuring the displacement in pixels, or disparity, between two images of the same visually distinctive feature taken from two shifted imagers.
With the measured disparity and the known length of the baseline (the distance between the two imagers), the distance to (or depth) of that visually distinct feature can be estimated.

![Stereo depth overview](https://media.githubusercontent.com/media/NVIDIA-ISAAC-ROS/.github/main/resources/isaac_ros_docs/concepts/stereo_depth/stereo.png/)

Stereo cameras have two monocular imagers rigidly attached to each other, facing in roughly the same direction. They are separated by some known baseline distance, typically on the scale of centimeters.
The pair of images must be rectified by being projected onto the same epipolar plane using pre-calibrated stereo rectified parameters.
The disparity in pixels between the same features in one image versus the other can be directly correlated to depth using the baseline and focal length of the imagers.

The disparity for each pixel cannot always be determined because it depends on the matching process. If the stereo camera is facing a featureless solid color wall, for example, there are no visually distinct features or corners (sharp variations in contrast) to match between images.
To work around this issue, a few stereo cameras project a pattern of infrared light into the scene that both imagers can always see but that human eyes cannot.

By measuring the disparity for all pairs of matching features in a scene between two images, you can produce a disparity image where each pixel represents the disparity or a `NaN` when disparity can’t be calculated.
Using the baseline and focal length, this disparity image can then be used to produce a more familiar depth image where every pixel represents a depth in metric distances or a point cloud.

- [SGM](https://docs.nvidia.com/vpi/algo_stereo_disparity.html) (semi-global matching) is a ubiquitous analytic algorithm for calculating stereo disparity. SGM has difficulty with smooth or regular surfaces and can suffer from excessive noise.

- [ESS](https://arxiv.org/abs/1803.09719) (efficient semi-supervised) is based on a deep-learned, pre-trained model available that can more intelligently handle smooth gradients and edges inferred from context.

- [Bi3D](https://arxiv.org/abs/2005.07274) casts the problem of continuous depth estimation to one of a series of ranged binary classification using deep-learned pre-trained models. Rather than testing if objects are at a particular depth, it classifies them as being closer or farther than some value of disparity. By collapsing a series of these binary classification masks, the depth can be classified within configurable ranges. For example, anything less than 1m away would have the same value in the output of Bi3D of 1 and anything further away would be 0.


- [SGM](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/sgm/index.html)
  - [Repositories and Packages](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/sgm/index.html#repositories-and-packages)
- [ESS](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/ess/index.html)
  - [Repositories and Packages](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/ess/index.html#repositories-and-packages)
- [Bi3D](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/bi3d/index.html)
  - [Repositories and Packages](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/bi3d/index.html#repositories-and-packages)

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/stereo_depth/index.html)[latest](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/concepts/stereo_depth/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/stereo_depth/index.html)