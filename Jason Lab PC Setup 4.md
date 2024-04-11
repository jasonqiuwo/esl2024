
## Install Github Desktop: 

```bash
## Get the @shiftkey package feed
wget -qO - https://apt.packages.shiftkey.dev/gpg.key | gpg --dearmor | sudo tee /usr/share/keyrings/shiftkey-packages.gpg > /dev/null

sudo sh -c 'echo "deb [arch=amd64 signed-by=/usr/share/keyrings/shiftkey-packages.gpg] https://apt.packages.shiftkey.dev/ubuntu/ any main" > /etc/apt/sources.list.d/shiftkey-packages.list'

## Install Github Desktop for Ubuntu
sudo apt update 
sudo apt install github-desktop
```

## Install TexLive Dependencies: 

- Try Install TexStudio in Ubuntu Software Store, and the run this:
```bash
sudo apt install texlive-full
```

## Fixing Docker Image Build and other issues: 



## Install Visual Studio Code

```bash
sudo snap install code --classic
```

## Extra Installations to run Gazebo and ROS2 bridge: 

- Install Fuel Models (https://gazebosim.org/libs/fuel_tools)
```bash
sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'

wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -

sudo apt-get update
sudo apt-get install libgz-fuel-tools7-dev

sudo apt install ruby-ffi libzip-dev libcurl-dev libjsoncpp-dev

gz fuel download -u https://fuel.gazebosim.org/1.0/OpenRobotics/models/Ambulance -v 4
```

- Install Gazebo Plugins (https://gazebosim.org/api/plugin/2/installation.html): 
```bash
sudo apt-get update
sudo apt-get install lsb-release

sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'

wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -

sudo apt-get update
sudo apt install libgz-plugin2-dev
```

- Install Gazebo Renderings (https://gazebosim.org/api/rendering/8/installation.html):
```bash
sudo apt-get update
sudo apt-get -y install wget lsb-release gnupg

sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'

wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -

sudo apt-get update
- sudo apt-get install libgz-rendering7-dev
```

- Install Supported Rendering Engines (OGRE 1 & 2): 
```bash
sudo apt-get install libogre-1.9-dev

sudo apt -y install wget lsb-release gnupg

sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'

wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -

sudo apt update

sudo apt install libogre-next-dev
```