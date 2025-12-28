# FAST-LIO converter

## Example Dataset: 

Download the dataset from [Bunker DVI Dataset](https://charleshamesse.github.io/bunker-dvi-dataset/)  

## Intended use 

This small toolset allows to integrate SLAM solution provided by [fast-lio](https://github.com/hku-mars/FAST_LIO.git) with [HDMapping](https://github.com/MapsHD/HDMapping).
This repository contains ROS 1 workspace that :
  - submodule to tested revision of FAST-LIO
  - a converter that listens to topics advertised from odometry node and save data in format compatible with HDMapping.

## Building

Clone the repo
```shell
mkdir -p /test_ws/src
cd /test_ws/src
git clone https://github.com/marcinmatecki/FAST-LIO-to-HDMapping.git --recursive
cd ..
catkin_make
```

## Dependencies

```shell
sudo apt install -y nlohmann-json3-dev
```

## Usage - data SLAM:

Prepare recorded bag with estimated odometry:

In first terminal record bag:
```shell
rosbag record /cloud_registered /Odometry
```

and start odometry:
```shell 
cd /test_ws/
source ./devel/setup.sh # adjust to used shell
For launch, check https://github.com/hku-mars/FAST_LIO/
rosbag play your bag file
```

## Usage - conversion:

```shell
cd /test_ws/
source ./devel/setup.sh # adjust to used shell
rosrun fast-lio-to-hdmapping listener <recorded_bag> <output_dir>
```

## Record the bag file:

```shell
rosbag record /cloud_registered /Odometry -O {your_directory_for_the_recorded_bag}
```


## Modify 
```shell 
  src/FAST-LIO-to-HDMapping/src/FAST_LIO/config/ouster64.yaml

  lid_topic: "/os_cloud_node/points"
  imu_topic: "/os_cloud_node/imu"

  to:
  
  lid_topic: "/os1_cloud_node1/points"
  imu_topic: "/os1_cloud_node1/imu"
```

## FAST-LIO Launch:

```shell
cd /test_ws/
source ./devel/setup.sh # adjust to used shell
roslaunch fast_lio mapping_ouster64.launch 
rosbag play your bag file
```

## During the record (if you want to stop recording earlier) / after finishing the bag:

```shell
In the terminal where the ros record is, interrupt the recording by CTRL+C
Do it also in ros launch terminal by CTRL+C.
```

## Usage - Conversion (ROS bag to HDMapping, after recording stops):

```shell
cd /test_ws/
source ./devel/setup.sh # adjust to used shell
rosrun fast-lio-to-hdmapping listener <recorded_bag> <output_dir>
```
