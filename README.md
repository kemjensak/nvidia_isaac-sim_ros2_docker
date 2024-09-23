
# nvidia_isaac-sim_ros2_docker
Run NVIDIA Isaac Sim in a Docker container with ROS2 Humble and ROS2 bridge already set up.

I work with Ubuntu 24.04.1 LTS and I found that NVIDIA Isaac Sim isn't running on it yet. Thus, I had to dockerize it over ubuntu 22.04. Furthermore, I wanted to build an image with ROS2 Humble installed too, as the ROS2 bridge for Isaac Sim.

Consequently, I write the Dockerfile in this repository. I hope it helps you! 

In order to build the image, you can either follow the steps manually or run the bash script ``build.sh``.<br>
In order to run the container, you can run it manually as shown below or run the bash script ``run.sh``.

# Specifications
This repository has been run with the following specifications:

OS: ``Ubuntu 24.04.1 LTS``*<br>
RAM: ``32 GB``<br>
Processor: ``13th Gen Intel® Core™ i7-13650HX × 20``<br>
Graphics card: ``NVIDIA GeForce RTX 4060 Laptop GPU``<br>
Graphics card memory: ``8 GB``<br>
Needed disk space: ``22 GB``<br>

It should work in previous releases as 20.04 and 22.04.

# Isaac Sim version
``4.2.0``

# ROS2 version
``ROS2 Humble Desktop``

# Docker version
``Client: Docker Engine - Community``<br>
``Version:           27.3.1``<br>
``API version:       1.47``<br>
``Go version:        go1.22.7``<br>
``Git commit:        ce12230``<br>
``Built:             Fri Sep 20 11:40:59 2024``<br>
``OS/Arch:           linux/amd64``<br>
``Context:           default``<br>

``Server: Docker Engine - Community``<br>
`Engine:`<br>
` Version:          27.3.1`<br>
` API version:      1.47 (minimum version 1.24)`<br>
`Go version:       go1.22.7`<br>
`Git commit:       41ca978`<br>
`Built:            Fri Sep 20 11:40:59 2024`<br>
`  OS/Arch:          linux/amd64`<br>
`  Experimental:     false`<br>
` containerd:`<br>
`  Version:          1.7.22`<br>
`  GitCommit:        7f7fdf5fed64eb6a7caf99b3e12efcf9d60e311c`<br>
` runc:`<br>
`  Version:          1.1.14`<br>
 ` GitCommit:        v1.1.14-0-g2c9f560`<br>
 `docker-init:`<br>
  `Version:          0.19.0`<br>
 ` GitCommit:        de40ad0`<br>


# Build image
```bash
docker build -t {IMAGE_NAME}:{TAG} .
```
Example:
```bash
docker build -t isaac_sim_ros2:4.2.0-Humble .
```

# Run container
Allow running graphic interfaces in the container:
```bash
xhost +
```
Run the container with the needed configuration:
```bash
docker run --name isaac-sim --entrypoint bash -it --gpus all -e "ACCEPT_EULA=Y" --rm --network=host   -e "PRIVACY_CONSENT=Y"   -v $HOME/.Xauthority:/root/.Xauthority   -e DISPLAY   -v ~/docker/isaac-sim/cache/kit:/isaac-sim/kit/cache:rw   -v ~/docker/isaac-sim/cache/ov:/root/.cache/ov:rw   -v ~/docker/isaac-sim/cache/pip:/root/.cache/pip:rw   -v ~/docker/isaac-sim/cache/glcache:/root/.cache/nvidia/GLCache:rw   -v ~/docker/isaac-sim/cache/computecache:/root/.nv/ComputeCache:rw   -v ~/docker/isaac-sim/logs:/root/.nvidia-omniverse/logs:rw   -v ~/docker/isaac-sim/data:/root/.local/share/ov/data:rw   -v ~/docker/isaac-sim/documents:/root/Documents:rw   isaac_sim_ros2:4.2.0-Humble
```

REMEMBER: if you want to share a folder between the host and the container, mount it adding the next flag to the previous command:
```bash
-v HOST_PATH:PATH_IN_CONTAINER
```
```bash
-v ~/Documents/isaac_sim/PROJECT_ID:~/PROJECT_ID
```

# Run Isaac Sim inside the container
If this command is included when running the container, ROS2 bridge will fail. That's because the container with ROS2 packages must be started first, and then Isaac Sim.
```bash
./runapp.sh
```

# Bibliography
https://docs.omniverse.nvidia.com/isaacsim/latest/installation/install_container.html

https://omniverse-content-production.s3-us-west-2.amazonaws.com/Assets/Isaac/Documentation/Isaac-Sim-Docs_2022.2.1/isaacsim/latest/install_ros.html

https://github.com/NVIDIA-Omniverse/IsaacSim-dockerfiles
