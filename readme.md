# FastLIO+PX4 单航点飞行节点

## 功能说明
使用FastLIO输出的里程计作为定位源，通过MAVROS控制PX4飞控实现无GPS单航点飞行。

## 前提条件
1. ROS Noetic/Melodic已安装
2. MAVROS已安装并配置完成
3. FastLIO已成功运行，且稳定发布`/Odometry`话题（nav_msgs/Odometry类型，ENU坐标系）
4. 已完成FastLIO坐标系与PX4机体坐标系的外参标定


## 启动流程
雷达启动（见mid360机载电脑配置.md）
要求正确输出/Odometry

1. 启动mavros
roslaunch mavros px4.launch fcu_url:="udp://:14540@127.0.0.1:14557"

2. 启动飞行逻辑节点
source devel/setup.bash
roslaunch fastlio_px4_waypoint single_waypoint.launch

## 原理说明
对于飞行代码只需要/Odometry的信息即可，需要保障这个话题的数据没有问题（我这里不保证这个话题是否正确，这个雷达负责），
此时代码就会订阅这个话题信息来定位。

然后程序会计算当前位置和目标位置并发送话题/mavros/setpoint_position/local给mavros，
然后mavros会以MAVLink协议格式发给飞控，飞控执行。（具体mavlink是啥格式，飞控咋读取的这段信息并如何飞行，请飞控完成，我这只保证通信没有问题）。

飞机到达航点（附近，定航点设置是0.2m），代码会给飞控发布降落指令（px4的降落和我无关，发送指令它自己就落了）
若未到达目标，则持续向 /mavros/setpoint_position/local 话题发布位置设定点。


