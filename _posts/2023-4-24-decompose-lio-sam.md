---
title: lio-sam优化和解耦
tags: slam
---

将lio-sam前端特征提取利用多线程优化，将各个模块以及前端后端解耦

<!--more-->

**原始的lio-sam由以下四个文件构成：**

imageProjection.cpp
负责点云去畸变和点云投影和按线数分割，以及点云的初值

featureExtraction.cpp
负责点云的特征提取，主要就是根据曲率判断角点和平面点

imuPreintegration.cpp
负责利用原始imu数据和雷达里程计数据，构建因子图输出高频imu预积分里程计

mapOptimization.cpp
负责利用带有初始位姿的特征点云做LM算法，以及将关键帧加入因子图，以及回环和可视化线程。

![](/assets/lio_sam_decompose/lio_sam.png)

**解耦后的lio-sam由两个文件构成**
front_end.cpp
负责前端点云特征提取和imu预积分
前端利用多线程提取点云特征，imu预积分线程负责输出高频预积分里程计

back_end.cpp
后端模块负责点云LM算法和因子图优化，以及回环线程和可视化线程。所有的关键帧保存由一个新的类mapManager管理，后续可根据mapManager做二次开发。

解耦后的坐标系也采用新的方式，低频的雷达里程计维护map到base_link的变换，高频的imu维护odom到base_link的变换。因此，解耦后的map到odom是低频的，odom到base_link是高频的。这样可以缓解建图过程中里程计跳变的情况。

![](/assets/lio_sam_decompose/lio_sam_decompose.png)

[仓库地址](https://github.com/lvruichen/Ground-LIO-SAM)