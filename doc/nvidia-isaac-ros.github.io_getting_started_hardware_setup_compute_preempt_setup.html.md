- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Getting Started](https://nvidia-isaac-ros.github.io/getting_started/index.html)
- [Hardware Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/index.html)
- [Compute Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/index.html)
- PREEMPT\_RT Kernel for Jetson
- [View page source](https://nvidia-isaac-ros.github.io/_sources/getting_started/hardware_setup/compute/preempt_setup.rst.txt)

* * *

# PREEMPT\_RT Kernel for Jetson [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/preempt_setup.html\#preempt-rt-kernel-for-jetson "Link to this heading")

We provide a PREEMPT\_RT kernel for `Jetpack 6.1`. This kernel is recommended only for use with robot manipulators, as many manipulator manufacturers recommend a low-latency kernel for interfacing with their robots.

## Install PREEMPT\_RT kernel from apt server [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/preempt_setup.html\#install-preempt-rt-kernel-from-apt-server "Link to this heading")

1. Follow instructions from [Developer guide](https://docs.nvidia.com/jetson/archives/r36.4/DeveloperGuide/SD/Kernel/KernelCustomization.html?real-time-kernel-using-ota-update#real-time-kernel-using-ota-update)
to install PREEMPT\_RT kernel on Jetson.

2. Check if PREEMPT\_RT kernel is active by running the below command which should return text that
contains `PREEMPT RT`.


> ```
> uname -a
>
> ```
>
> Copy to clipboard


## Reduce Ethernet Latency [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/preempt_setup.html\#reduce-ethernet-latency "Link to this heading")

When connecting your robot through Ethernet, it’s helpful to reduce the latency by running the
following commands.

1. Install Ethernet tools.


> ```
> sudo apt-get install ethtool net-tools
>
> ```
>
> Copy to clipboard

2. Bring down the Ethernet interface.


> ```
> ifconfig eno1 down
>
> ```
>
> Copy to clipboard

3. Tune the `rx-usecs` parameter using `ethtool`.


> ```
> sudo ethtool -C eno1 rx-usecs 64
>
> ```
>
> Copy to clipboard

4. Bring up the interface.


> ```
> ifconfig eno1 up
>
> ```
>
> Copy to clipboard

5. Check if the new value is reflected as `rx-usecs: 64` in the output of `ethtool`.


> ```
> ethtool -c eno1 | grep rx-usecs
>
> ```
>
> Copy to clipboard

6. Now ping your robot’s IP address ( `<ROBOT_IP_ADDRESS>`) to check the latency.


> ```
> sudo ping <ROBOT_IP_ADDRESS> -i 0.001 -D -c 10000 -s 1200
>
> ```
>
> Copy to clipboard


## Uninstall PREEMPT\_RT kernel [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/preempt_setup.html\#uninstall-preempt-rt-kernel "Link to this heading")

1. Remove kernel headers and modules


> ```
> sudo apt remove nvidia-l4t-rt-kernel nvidia-l4t-rt-kernel-headers nvidia-l4t-rt-kernel-oot-modules nvidia-l4t-display-rt-kernel
>
> ```
>
> Copy to clipboard

2. Reboot your machine


> ```
> sudo reboot
>
> ```
>
> Copy to clipboard

3. Verify the kernel version does not include `PREEMPT RT`.


> ```
> uname -a
>
> ```
>
> Copy to clipboard


Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/getting_started/hardware_setup/compute/preempt_setup.html)[latest](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/preempt_setup.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/getting_started/hardware_setup/compute/preempt_setup.html)