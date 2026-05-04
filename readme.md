# FastLIO+PX4 单航点飞行节点

## 功能说明
使用FastLIO输出的里程计作为定位源，通过MAVROS控制PX4飞控实现无GPS单航点飞行。

## 前提条件
1. ROS Noetic/Melodic已安装
2. MAVROS已安装并配置完成
3. FastLIO已成功运行，且稳定发布`/Odometry`话题（nav_msgs/Odometry类型，ENU坐标系）
4. 已完成FastLIO坐标系与PX4机体坐标系的外参标定

## 编译方法
```bash
cd ~/catkin_ws/src
# 将本文件夹复制到此处
cd ~/catkin_ws
catkin_make
source devel/setup.bash


