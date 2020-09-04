***
# VINS-Fusion (CPU & GPU version)
+ **hardware setup**
    + jetson TX2 - Jetpack 4.2
    + jetson AGX Xavier
    + jetson Xavier NX - Jetpack 4.4
    + realsense D435i (color, infra1, infra2)
    + pixhawk4 mini
    <br>
+ **software setup**
    + Ubuntu: 18.04 
    + ROS: Melodic 
    <br>
+ **CPU ver. github link**: [HKUST-Aerial-Robotics](https://github.com/HKUST-Aerial-Robotics/VINS-Fusion)
+ **GPU ver. github link**: [HKUST-Aerial-Robotics](https://github.com/pjrambo/VINS-Fusion-gpu)
***
<br>

# Index
### 1. Prerequisites
####    &nbsp;&nbsp;&nbsp;&nbsp;● Eigen
####    &nbsp;&nbsp;&nbsp;&nbsp;● Ceres solver
### 2. Install
### 3. Jetson Boards
####    &nbsp;&nbsp;&nbsp;&nbsp;● Actually, there is no installation difference among TX2, Xavier, and NX
### 4. Run
<br><br>

## 1. Prerequisites
### ● Eigen
+ Eigen from [here](http://eigen.tuxfamily.org/index.php?title=Main_Page)
```
$ wget -O eigen.zip http://bitbucket.org/eigen/eigen/get/3.3.7.zip #check version
$ unzip eigen.zip
$ mkdir eigen-build && cd eigen-build
$ cmake ../eigen_source_folder_name && sudo make install
```
### ● Ceres solver
+ Ceres solver from [here](http://ceres-solver.org/installation.html)
```
$ sudo apt-get install -y cmake libgoogle-glog-dev libatlas-base-dev libsuitesparse-dev
$ wget http://ceres-solver.org/ceres-solver-1.14.0.tar.gz
$ tar zxf ceres-solver-1.14.0.tar.gz
$ mkdir ceres-bin
$ mkdir solver && cd ceres-bin
$ cmake ../ceres-solver-1.14.0 -DEXPORT_BUILD_DIR=ON -DCMAKE_INSTALL_PREFIX="../solver"  #good for build without being root privileged and at wanted directory
$ make -j8 # 8 : number of cores
$ make test
$ make install
```
<br><br>

## 2. Install
+ CPU version: git clone and build from source
```
$ cd ~/catkin_ws/src
$ git clone https://github.com/HKUST-Aerial-Robotics/VINS-Fusion.git
$ cd ../ && catkin build -DCMAKE_BUILDTYPE=Release -j3
$ source ~/catkin_ws/devel/setup.bash
```
+ GPU version: to use GPU version, you need to build **cv_bridge**. For ROS melodic, download from [here](https://github.com/ros-perception/vision_opencv/tree/melodic) and do catkin build. For more information, refer [engcang](https://github.com/engcang/vins-application#-opencv-with-cuda--necessary-for-gpu-version-1)
<br><br>

## 3. Jetson Boards
#### ● Actually, no installation difference among TX2, Xavier, and NX
<br><br>

## 4. Run
#### ● you have to get a calibration data using [kalibr](https://github.com/zinuok/kalibr)
```
$ roslaunch realsense2_camera rs_camera.launch
$ roslaunch mavros px4.launch
$ rosrun vins vins_node [path of realsense_stereo_imu_config.yaml]
$ rosrun loop_fusion loop_fusion_node [path of realsense_stereo_imu_config.yaml]
$ roslaunch vins vins_rviz.launch
```

