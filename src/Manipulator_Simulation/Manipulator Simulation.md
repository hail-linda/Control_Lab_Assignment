# 多级机械臂联动控制算法

## index
[TOC]

TODOs:

- 数据成功读入的反馈
- 能否在给定数据下成功运动的判断（可行域判断）
- 得到正确计算结果
- 可视化仿真

## 坐标轴变换方法

### 欧拉角

对于在三维空间里的一个参考系，任何坐标系的取向，都可以用三个欧拉角来表现。参考系又称为实验室参考系，是静止不动的。而坐标系则固定于刚体，随着刚体的旋转而旋转。  
用欧拉角描述三维刚体旋转，它将刚体绕过原点的轴(i,j,k)旋转θ，分解成三步（蓝色是起始坐标系，而红色的是旋转之后的坐标系。）。

![](https://pic4.zhimg.com/80/v2-c339d9c88aa8988a0480365c74bbe1eb_hd.jpg)


1. 绕z轴旋转α，使x轴与N轴重合，N轴是旋转前后两个坐标系x-y平面的交线
2. 绕x轴（也就是N轴）旋转β，使z轴与旋转后的z轴重合
3. 绕z轴旋转γ，使坐标系与旋转后的完全重合


按照旋转轴的顺序，该组欧拉角被称为是“zxz顺规”的。对于顺规的次序，学术界没有明确的约定。

其中，矩阵M表示了上面三次旋转的总过程。
![](https://www.zhihu.com/equation?tex=M+%3D+Rot%28z%2C%5Calpha%29%5Ccdot+Rot%28x%2C%5Cbeta%29%5Ccdot+Rot%28z%2C%5Cgamma%29+%3D+%5Cleft%28+%5Cbegin%7Bmatrix%7Dcos%5Cgamma%26sin%5Cgamma%260%5C%5C-sin%5Cgamma%26cos%5Cgamma%260+%5C%5C0%260%261%5Cend%7Bmatrix%7D%5Cright%29%5Cleft%28%5Cbegin%7Bmatrix%7D1%260%260%5C%5C0%26cos%5Cbeta%26sin%5Cbeta+%5C%5C0%26-sin%5Cbeta%26cos%5Cbeta%5Cend%7Bmatrix%7D%5Cright%29%5Cleft%28+%5Cbegin%7Bmatrix%7Dcos%5Calpha%26sin%5Calpha%260%5C%5C-sin%5Calpha%26cos%5Calpha%260+%5C%5C0%260%261%5Cend%7Bmatrix%7D%5Cright%29)



![](https://www.zhihu.com/equation?tex=M+%3D+%5Cleft%28%5Cbegin%7Bmatrix%7D+cos%5Calpha+cos%5Cgamma-sin%5Calpha+cos%5Cbeta+sin%5Cgamma+%26sin%5Calpha+cos%5Cgamma%2Bcos%5Calpha+cos%5Cbeta+sin%5Cgamma+%26+sin%5Cbeta+sin%5Cgamma%5C%5C+-cos%5Calpha+sin%5Cgamma-sin%5Calpha+cos%5Cbeta+cos%5Cgamma+%26-sin%5Calpha+sin%5Cgamma%2Bcos%5Calpha+cos%5Cbeta+cos%5Cgamma%26+sin%5Cbeta+cos%5Cgamma%5C%5C+sin%5Calpha+sin%5Cbeta+%26+-cos%5Calpha+sin%5Cbeta%26+cos%5Cbeta+%5Cend%7Bmatrix%7D%5Cright%29)

这个过程中新生成的坐标系 $ \left(\begin{matrix}\\X \\Y \\Z \end{matrix}\right) $可以通过运算由原坐标系$  \left(\begin{matrix}\\x \\y \\z \end{matrix}\right) $得到:

![](https://www.zhihu.com/equation?tex=%5Cleft%28%5Cbegin%7Bmatrix%7DX+%5C%5CY+%5C%5CZ%5Cend%7Bmatrix%7D%5Cright%29+%3D+M%5Cleft%28%5Cbegin%7Bmatrix%7Dx+%5C%5Cy+%5C%5Cz%5Cend%7Bmatrix%7D%5Cright%29)

### 四元数 

这部分最后也还没有理解如何实现

四元数是一种高阶复数，四元数q表示为：
$$
q=(x,y,z,w)=xi+yj+zk+w
$$
其中，i，j，k满足：
$$
i^2=j^2=k^2=-1
$$

$$
ij=k,jk=i,ki=j
$$

由于i，j，k的性质和笛卡尔坐标系三个轴叉乘的性质很像，所以可以将四元数写成一个向量和一个实数组合的形式：
$$
q=(\vec{v}+w)=((x,y,z),w)
$$
---
**使用四元数旋转向量**

三维空间中的某个点可以用纯虚四元数表示
若三维空间里的一个点的笛卡尔坐标为 $(x,y,z)$，则它用纯四元数（类似于纯虚数，即实部为0的四元数）$xi+yj+zk$ 表示。 





对向量 $ \overrightarrow p $ 和四元数 $q$, $p$ 可看做四元数 $ p= (0, \overrightarrow p) $, 向量 $ p $ 旋转 $q$ 后为：
$$
p'=qpq^{-1}
$$


如果你想算一个点![w=(wx,wy,wz)](https://www.zhihu.com/equation?tex=w%3D%28wx%2Cwy%2Cwz%29)在这个旋转下新的坐标![w^{'} ](https://www.zhihu.com/equation?tex=w%5E%7B%27%7D+),需要进行如下操作，

1.定义纯四元数

![qw=(0,wx,wy,wz)=0+wx*i+wy*j+wz*k](https://www.zhihu.com/equation?tex=qw%3D%280%2Cwx%2Cwy%2Cwz%29%3D0%2Bwx%2Ai%2Bwy%2Aj%2Bwz%2Ak)

2.进行四元数运算

![qw^{'} =q*qw*q^{-1}  ](https://www.zhihu.com/equation?tex=qw%5E%7B%27%7D+%3Dq%2Aqw%2Aq%5E%7B-1%7D++)

3.产生的![qw^{'} ](https://www.zhihu.com/equation?tex=qw%5E%7B%27%7D+)一定是纯四元数，也就是说它的第一项为0，有如下形式：

![qw^{'} =(0,wx^{'},wy^{'},wz^{'})=0+wx^{'}*i+wy^{'}*j+wz^{'}*k](https://www.zhihu.com/equation?tex=qw%5E%7B%27%7D+%3D%280%2Cwx%5E%7B%27%7D%2Cwy%5E%7B%27%7D%2Cwz%5E%7B%27%7D%29%3D0%2Bwx%5E%7B%27%7D%2Ai%2Bwy%5E%7B%27%7D%2Aj%2Bwz%5E%7B%27%7D%2Ak)

4.![qw^{'}](https://www.zhihu.com/equation?tex=qw%5E%7B%27%7D)中的后三项![(wx^{'},wy^{'},wz^{'})](https://www.zhihu.com/equation?tex=%28wx%5E%7B%27%7D%2Cwy%5E%7B%27%7D%2Cwz%5E%7B%27%7D%29)就是![w^{'} ](https://www.zhihu.com/equation?tex=w%5E%7B%27%7D+)：

![w^{'} =(wx^{'},wy^{'},wz^{'})](https://www.zhihu.com/equation?tex=w%5E%7B%27%7D+%3D%28wx%5E%7B%27%7D%2Cwy%5E%7B%27%7D%2Cwz%5E%7B%27%7D%29)

这样，就完成了一次四元数旋转运算。

https://krasjet.github.io/quaternion/quaternion.pdf  
https://www.zhihu.com/question/23005815  
https://zh.wikipedia.org/wiki/%E5%9B%9B%E5%85%83%E6%95%B0%E4%B8%8E%E7%A9%BA%E9%97%B4%E6%97%8B%E8%BD%AC  



## Matlab 仿真





https://blog.csdn.net/Kalenee/article/details/81990130  
matlab工具箱

https://blog.csdn.net/bjash/article/details/80117771  
工具箱下载地址：  
https://pan.baidu.com/s/1pLo7V7d
