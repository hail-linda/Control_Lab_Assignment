# Control_Lab_Assignment

The last assignment of a HEU course called control-lab.  
HEU control-lab 课程作业  
@李昕达

----

作业包括三个部分，分别对应本课程的三个研究对象：

> 云台：特征点，区域提取  
> 飞行器（四旋翼）：控制算法仿真  
> 机械臂：多级机械臂联动路径规划

全文链接：https://github.com/hail-linda/Control_Lab_Assignment/blob/master/control-lab%20%E8%AF%BE%E7%A8%8B%E4%BD%9C%E4%B8%9A-%E6%9D%8E%E6%98%95%E8%BE%BE.pdf

## 云台

预估难度：★★☆☆☆

在云台作业中，常常需要对某些人、物进行跟踪，那么通过图像处理的方法对目标进行特征点提取，分析，定位就是云台进行何种运动的指导方法。本文这一部分就几种特征提取与特征匹配算法进行基于个人理解的描述，并在文末给出一种算法的使用python-opencv库的具体实现。 

TODOs:

- 一些术语、关键词的介绍
- 几个常用算法的介绍与对比
     - SIFT<sup>[1-2]</sup>
     - SURF<sup>[1-2]</sup>
     - ORB<sup>[1-2]</sup>
- 某种算法的opencv-python实现与展示


## 飞行器（四轴飞行器）

预估难度：★★★☆☆

在作业中以四旋翼为对象的部分，考虑使用matlab-simulink进行四旋翼飞行器的运动仿真，进而进行控制仿真。  

TODOs:

- 对matlab-simulinuk的学习  
- 找一个现成的库或者工程<sup>[4-5]</sup>  
- 对其中各个部分/参数的理解、调整，并尝试搞清楚他们的影响

## 机械臂

预估难度：★★★★★

对于以机械臂为主题的相关的学习，考虑研究一种路径规划方法并加以仿真。在多级机械臂运动问题中，从一种姿态到另一种姿态，从一个位置到另一个位置需要一定的时间，在机械部分性能无差别的前提下（各级旋转速度恒定且合理），如果想要耗时最短，就需要使得各级机械臂旋转角度最大值最小。  
基于这种解决方案，考虑找到（开发）一种算法能够在给定以下参数的条件下给出一组参数，这组参数表示各级电机需要转动的方向与角度。

- 起始时间各臂相对参数，如下表

| 臂序号 | 臂长度（mm） | 臂转轴平面法线(矢量指向) | 臂初始指向角度（度/°） |
| :------: | :------: | :------: | :------: |
| 0 | 100 | (1,1,1) | 30 |
| 1 | 200 | (3,2,4) | -45 |
| …… | …… | …… | …… |

- 要求达到的目标坐标与指向 

TODOs:

- 数据成功读入的反馈  
- 能否在给定数据下成功运动的判断（可行性）  
- 得到正确计算结果  
- 可视化仿真  

项目地址：https://github.com/hail-linda/Control_Lab_Assignment

## references:

[1] https://blog.csdn.net/keen_zuxwang/article/details/72817302  
[2] https://zhuanlan.zhihu.com/p/32084529  
[3] https://www.youtube.com/watch?v=fpJZSQmqDVk  
[4] https://github.com/enu89/quadrotor  
[5] https://github.com/clausqr/qrsim2  








