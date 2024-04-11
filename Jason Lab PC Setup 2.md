## - Install Gazebo 
```bash
sudo apt install gazebo
```

## - Install RealSense Packages SDK 2.0
- Register the server's public key, make sure apt HTTPS support is installed:
```bash
sudo mkdir -p /etc/apt/keyrings
curl -sSf https://librealsense.intel.com/Debian/librealsense.pgp | sudo tee /etc/apt/keyrings/librealsense.pgp > /dev/null

sudo apt-get install apt-transport-https
```

- Add the server to the list of repositories:
```bash
echo "deb [signed-by=/etc/apt/keyrings/librealsense.pgp] https://librealsense.intel.com/Debian/apt-repo `lsb_release -cs` main" | \
sudo tee /etc/apt/sources.list.d/librealsense.list
sudo apt-get update
```

- Install the libraries:
```bash
sudo apt-get install librealsense2-dkms`  
sudo apt-get install librealsense2-utils
sudo apt-get install librealsense2-dev`  
sudo apt-get install librealsense2-dbg``

sudo apt-get update
sudo apt-get upgrade
```

## - Install RealSense ROS2 Wrapper
```bash
sudo apt install ros-humble-realsense2-*
```

## - Start Realsense Camera Node in RViz2 
```bash
ros2 launch realsense2_camera rs_launch.py
ros2 launch realsense2_camera rs_launch.py depth_module.profile:=640x720x30 pointcloud.enable:=true
```

## - Install SICK 2D LiDAR on ROS2 
- Create a workspace folder, e.g. `sick_scan_ws` (or any other name):
```bash
mkdir -p ./sick_scan_ws
cd ./sick_scan_ws
```

- Clone repositories [https://github.com/SICKAG/libsick_ldmrs](https://github.com/SICKAG/libsick_ldmrs) and [https://github.com/SICKAG/sick_scan_xd](https://github.com/SICKAG/sick_scan_xd):
```bash
mkdir ./src
pushd ./src
git clone https://github.com/SICKAG/libsick_ldmrs.git
git clone https://github.com/SICKAG/sick_scan_xd.git
popd
rm -rf ./build ./build_isolated/ ./devel ./devel_isolated/ ./install ./install_isolated/ ./log/ # remove any files from a previous build
```

- Build sick_generic_caller:
```bash
source /opt/ros/humble/setup.bash
colcon build --packages-select libsick_ldmrs --event-handlers console_direct+
source ./install/setup.bash
colcon build --packages-select sick_scan_xd --cmake-args " -DROS_VERSION=2" --event-handlers console_direct+
source ./install/setup.bash
```

