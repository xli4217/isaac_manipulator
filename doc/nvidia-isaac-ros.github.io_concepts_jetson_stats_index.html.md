- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Concepts](https://nvidia-isaac-ros.github.io/concepts/index.html)
- Jetson Stats
- [View page source](https://nvidia-isaac-ros.github.io/_sources/concepts/jetson_stats/index.rst.txt)

* * *

# Jetson Stats [](https://nvidia-isaac-ros.github.io/concepts/jetson_stats/index.html\#jetson-stats "Link to this heading")

## Overview [](https://nvidia-isaac-ros.github.io/concepts/jetson_stats/index.html\#overview "Link to this heading")

The Isaac ROS Jetson Stats package wraps the output from the [jetson\_stats](https://rnext.it/jetson_stats) package and publishes a diagnostic message with the status of your device.
It remaps all board statuses to the desired format for more accessible analysis and monitoring.

![jtop terminal](https://rnext.it/jetson_stats/_images/jtop.gif)

`jetson-stats` is a powerful tool to analyze your board, you can use with a stand alone application with jtop or import in your Python script, the main features are:

> - Decode hardware, architecture, L4T and NVIDIA JetPack
>
> - Monitoring, CPU, GPU, memory, engines, fan
>
> - Control NVP model, fan speed, `jetson_clocks`
>
> - Importable in a Python script
>
> - Containerized
>
> - Do not need super user
>
> - Tested on many different hardware configurations
>
> - Works with all NVIDIA JetPack versions

## Resources [](https://nvidia-isaac-ros.github.io/concepts/jetson_stats/index.html\#resources "Link to this heading")

- For more information on `jetson-stats`, see [Documentation](https://rnext.it/jetson_stats).

- For more information on `jtop` documentation, see [jtop](https://rnext.it/jetson_stats/reference/index.html).

- For more information on NVIDIA Jetson documentation, see [jtop](https://docs.nvidia.com/jetson/archives/r35.5.0/DeveloperGuide/index.html).


Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/concepts/jetson_stats/index.html)[latest](https://nvidia-isaac-ros.github.io/concepts/jetson_stats/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/concepts/jetson_stats/index.html)