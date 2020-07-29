# VINS-Fusion
+ VINS-Fusion setup for following nvidia boards
    + jetson TX2 - Jetpack 4.2
    + jetson Xavier NX - Jetpack 4.4
+ version info
    + Ubuntu: 18.04 
    + ROS: Melodic 
+ github link: [here](https://github.com/HKUST-Aerial-Robotics/VINS-Mono)
<br>

# Index
### 1. Prerequisites
#### ● [Ceres solver and Eigen] (build Eigen first)
### 2. Installation
### 3. TX2, NX
#### ● Actually, there is no installation difference between TX2 and NX
<br><br>

# 1. Prerequisites
### ● Ceres solver and Eigen (mandatory)
+ Eigen from [here](http://eigen.tuxfamily.org/index.php?title=Main_Page)
```
$ wget -O eigen.zip http://bitbucket.org/eigen/eigen/get/3.3.7.zip #check version
$ unzip eigen.zip
$ mkdir eigen-build && cd eigen-build
$ cmake ../eigen_source_folder_name && sudo make install
```

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

# 2. Installation
+ git clone and build from source
```
$ cd ~/catkin_ws/src
$ git clone https://github.com/HKUST-Aerial-Robotics/VINS-Mono.git
$ cd ../ && catkin build -DCMAKE_BUILDTYPE=Release -j3
$ source ~/catkin_ws/devel/setup.bash
```
<br><br>

### 3. TX2, NX
#### ● Actually, there is no installation difference between TX2 and NX

