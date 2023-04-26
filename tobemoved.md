## From a fresh install of ubuntu 20.04.6

Some packages craves a fair amount om installed RAM to build without errors.

A workaround solution if you only have <16Gb RAM; follow this simple instruction to increase your swap (default size 2Gb): [increase swap]( https://github.com/discoimp/ORB_SLAM3#02-create-a-new-swap-file-optional)

Make sure your system is up to date and rebooted (if necessary)
``` 
sudo apt update && sudo apt upgrade -y
reboot
```

### Install prerequisites:

```
sudo apt update && sudo apt install git curl liblapack-dev libblas-dev python3-catkin-tools python3-vcstool -y
```


### Install [ROS Noetic](http://wiki.ros.org/noetic/Installation/Debian)
Add the repository to your sources and install ROS:
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
sudo apt update && sudo apt install ros-noetic-desktop-full -y
```
If it fails the first time, try running the following:
```
sudo apt update && sudo apt upgrade -y && sudo apt install --fix-missing
```
Then just run it again
```
sudo apt update && sudo apt install ros-noetic-desktop-full -y
```

Source your installation
```
source /opt/ros/noetic/setup.bash

# Optionally make ROS automatically sourced in every termninal session:
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
```

### Install the ROS-enabled Event camera [driver](https://github.com/discoimp/rpg_dvs_ros)
```
# Downloads convenient install scripts
curl -o /tmp/install_event_driver.sh -LO https://raw.githubusercontent.com/discoimp/rpg_dvs_ros/master/install_event_driver.sh && curl -o /tmp/check_prerequisites.sh -LO https://raw.githubusercontent.com/discoimp/rpg_dvs_ros/master/check_prerequisites.sh && chmod +x /tmp/install_event_driver.sh /tmp/check_prerequisites.sh

# Checks and asks to install missing dependencies
sudo -E /tmp/check_prerequisites.sh

# Creates and builds your workspace
/tmp/install_event_driver.sh
```
Source your installation
```
# The script already sourced your installation
# Optionally automatically source in every new termninal session:
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
```


### Install USB/IP support
(not needed for dataset playback)
Install Virtual Here
```
sudo apt install linux-tools-virtual hwdata linux-tools-$(uname -r) linux-tools-generic libcanberra-gtk-module libcanberra-gtk3-module
sudo update-alternatives --install /usr/local/bin/usbip usbip $(command -v ls /usr/lib/linux-tools/*/usbip | tail -n1) 20
sudo apt update && sudo apt install usbip
```



### Install [Ultimate SLAM](https://github.com/discoimp/rpg_ultimate_slam_open/blob/main/docs/Installation-of-UltimateSLAM.md)
Create the Catkin workspace
```
mkdir -p ~/uslam_ws/src && cd ~/uslam_ws && catkin init && catkin config --extend /opt/ros/$ROS_DISTRO --cmake-args -DCMAKE_BUILD_TYPE=Release
```
Clone the repositories
```
cd src/
git clone http://github.com/discoimp/rpg_ultimate_slam_open.git
vcs-import < rpg_ultimate_slam_open/dependencies.yaml
```
Build the workspace
```
catkin build ze_vio_ceres
```
Source your installation
```
source ~/uslam_ws/devel/setup.bash

# Optionally automatically source in every new termninal session:
echo "source ~/uslam_ws/devel/setup.bash" >> ~/.bashrc
```

### Install resources
(Assumes ~/catkin_ws/src exists from Event Camera Driver)

```
cd ~/catkin_ws/src/
git clone https://github.com/discoimp/blue-rov2-noetic-interface.git
curl -o ~/catkin_ws/blue-rov2-noetic-interface/resources/vhuit64 -LO https://www.virtualhere.com/sites/default/files/usbclient/vhuit64
curl -o ~/catkin_ws/blue-rov2-noetic-interface/resources/QGroundControl.AppImage -LO https://d176tv9ibo4jno.cloudfront.net/latest/QGroundControl.AppImage
```



