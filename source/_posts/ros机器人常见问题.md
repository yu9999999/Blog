---
title: ros2机器人常见问题以及一些使用方法
date: 2023-10-19
tags:
- ROS
categories:
- ROS与机器人相关问题
aside: false
---

# 问题

## 1. ros2build出错：SetuptoolsDeprecationWarning: setup.py install is deprecated. Use build and pip and other...

办法：该问题为版本问题，将python版本降低为58.2.0即可
``` bash
$ sudo pip install -i https://pypi.tuna.tsinghua.edu.cn/simple setuptools==58.2.0 --upgrade
```

备注：可以通过以下命令查看python3版本：
``` bash
$ python3
$ import setuptools
$ print(setuptools.__version__)
```

## 2. CMake Error: The source directory "文件位置" does not exist.

因为前面编译了该文件，后来将其删除后，没有将build中的记录删除，删除即可

## 3. No such file or directory

缺少该文件，否则查看文件名字可能与库不一致，仔细检查

## 4. rosdepc update失败

``` bash
$ sudo rosdepc init
$ rosdepc update
```

# 使用方法

## 1. 依赖问题：可以使用rosdepc进行依赖的安装，然后对依赖中的版本进行无视（鱼香ros真的很好用）

``` bash
$ wget http://fishros.com/install -O fishros && . fishros
$ rosdepc install -r --from-paths src --ignore-src --rosdistro $ROS_DISTRO -y
```
## 2. 切换teb算法

首先下载源码：可以下载压缩文件，也可以git。teb有的需要costmap_converter文件，所以都下载
然后安装依赖：`rosdep install -i --from-path src --rosdistro galactic -y`（用什么版本就换什么，比如这里用galactic）
最后修改yaml文件替换dwb插件，编译，测试

## 3. ros2自定义msg

(1)创建msg文件,修改cmake，修改package
(2)订阅话题，可以直接将话题数据拿去用，也可以赋值给私有变量x，x再拿去做运算
``` bash
public:
  example_subscribe_ = create_subscription<xxx::msg::Example>("example", 10, std::bind(&类名::examplesubscribecallback, this, std::placeholders::_1));
private:
  double x;
  rclcpp::Subscription<nav2_msgs::msg::Person>::SharedPtr example_subscribe_;
  void examplebscribecallback(const xxx::msg::Example::SharedPtr msg)
  {
    x=msg->x;
  }
```
注意：头文件：#include "xxx/msg/example.hpp"，必须小写，如果是ExampleExample这种命名的，头文件#include "xxx/msg/example_example.hpp"这么写

## 4. ros2手动发布目标点

``` bash
$ ros2 action send_goal /navigate_to_pose nav2_msgs/action/NavigateToPose "{pose: {header: {frame_id: 'map'},pose: {position: {x: 0.0,y: 0.0},orientation: {w: 1.0}}}}"
```

## 5. rqt中话题使用

rqt->plugins->topic->message publisher->选择所需话题和消息类型->右上角点击+号->勾选

## 6. 搭建ros仿真环境

（1）gazebo->Edit->Building Editor->绘制好墙->保存->退出->添加物品->保存至world文件夹
（2）修改gazebo的launch文件中的新建图world名字->启动gazebo的launch->启动cartographer的launch，->启动遥控`ros2 run teleop_twist_keyboard teleop_twist_keyboard` ->走完先暂停再地图后保存地图
（3）保存地图前安装`sudo apt install ros-humble-nav2-map-server`->再保存地图:
``` bash
$ cd src/cartographer目录/ && mkdir map && cd map（如果有这个目录直接进入该目录，然后运行下面的程序）
$ ros2 run nav2_map_server map_saver_cli -t map -f 文件名
```
