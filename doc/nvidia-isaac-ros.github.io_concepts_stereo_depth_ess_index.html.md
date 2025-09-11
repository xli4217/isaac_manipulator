- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Concepts](https://nvidia-isaac-ros.github.io/concepts/index.html)
- [Stereo Depth](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/index.html)
- ESS
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/stereo_depth/ess/index.rst.txt)

* * *

# ESS [](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/ess/index.html\#ess "Link to this heading")

[ESS](https://arxiv.org/pdf/1803.09719.pdf) stands for Efficient
Semi-Supervised stereo disparity and was developed by NVIDIA. The ESS DNN is
used to predict the disparity for each pixel from stereo camera image
pairs. This network has improvements over classic CV approaches that use
epipolar geometry to compute disparity, because the DNN can learn to predict
disparity in cases where epipolar geometry feature matching fails. The
semi-supervised learning and stereo disparity matching makes the ESS DNN
robust in environments unseen in the training datasets and with occluded
objects. This DNN is optimized for and evaluated with color (RGB) global
shutter stereo camera images and accuracy can vary for monochrome
stereo images used in analytic computer vision approaches to stereo
disparity.

The predicted
[disparity](https://en.wikipedia.org/wiki/Binocular_disparity) values
represent the distance a point moves from one image to the other in a
stereo image pair (a.k.a. the binocular image pair). The disparity is
inversely proportional to the depth, that is, `disparity = focalLength x baseline / depth`. Given the [focal\\
length](https://en.wikipedia.org/wiki/Focal_length) and
[baseline](https://en.wikipedia.org/wiki/Stereo_camera) of the camera
that generates a stereo image pair, the predicted disparity map from the
`isaac_ros_ess` package can be used to compute depth and generate a
[point cloud](https://en.wikipedia.org/wiki/Point_cloud).

## Repositories and Packages [](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/ess/index.html\#repositories-and-packages "Link to this heading")

The Isaac ROS implementations of this technology are available here:

- [Isaac ROS Depth Estimation using ESS](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_dnn_stereo_depth/isaac_ros_ess/index.html)


Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/stereo_depth/ess/index.html)[latest](https://nvidia-isaac-ros.github.io/concepts/stereo_depth/ess/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/concepts/stereo_depth/ess/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/stereo_depth/ess/index.html)