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
    + OpenCV 3.4.1
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
#### ● Uploaded folders for following setup: D435i, pixhawk4 mini 
#### ● for using your own sensor setup, you have to get a calibration data using [kalibr](https://github.com/zinuok/kalibr)
<br><br>

## 1. Prerequisites
### ● Eigen
+ ~Eigen from [here](http://eigen.tuxfamily.org/index.php?title=Main_Page)~ 
=> This link is not avaiable (2020.09.03). Download eigen-3.3.7.zip from [here](http://eigen.tuxfamily.org/index.php?title=Main_Page)
```
$ wget -O eigen.zip http://bitbucket.org/eigen/eigen/get/3.3.7.zip # <= do not download from this. 
$ unzip eigen-3.3.7.zip
$ cd ~/eigen-3.3.7 && mkdir build && build
$ cmake ../ && sudo make install -j $(nproc)
```
### ● Ceres solver
+ Ceres solver from [here](http://ceres-solver.org/installation.html)
```
$ sudo apt-get install -y cmake libgoogle-glog-dev libatlas-base-dev libsuitesparse-dev
$ wget http://ceres-solver.org/ceres-solver-1.14.0.tar.gz
$ tar zxf ceres-solver-1.14.0.tar.gz
$ cd ceres-solver-1.14.0
$ mkdir build && cd build
$ cmake -DEXPORT_BUILD_DIR=ON \
        -DCMAKE_INSTALL_PREFIX=/usr/local \
        ../
$ make -j $(nproc) # number of cores
$ make test -j $(nproc)
$ sudo make install -j $(nproc)
```

### ● cv_bridge
+ if you built OpenCV manually (refer [here](https://github.com/zinuok/Xavier_NX#1-opencv-ver-341-install)), you should also build 'cv_bridge' manually from source.
+ Download from proper branch in [here](https://github.com/ros-perception/vision_opencv/tree/melodic) according to your ROS version. For example,
```
$ cd ~/catkin_ws/src
$ git clone -b melodic https://github.com/ros-perception/vision_opencv.git
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
+ GPU version: for more information, refer [engcang](https://github.com/engcang/vins-application#-opencv-with-cuda--necessary-for-gpu-version-1)

+ download
```
$ cd ~/catkin_ws/src
$ git clone https://github.com/pjrambo/VINS-Fusion-gpu.git
```

+ edit following CMakeLists.txt (from [engcang](https://github.com/engcang/vins-application#-opencv-with-cuda--necessary-for-gpu-version-1))
    1) ~/VINS-Mono/loop_fusion/CMakeLists.txt : line 19 <br>
        #find_package(OpenCV) <br>
        => include(/usr/local/share/OpenCV/OpenCVConfig.cmake) <br><br>

    2) ~/VINS-Mono/vins_estimator/CMakeLists.tx: line 20 <br>
        #find_package(OpenCV REQUIRED) <br>
        => include(/usr/local/share/OpenCV/OpenCVConfig.cmake) <br><br>


+ catkin build
```
$ cd ~/catkin_ws/
$ catkin build -DCMAKE_BUILDTYPE=Release -j $(nproc)
$ source ~/catkin_ws/devel/setup.bash
```
<br><br>

## 3. Jetson Boards
#### ● Actually, no installation difference among TX2, Xavier, and NX
<br><br>

## 4. Run
#### ● Uploaded folders for following setup: D435i, pixhawk4 mini 
#### ● for using your own sensor setup, you have to get a calibration data using [kalibr](https://github.com/zinuok/kalibr)
```
$ roslaunch realsense2_camera rs_camera.launch
$ roslaunch mavros px4.launch
$ rosrun vins vins_node [path of realsense_stereo_imu_config.yaml]
$ rosrun loop_fusion loop_fusion_node [path of realsense_stereo_imu_config.yaml]
$ roslaunch vins vins_rviz.launch
```

