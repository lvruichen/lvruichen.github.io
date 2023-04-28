---
title: 动态场景下的SLAM一些问题和解决方案
tags: SLAM
---

针对城市环境下的SLAM如何剔除动态物体

<!--more-->

#### DynamicFilter: an Online Dynamic Objects Removal Framework for Highly Dynamic Environments
**论文框图**

![](/assets/dynamic_slam/dynamicFilter.png)
**简要介绍**

本文是发布在2023年ICRA上的一篇文章，论文中主要的贡献在于提出了一种在线式的动态环境建图方案。其主要的核心思想就是前后端的思想，根据slam前端对实时性的要求，将动态滤除方法也做了前后端的区分。

其中，前端部分接受一系列由slam定位得到的scan和相应的pose，他们之间根据visibility-based removal的方法将其粗略的分为动态点云和静态点云。之后利用局部子图对动态点云中误杀的点云做revert，主要的思路就是利用pca，之后保存为一个子图。

后端就是对比多个子图，根据子图之间的差异来进一步滤除动态点云。其中后端滤除的部分提出了一个伪ray-tracing的方法，能够做到以较少资源消耗的情况下达到ray-tracing的效果。



