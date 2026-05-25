docker compose build
docker compose up -d
docker exec -it ros2_humble_dev bash

通信测试
容器内
source /opt/ros/humble/setup.bash
ros2 run demo_nodes_cpp talker
再开一个窗口
容器内
source /opt/ros/humble/setup.bash
ros2 run demo_nodes_cpp listener

测试 RViz
xhost +local:docker
docker exec -it ros2_humble_dev bash
echo $DISPLAY
rviz2

创建 workspace
容器内
cd /workspace
colcon build
source install/setup.bash


创建 ROS2 package
cd /workspace/src

ros2 pkg create \
  --build-type ament_cmake \
  my_pkg

cd /workspace

colcon build


10. 摄像头支持

后面如果你接：

USB Camera
RealSense
CSI Camera

compose 加：

devices:
  - /dev/video0:/dev/video0

或者：

privileged: true

（开发期方便）


11. NVIDIA GPU 支持（x86）

如果你有 NVIDIA GPU：

先安装：

NVIDIA Container Toolkit

compose 增加：

deploy:
  resources:
    reservations:
      devices:
        - driver: nvidia
          count: all
          capabilities: [gpu]


12. Jetson 特殊情况

如果你之后迁移到：

NVIDIA Jetson

不要直接用：

FROM ros:humble

而要：

FROM nvcr.io/nvidia/l4t-ros2:r35.x.x

因为：

CUDA
TensorRT
GStreamer
nvargus

都和 L4T 强绑定。



13. CycloneDDS（推荐）

ROS2 默认 DDS 有时不稳定。

推荐：

sudo apt install ros-humble-rmw-cyclonedds-cpp

然后：

export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp

很多团队默认就是 CycloneDDS。

14. 后面还能继续扩展

这个架构后面还能加：

Gazebo
Nav2
SLAM Toolbox
Foxglove
Isaac ROS
CUDA inference
多容器 ROS graph
DDS tuning
Zero-copy transport
推荐下一步

建议你下一步直接做：

A. 最小 Publisher/Subscriber

真正理解：

node
topic
QoS
executor
B. 学 colcon + ament

这是 ROS2 核心。

C. DDS 网络机制

因为 ROS2 最大坑基本都在 DDS。

D. 组件化（Component）

ROS2 真正强的地方。