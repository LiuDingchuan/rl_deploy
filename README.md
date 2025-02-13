# rl_deploy
> Use ros_control to realize sim2sim in gazebo and sim2real

>Many thanks to DirectDriveTech-DDT[https://github.com/DirectDriveTech-DDT]  for the guidance and hardware provided for this project

### Frame

- `rl_sdk` mainly refers to https://github.com/fan-ziqi/rl_sar and uses `libtorch` to complete the loading of .pt files.

- `rl_msgs` ：For interaction between rl_sdk and rl_controller
-  `wheeled_bipedal_rl_controller`：use ros_control to interact with gazebo



#### Install

- ros_control

```python
sudo apt install ros-noetic-teleop-twist-keyboard ros-noetic-controller-interface  ros-noetic-gazebo-ros-control ros-noetic-joint-state-controller ros-noetic-effort-controllers ros-noetic-joint-trajectory-controller
sudo apt-get install ros-noetic-imu*
```

- libtorch

```python
wget https://download.pytorch.org/libtorch/cpu/libtorch-cxx11-abi-shared-with-deps-2.0.1%2Bcpu.zip
unzip libtorch-cxx11-abi-shared-with-deps-2.0.1+cpu.zip -d ./
echo 'export Torch_DIR=/path/to/your/torchlib' >> ~/.bashrc
```

- yaml-cpp

方法1：
```
sudo apt install liblcm-dev libyaml-cpp-dev
```
方法2：

yaml-cpp:
```python
cd src
git clone https://github.com/jbeder/yaml-cpp.git
cd yaml-cpp && mkdir build && cd build
cmake -DYAML_BUILD_SHARED_LIBS=on .. && make
sudo make install
sudo ldconfig
```
lcm:
```
cd src
git clone https://github.com/lcm-proj/lcm.git
cd lcm && mkdir build && cd build
cmake .. && make
sudo make install
sudo ldconfig
```
- imu_tools(如果有报这方面的错误)

```
cd /src
git clone https://github.com/ccny-ros-pkg/imu_tools.git
```

- bugfix

```
cd src/rl_deploy/rl_msgs
mkdir srv
```

#### Run

```
source devel/setup.bash
roslaunch diablo load_gazebo.launch
roslaunch diablo load_controllers.launch
roslaunch diablo load_rl_interface.launch
```

#### Reference code
> Thank very much for these excellent open source projects！

- Very good sim2sim code reference：
```
https://github.com/fan-ziqi/rl_sar
```
- Top ros_control use cases
```aiignore
https://github.com/qiayuanl/legged_control
https://github.com/rm-controls/rm_control
```

#### 部署tips

1. 通过导入.pt文件来实现强化策略的切换，导入后改动config.yaml文件中的model_name即可
2. diablo_hw 文件夹中的 hw 表明该模块专注于 硬件接口和控制，在整个工程中扮演着连接软件系统与实际硬件设备的桥梁角色