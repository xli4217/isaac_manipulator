- [Home](https://nvidia-isaac-ros.github.io/index.html)
- [Getting Started](https://nvidia-isaac-ros.github.io/getting_started/index.html)
- [Hardware Setup](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/index.html)
- Compute Setup
- [View page source](https://nvidia-isaac-ros.github.io/_sources/getting_started/hardware_setup/compute/index.rst.txt)

* * *

# Compute Setup [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/index.html\#compute-setup "Link to this heading")

## x86 Platforms [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/index.html\#x86-platforms "Link to this heading")

1. Prepare a NVIDIA-powered platform with the following minimum specs:

   - Ubuntu 22.04+

     - **Experimental**: WSL2 on Windows 11
   - 16 GB general RAM

   - Discrete NVIDIA GPU with the following specs:


     - supports CUDA 12.6+ with `Ampere` NVIDIA GPU Architecture or newer

     - minimum 8GB of VRAM and recommended 12GB+


Note

Various DNN models when loaded together can consume more memory than you may have available.
2. Install Docker from official instructions ( [here](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)). (Recommended 27.2.0 or newer)

3. Install the latest NVIDIA GPU Driver from official instructions ( [here](https://ubuntu.com/server/docs/nvidia-drivers-installation)). (Minimum 560+ is required, see [here](https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html#id5)).


## Jetson Platforms [](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/index.html\#jetson-platforms "Link to this heading")

1. [Install Jetpack](https://docs.nvidia.com/jetson/jetpack/install-setup/index.html) including the `nvidia-container` package.

2. After boot, confirm that you have installed the correct version of `Jetpack` by
running the following command. Confirm that the output has the terms
`R36 (release), REVISION: 4.0`.





```
cat /etc/nv_tegra_release

```

Copy to clipboard

3. Run the following command to set the GPU and CPU clock to max. See
[Maximizing Jetson\\
Performance](https://docs.nvidia.com/jetson/archives/r35.1/DeveloperGuide/text/SD/PlatformPowerAndPerformance/JetsonOrinNxSeriesAndJetsonAgxOrinSeries.html?highlight=maxn#maximizing-jetson-orin-performance)
for more details.





```
sudo /usr/bin/jetson_clocks

```

Copy to clipboard

4. Run the following command to set the to power to MAX settings. See
[Power Mode\\
Controls](https://docs.nvidia.com/jetson/archives/r35.1/DeveloperGuide/text/SD/PlatformPowerAndPerformance/JetsonOrinNxSeriesAndJetsonAgxOrinSeries.html?highlight=maxn#power-mode-controls)
for more details.





```
sudo /usr/sbin/nvpmodel -m 0

```

Copy to clipboard

5. Add your user to the `docker` group.





```
sudo usermod -aG docker $USER
newgrp docker

```

Copy to clipboard

6. Setup Docker.

From the official Docker install instructions ( [here](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)), install the `docker-buildx-plugin.`





```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
"deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
"$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt install docker-buildx-plugin

```

Copy to clipboard


Consider adding more storage to your Jetson for a better experience: [Developer Environment Setup for Jetson](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/jetson_storage.html)

Version:
v: latest


Releases[release-3.2](https://nvidia-isaac-ros.github.io/v/release-3.2/getting_started/hardware_setup/compute/index.html)[latest](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/index.html)[release-2.1](https://nvidia-isaac-ros.github.io/v/release-2.1/getting_started/hardware_setup/compute/index.html)[release-3.1](https://nvidia-isaac-ros.github.io/v/release-3.1/getting_started/hardware_setup/compute/index.html)