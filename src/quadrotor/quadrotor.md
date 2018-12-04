# 四轴飞行器


### 对matlab-simulink的简单学习<sup>[1]</sup>

通过Mathwork提供的simulink教程来进行入门的学习:

示例仿真了在踩下汽车踏板后简化的汽车运动

#### 简化的汽车运动

该运动考虑以下三个问题:

- 汽车在到达障碍物时会紧急刹车。
- 在现实世界中，传感器对距离的测量不够精确，从而导致随机数值误差。
- 数字传感器以固定时间间隔运行。

下图为仿真结果图

![](./simu.png)

仿真描绘了在汽车踩下踏板后的运动(只踩了一下且不考虑摩擦),使用了脉冲信号,放大环节,积分环节并引入了噪声,最后进行可视化输出.

图中示波器红线表示汽车与障碍物的实际距离,棕线表示传感器的测量值.
可出传感器的测量不够精确导致的随机误差,并且传感器的采样距离0.1s.

#### 模拟一个简单的物块

对一受重力的物块施加力,从某高度移动到某高度,使用PID控制
模拟一个目标高度随时间不规则变化的过程  
通过加误差模拟目标高度的变化  

一些参数:

$
g=9.8m/s^2 \quad  
k=100PID1400N/m \quad  
目标高度:1m \quad  
初始高度:0m \quad  
物重:500g \quad  
$

仿真图与结果

![](/home/linda/Ctrl-lab/Control_Lab_Assignment/src/quadrotor/simu1.png)

$
KP=40\quad  
KI=2\quad  
KD=15\quad  
$

### 找一个现成的库或者工程,为可能的任务进行参数调整<sup>[2]</sup>

在github找到了一个开源的四轴飞控

[https://github.com/yrlu/quadrotor)

![](/home/linda/Ctrl-lab/Control_Lab_Assignment/src/quadrotor/simu4.png)

工程没有使用simulink,而是全部使用\*.m文件
通过阅读代码,分析各个部分作用,找到路径文件,PID控制文件

按照例程的格式新建一个路径文件并进行仿真

![](/home/linda/Ctrl-lab/Control_Lab_Assignment/src/quadrotor/simu3.png)



## reference:
[1]  https://ww2.mathworks.cn/help/simulink/gs/create-a-simple-model.html  
[2] https://github.com/wilselby/MatlabQuadSimAP  

