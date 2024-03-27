==26 March 2024==

## Install Ignition Fortress from SubT

- Setup and install dependencies:
```bash
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

```bash
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
```

```bash
sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'
```

```bash
wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
```

```bash
sudo apt-get update

sudo apt-get -y install build-essential cmake libusb-dev libccd-dev libfcl-dev lsb-release pkg-config python3-vcstool python3-colcon-common-extensions \
python3-pip python3-colcon-mixin python3-rosdep libpython3-dev git

sudo apt-get upgrade
sudo rosdep init && rosdep update
```

- Setup the workspace
```bash
mkdir -p ~/subt_ws/src && cd ~/subt_ws/src

git clone https://github.com/osrf/subt -b fortress
git clone https://github.com/ignitionrobotics/ros_ign -b melodic
```

```bash
source /opt/ros/humble/setup.bash
```

- Build and install into workspace
```bash
cd ~/subt_ws/
export IGNITION_VERSION=fortress
rosdep install --from-paths src --ignore-src -r
colcon build --merge-install --packages-skip ros_ign_gazebo
```

- Source SubT setup file
```bash
source ~/subt_ws/install/setup.bash
```

## Install Docker

- Set up Docker's apt repository: 
```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

- Install the Docker packages: 
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

- For a quick sanity check, verify if docker is installed correctly by running:
```bash
docker run hello-world
```

## Download and Install CUDA Toolkit 12.4

- Installation Instructions:
```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-ubuntu2204.pin

sudo mv cuda-ubuntu2204.pin /etc/apt/preferences.d/cuda-repository-pin-600

wget https://developer.download.nvidia.com/compute/cuda/12.4.0/local_installers/cuda-repo-ubuntu2204-12-4-local_12.4.0-550.54.14-1_amd64.deb

sudo dpkg -i cuda-repo-ubuntu2204-12-4-local_12.4.0-550.54.14-1_amd64.deb

sudo cp /var/cuda-repo-ubuntu2204-12-4-local/cuda-*-keyring.gpg /usr/share/keyrings/

sudo apt-get update
sudo apt-get -y install cuda-toolkit-12-4
```

- To install the legacy kernel module flavour:
```bash
sudo apt-get install -y cuda-drivers
```

- To install the open kernel module flavor:
```bash
sudo apt-get install -y nvidia-driver-550-opensudo apt-get install -y cuda-drivers-550
```

- Install Nvidia Driver: (https://www.cyberciti.biz/faq/ubuntu-linux-install-nvidia-driver-latest-proprietary-driver/#verification)
```bash
sudo apt install nvidia-driver-535 nvidia-dkms-535
```

- Double check Dockerfile (https://gitlab.com/nvidia/container-images/cuda/blob/master/doc/supported-tags.md): 
```bash
docker run --runtime=nvidia --rm nvidia/cuda:12.2.2-cudnn8-devel-ubuntu22.04 nvidia-smi
```

## Install Toolkit and Restart Docker

- *If you are trying to use Docker and the CircleCI GPU executor, you may get the following error.* *"docker: Error response from daemon: could not select device driver "" with capabilities: [[gpu]]. ERRO[0407] error waiting for container: context canceled"* *This is due to the removal of the nvidia-container-toolkit when CircleCI Switched to using images with multiple CUDA versions available at runtime.*

- Solution to above error: 
```bash
sudo apt-get update 
sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker 
```

- Verify Compatibility for above solution (Test GPU Docker): 
```bash
docker run --gpus all nvidia/cuda:12.2.2-devel-ubuntu22.04 nvidia-smi
```

## Run the SubT Simulator and relative files: 

- Terminal 1: Open the simulator (Hello Word Testing Sim: https://github.com/osrf/subt_hello_world/tree/master): 
```bash
./subt/docker/run.bash osrf/subt-virtual-testbed:latest \ 
cave_circuit.ign \ 
circuit:=cave \ 
worldName:=simple_cave_01 \ 
robotName1:=X1 \ 
robotConfig1:=X1_SENSOR_CONFIG_1
```

- Terminal 2: Launch the teleop
```bash
docker ps

docker exec -it [CONTAINER_ID] /bin/bash

roslaunch subt_example teleop.launch
```

- *Another test bed:
```bash
docker images | grep subt_sim_entry
docker rmi -f [IMAGE_ID]
 
mkdir -p ~/subt_testbed && cd ~/subt_testbed

wget https://raw.githubusercontent.com/osrf/subt/master/docker/run.bash

chmod +x run.bash

./run.bash osrf/subt-virtual-testbed competition.ign worldName:=tunnel_circuit_practice_01 circuit:=tunnel robotName1:=X1 robotConfig1:=X1_SENSOR_CONFIG_1
```

