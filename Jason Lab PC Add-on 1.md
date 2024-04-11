## Install and build ORB_SLAM2 with ROS2

- Create a Workspace for ORB_SLAM2
```bash
mkdir ORB_SLAM2_ws
cd ORB_SLAM2_ws
mkdir src
cd src
```

- Download ORB_SLAM2
```bash
git clone https://github.com/raulmur/ORB_SLAM2.git
```

- Build ORB_SLAM2
```bash
cd ORB_SLAM2 
chmod +x build.sh
./build.sh
```

- Install Dependencies
```bash
sudo apt-get install libglew-dev libglfw3-dev libopencv-dev
```

- Run ORB_SLAM2
```bash
ros2 run ORB_SLAM2 Mono path_to_vocabulary path_to_settings
```

## Build Pangolin

```bash
git clone --recursive https://github.com/stevenlovegrove/Pangolin.git
cd Pangolin

./scripts/install_prerequisites.sh recommended
```

- Configure and build
```bash
cmake -B build
cmake --build build

cmake -B build -GNinja
cmake --build build

cmake --build build -t pypangolin_pip_install

cmake -B build -G Ninja -D BUILD_TESTS=ON
cmake --build build
cd build
```

## Install TurtleBot3 Simulation Package

1. Install Gazebo
```bash
sudo apt install ros-humble-gazebo-*
```
    
2. Install Cartographer
```bash
sudo apt install ros-humble-cartographer
sudo apt install ros-humble-cartographer-ros
```
    
3. Install Navigation2
```bash
sudo apt install ros-humble-navigation2
sudo apt install ros-humble-nav2-bringup
```
    
4. Install TurtleBot3 via Debian Packages.
```
source ~/.bashrc
sudo apt install ros-humble-dynamixel-sdk
sudo apt install ros-humble-turtlebot3-msgs
sudo apt install ros-humble-turtlebot3
```

5. Set the ROS environment for PC.
    
    ```
    $ echo 'export ROS_DOMAIN_ID=30 #TURTLEBOT3' >> ~/.bashrc
    $ source ~/.bashrc
    ```
```
echo 'export ROS_DOMAIN_ID=30 #TURTLEBOT3' >> ~/.bashrc
source ~/.bashrc

mkdir turtlebot3_ws
cd turtlebot3_ws
mkdir src
cd src

git clone -b humble-devel https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git
cd ~/turtlebot3_ws && colcon build --symlink-install
```

6. Launch Gazebo error fix: (Add the following and then run Gazebo)
```
source /usr/share/gazebo/setup.bash
```

7. Simulate Gazebo World turtlebot3: 
```
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```

8. Teleoperate with keyboard
```
ros2 run turtlebot3_teleop teleop_keyboard
```

9. Run Cartographer SLAM mode
```
ros2 launch turtlebot3_cartographer cartographer.launch.py use_sim_time:=True
```

10. Save Map
```
ros2 run nav2_map_server map_saver_cli -f ~/map
```

11. Run Navigation simulation 
```
ros2 launch turtlebot3_navigation2 navigation2.launch.py use_sim_time:=True map:=$HOME/map.yaml
```

12. Fake Node Simulation
```
ros2 launch turtlebot3_fake_node turtlebot3_fake_node.launch.py
```


