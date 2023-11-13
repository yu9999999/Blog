---
title: dwa算法的理解
date: 2023-10-31
tags:
- DAW
- 局部路径规划算法
- ROS
- 机器人
categories:
- 机器人路径规划算法
aside: false
---

# dwa算法流程：
## step1:计算速度样本
在计算速度样本过程中，先通过速度约束（速度限制，加速度约束，障碍物约束）等生成一个速度范围，也就是所有限制做个交集；然后将速度分成几份。eg：线速度为[0,1],角速度为[0,1]，都将速度分为11份；那么组合成的速度样本有[0,0],[0,0.1],[0,0.2],……,[1,0.9],[1,1]。在nav2源码中vx_samples、vy_samples、vtheta_samples、linear_granularity、angular_granularity这几个参数都是为了划分速度样本。
## step2:对每个样本速度生成轨迹
通过s=vt可以计算路径，如果是二维且速度都不为0的情形下会是一条弧线，这个自己去看运动学，很好理解，比如：线速度为0m/s，角速度为1m/s，机器人原地旋转。
## step3:对每一条生成轨迹进行评分，选择最优路径
评分主要是对目标点、路径、障碍物进行评分，当然可以加入其他评分标准。目标点主要是为了让机器人向前移动；路径是为了让机器人移动轨迹与规划路线更贴合，也就是让机器人跟着规划路线走；障碍物就是为了避障。nav2相关参数BaseObstacle.scale、PathAlign.scale、PathAlign.forward_point_distance、GoalAlign.scale、GoalAlign.scale、forward_point_distance、PathDist.scale、GoalDist.scale、RotateToGoal.scale、RotateToGoal.slowing_factor、RotateToGoal.lookahead_time。
## step4:发布局部规划和代价图，并返回best对应的速度
这里是评分越低的越好，权重越高影响越大。然后分数最低的轨迹对应的速度发布出来
## step5：将step4中的速度发布到控制
在nav2中dwa算法生成的轨迹发布到nav2_controller文件，也可以将dwa替换成teb等局部规划算法。

## 注意事项
1.当设置目标点容差过低时，机器人会有抖动现象，那是因为机器人越靠近目标点，机器人就会一直减速，这跟硬件有关，好点的硬件是看不出来啥的，如果硬件没办法，自己也可以增加评分器进行优化
2.sim_time是仿真时间，你可以当作用来模拟轨迹评分