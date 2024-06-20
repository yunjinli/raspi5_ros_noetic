# Install ROS Noetic from Source on Raspberry Pi 5 (Mantic)
## Introduction
One of my company project needs to use ROS with Raspberry Pi 5, as I already have all the ROS packages implemented in ROS noetic, I don't wanna spend too much time to migrate from ROS1 to ROS2 (because only ubuntu 24.04 and 23.10 are supported for raspi5), so I decided to dig into this topic and finally make all my previous ROS packages work off-the-shelf on rapi5. Many thanks for these tutorials ([1](https://medium.com/@ybrama_30738/install-ros-noetic-desktop-from-source-on-ubuntu-22-04-f540d158fbc5), [2](https://github.com/lucasw/ros_from_src), [3](https://github.com/ros/rosconsole/pull/54)).

## Step-by-Step Installation
```
sudo apt-get update
sudo apt-get install python3 cmake build-essential
sudo apt-get install git python3-rosinstall-generator

mkdir ~/catkin_ws
cd catkin_ws
rosinstall_generator desktop --rosdistro noetic --deps --tar > noetic-desktop.rosinstall
mkdir src
cd src
```
Copy git_clone.sh and install_apt_dep.sh into ~/catkin_ws/src
```
bash git_clone.sh
bash install_apt_dep.sh
```
Copy [ros_comm.patch](./ros_comm.patch) into ~/catkin_ws/src/ros_comm
```
cd ~/catkin_ws/src/ros_comm
git apply --ignore-whitespace ros_comm.patch
```
Now we're all set, start the build process.
```
cd ~/catkin_ws
./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release
```
Once the building is done
```
cd install_isolated
source setup.bash
roscore
```
You should be able to see the roscore launch successfully.
```
```
Automatically source this script every time a new shell is launched by adding this line to ~/.bashrc
```
echo "source ~/catkin_ws/install_isolated/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
Now you are all set :)
