# ct-lio
CT-LIO: Continuous-Time LiDAR-Inertial Odometry

**ct-lio** (Continuous-Time LiDAR-Inertial Odometry) is an accurate and robust LiDAR-inertial odometry (LIO). It fuses LiDAR constraints(ct-icp) with IMU data using ESKF(loose couple) to allow robost localizate in fast moton (as lio-sam). Besides, provide analytical derivation and automatic derivation for ct-icp.

<img src="doc/road.gif" /> 

### Some test results are show below:

#### Velodyne 32, NCLT dataset
(mode:normal + eskf)
<img src="doc/nclt.gif" /> 

#### Ouster-32, multi-layer office 
**Left**: ours (mode:normal + eskf)

**Right**: fast-lio2

<img src="doc/m-office.gif" /> 


#### Robosense RS16, staircase_crazy_rotation dataset
**Left**: PV_LIO

**Right**: ours (mode:normal + eskf)

<img src="doc/mm-layer.gif"/> 


#### Velodyne 16, LIO-SAM dataset
**Left**: ours (mode:normal + eskf)

**Right**: direct_lidar_inertial_odometry

<img src="doc/compare_with_dlio.png" /> 

#### Velodyne 16, LIO-SAM dataset

(mode:CT + eskf)
<img src="doc/casual_walk.png" /> 

## 1. Prerequisites

### 1.1 **Ubuntu** and **ROS**
**Ubuntu >= 18.04**

For **Ubuntu 18.04 or higher**, the **default** PCL and Eigen is enough for PV-LIO to work normally.

ROS    >= Melodic. [ROS Installation](http://wiki.ros.org/ROS/Installation)

### 1.2. **PCL && Eigen**
PCL    >= 1.8,   Follow [PCL Installation](http://www.pointclouds.org/downloads/linux.html).

Eigen  >= 3.3.4, Follow [Eigen Installation](http://eigen.tuxfamily.org/index.php?title=Main_Page).


## 2. Build

Clone the repository and catkin_make:

**NOTE**:**[This is import]** before catkin_make, make sure your dependency is right(you can change in ./cmake/packages.cmake)

```
    cd ~/$A_ROS_DIR$/src
    git clone https://github.com/chengwei0427/ct-lio.git
    cd ct_lio
    cd ../..
    catkin_make
    source devel/setup.bash
```

- If you want to use a custom build of PCL, add the following line to ~/.bashrc
```export PCL_ROOT={CUSTOM_PCL_PATH}```
  
## 3. Directly run
Noted:

A. Please make sure the IMU and LiDAR are **Synchronized**, that's important.

B. The warning message "Failed to find match for field 'time'." means the timestamps of each LiDAR points are missed in the rosbag file. That is important for the forward propagation and backwark propagation.

C. Before run with **NCLT** dataset, you should change time-scale in **cloud_convert.cpp**( static double tm_scale = 1e6)


## 4. Rosbag Example
### 4.1 Robosense 16 Rosbag 

<div align="left">
<img src="doc/staircase.png" width=95% />
</div>

Files: Can be downloaded from [Baidu Pan (password:4kpf)](https://pan.baidu.com/s/1VHIVYo2LAyFKzMzdilOZlQ) or [Google Drive](https://drive.google.com/drive/folders/1f-VQOORs1TA5pT-OO_7-rG0kW5F5UoGG?usp=sharing)

**Noted**: For this narrow staircases, should adjust the params(such as surf_res etc.) before run the program.

Run:
```
roslaunch sr_lio run_eskf.launch
cd YOUR_BAG_DOWNLOADED_PATH
rosbag play *
```

## Related Works
1. [ct_icp](https://github.com/jedeschaud/ct_icp):  Continuous-Time LiDAR Odometry .
2. [slam_in_autonomous_driving](https://github.com/gaoxiang12/slam_in_autonomous_driving): SLAM in Autonomous Driving book
3. [semi_elastic_lio](https://github.com/ZikangYuan/semi_elastic_lio): Semi-Elastic LiDAR-Inertial Odometry.



## Acknowledgments

