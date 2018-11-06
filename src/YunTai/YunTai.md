<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

# 云台
@李昕达
##index

- 一些术语、关键词的介绍
- 几个常用算法的介绍与对比
     - SIFT<sup>[1-2]</sup>
     - SURF<sup>[1-2]</sup>
     - ORB<sup>[1-2]</sup>
- 某种算法的opencv-python实现与展示

#### 术语与一些关键词

__特征点__ ：指在图像中突出且具有代表意义的一些点，这些点可以用来表征、识别图像、进行图像配准、进行3D重建等，主要将角点视为有效的特征点。  

角点通常为在灰度化图像中水平、竖直方向上变化均较大的点，通常为两条边缘的交点，更严格的说，角点的局部邻域应该具有两个不同区域的不同方向的边界<sup>[3]</sup>。  

__Harris角点算法__<sup>[4-6]</sup>：简单来讲，就是拿一个小窗在图像中移动，通过考察这个小窗口内图像灰度的平均变换值来确定角点。  

- 如果窗口内区域图像的灰度值恒定，那么所有不同方向的偏移几乎不发生变化；  
- 如果窗口跨越一条边，那么沿着这条边的偏移几乎不发生变化， 但是与边垂直的偏移会发生很大的变化；  
- 如果窗口包含一个孤立的点或者角点，那么所有不同方向的偏移会发生很大的变化。  

当窗口发生[u,v]移动时，那么滑动前与滑动后对应的窗口中的像素点灰度变化描述如下：  
$$
E(u,v)=\sum w(x,y)[I(x_u,y+v)-I(x,y)]^2
$$

>[u,v]是窗口的偏移量  
>(x,y)是窗口内所对应的像素坐标位置，窗口有多大，就有多少个位置  
>w(x,y)是窗口函数,窗口函数内所设置权值可以为平均分布或高斯分布，当窗口中心存在孤立的角点时，该点在高斯分布下的灰度变化贡献远大于平均分布下的贡献  
> 此外，将窗口函数描绘为一个椭圆高斯分布还可以指明角点的指向。  








[3] https://blog.csdn.net/mmjwung/article/details/6748895  
[4] https://blog.csdn.net/a429051366/article/details/51009438  
[5] Harris C, Stephens M. A combined corner and edge detector[C]//Alvey vision conference. 1988, 15(50): 10-5244.   
[6] https://blog.csdn.net/lwzkiller/article/details/54633670  
[4]
[4]
[4]
[4]
[4]