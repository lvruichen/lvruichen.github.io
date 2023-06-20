---
title: Remove then Revert
tags: slam
---
static point cloud map construction using multiresolution range images
<!--more-->

## 算法流程图
![](/assets/removert/frame.png)
removert算法的思路很简单，就是将地图和scan都转换成rangeMat，然后根据每个rangeMat像素的区别判断点云是动态点云还是静态点云。

其中主要的模块是BR(batch removal)，一开始的BR选取分辨率较高的rangeMat，这样可以保证动态点云应杀尽杀，后续用低分辨率的rangeMat恢复误杀点云。

经过尝试，发现算法对于参数很敏感，对于不同的降采样率，不同的rangeMat分辨率，算法的性能会下降的比较厉害。

## 算法效果图
![](/assets/removert/res.png)

## 复现主要流程
1. 原始代码没有revert模块
2. 点云支持局部子图处理模式
3. 可以支持kitti格式以及其他格式的数据
4. 可以保存cleaned_scan，static_submap，dynamic_submap。