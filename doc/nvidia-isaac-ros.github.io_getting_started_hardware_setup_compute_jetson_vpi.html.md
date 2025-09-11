- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Getting Started](https://nvidia-isaac-ros.github.io/getting_started/index.html)
- [Hardware Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/index.html)
- [Compute Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/index.html)
- Jetson Setup for VPI
- [View page source](https://nvidia-isaac-ros.github.io/_sources/getting_started/hardware_setup/compute/jetson_vpi.rst.txt)

* * *

# Jetson Setup for VPI [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/jetson_vpi.html\#jetson-setup-for-vpi "Link to this heading")

The following instructions are for running compute on Jetson device’s PVA accelerator.
The steps are required to run on Jetson device outside a docker container.

1. Generate CDI Spec for GPU/PVA:

Ensure NVIDIA Container Toolkit is installed on the Jetson device.
Use the following command to generate the CDI spec:





```
sudo nvidia-ctk cdi generate --mode=csv --output=/etc/cdi/nvidia.yaml

```

Copy to clipboard

2. Install `pva-allow-2` package:





```
# Add Jetson public APT repository
sudo apt-get update
sudo apt-get install software-properties-common
sudo apt-key adv --fetch-key https://repo.download.nvidia.com/jetson/jetson-ota-public.asc
sudo add-apt-repository 'deb https://repo.download.nvidia.com/jetson/common r36.4 main'
sudo apt-get update
sudo apt-get install -y pva-allow-2

```

Copy to clipboard


Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/getting_started/hardware_setup/compute/jetson_vpi.html)[latest](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/jetson_vpi.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/index.html)