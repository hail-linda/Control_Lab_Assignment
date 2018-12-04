# 多级机械臂联动控制算法





## 坐标轴变换方法

### 欧拉角

对于在三维空间里的一个参考系，任何坐标系的取向，都可以用三个欧拉角来表现。参考系又称为实验室参考系，是静止不动的。而坐标系则固定于刚体，随着刚体的旋转而旋转。  
用欧拉角描述三维刚体旋转，它将刚体绕过原点的轴(i,j,k)旋转θ，分解成三步（蓝色是起始坐标系，而红色的是旋转之后的坐标系。）。

<center>
<img width="40%" src="https://pic4.zhimg.com/80/v2-c339d9c88aa8988a0480365c74bbe1eb_hd.jpg">
</center>




1. 绕z轴旋转α，使x轴与N轴重合，N轴是旋转前后两个坐标系x-y平面的交线
2. 绕x轴（也就是N轴）旋转β，使z轴与旋转后的z轴重合
3. 绕z轴旋转γ，使坐标系与旋转后的完全重合


按照旋转轴的顺序，该组欧拉角被称为是“zxz顺规”的。对于顺规的次序，学术界没有明确的约定。

其中，矩阵M表示了上面三次旋转的总过程。
![](https://www.zhihu.com/equation?tex=M+%3D+Rot%28z%2C%5Calpha%29%5Ccdot+Rot%28x%2C%5Cbeta%29%5Ccdot+Rot%28z%2C%5Cgamma%29+%3D+%5Cleft%28+%5Cbegin%7Bmatrix%7Dcos%5Cgamma%26sin%5Cgamma%260%5C%5C-sin%5Cgamma%26cos%5Cgamma%260+%5C%5C0%260%261%5Cend%7Bmatrix%7D%5Cright%29%5Cleft%28%5Cbegin%7Bmatrix%7D1%260%260%5C%5C0%26cos%5Cbeta%26sin%5Cbeta+%5C%5C0%26-sin%5Cbeta%26cos%5Cbeta%5Cend%7Bmatrix%7D%5Cright%29%5Cleft%28+%5Cbegin%7Bmatrix%7Dcos%5Calpha%26sin%5Calpha%260%5C%5C-sin%5Calpha%26cos%5Calpha%260+%5C%5C0%260%261%5Cend%7Bmatrix%7D%5Cright%29)



![](https://www.zhihu.com/equation?tex=M+%3D+%5Cleft%28%5Cbegin%7Bmatrix%7D+cos%5Calpha+cos%5Cgamma-sin%5Calpha+cos%5Cbeta+sin%5Cgamma+%26sin%5Calpha+cos%5Cgamma%2Bcos%5Calpha+cos%5Cbeta+sin%5Cgamma+%26+sin%5Cbeta+sin%5Cgamma%5C%5C+-cos%5Calpha+sin%5Cgamma-sin%5Calpha+cos%5Cbeta+cos%5Cgamma+%26-sin%5Calpha+sin%5Cgamma%2Bcos%5Calpha+cos%5Cbeta+cos%5Cgamma%26+sin%5Cbeta+cos%5Cgamma%5C%5C+sin%5Calpha+sin%5Cbeta+%26+-cos%5Calpha+sin%5Cbeta%26+cos%5Cbeta+%5Cend%7Bmatrix%7D%5Cright%29)

这个过程中新生成的坐标系 $ \left(\begin{matrix}\\X \\Y \\Z \end{matrix}\right) $可以通过运算由原坐标系$  \left(\begin{matrix}\\x \\y \\z \end{matrix}\right) $得到:

![](https://www.zhihu.com/equation?tex=%5Cleft%28%5Cbegin%7Bmatrix%7DX+%5C%5CY+%5C%5CZ%5Cend%7Bmatrix%7D%5Cright%29+%3D+M%5Cleft%28%5Cbegin%7Bmatrix%7Dx+%5C%5Cy+%5C%5Cz%5Cend%7Bmatrix%7D%5Cright%29)

### 四元数 <sup>[1]-[3]</sup>

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



### DH参数矩阵<sup>[4]</sup>

在机器人运动学中，所谓连杆就是具有一定运动学功能的刚性杆，这和《机械原理》中的构件有相似的性质，就是它是运动的最小单元，而且由于它本身的形状和大小会对运动有影响。至于刚性，就是在运动学阶段我们认为连杆受力不会发生变形（事实上机器人在操作臂在运动过程中受力情况复杂，一定会发生变形）。在机器人操作臂中，研究的内容主要是一系列连杆通过关节连接起来而组成的空间开式运动链。

描述的连杆参数有两个，一个是连杆的长度a，另一个是连杆的转角α



<center>
<img width="60%" src="https://images2015.cnblogs.com/blog/802039/201612/802039-20161231095616148-1602826667.jpg">
</center>


上面我使用了两个参数完全地将一个连杆的运动学特殊性描述出来了，接下来就需要思考如何描述相邻两个连杆之间的关系，两个连杆之间是通过一个关节连接的，我们很容易想到描述连杆之间的关系实际上反映的是这个关节轴的一些特性。


<center>
<img width="60%" src="/home/linda/.config/Typora/typora-user-images/1543941840302.png">
</center>

连杆i-1和连杆i通过关节轴i相互连接，沿着关节轴i的轴向，将连杆i-1的长度$a_{i-1}$移动到和连杆i的长度$a_i$的距离叫做连杆偏距$d_i$ ，这个参数反映了两个连杆沿着轴i的距离。

连杆i-1和连杆i的长度线并不共线，这说明这两条线之间存在夹角，我们规定绕轴i将长度线$a_{i-1}$的延长线转动到和长度线$a_i$ 转过的角度叫做关节角θi, 

当按照上述的要求定义好坐标系之后，四个连杆参数可以有在坐标系中的描述：

　 -　$a_i$ = 沿着$X_i$ 轴从$Z_i$ 移动到$Z_{i+1}$的距离。

　-　$α_i$ = 绕着$X_i$轴从$Z_i$转到$Z_{i+1}$的角度。

　-　$d_i$ = 沿着$Z_i$轴从$X_{i-1}$到$X_i$的距离。

　-　$θ_i$ = 绕着$Z_i$轴从$X_{i-1}$到$X_i$的角度。

以上四个参数可以确定一个连杆在空间中的相对位置

由多个连杆的这样四个参数可以组成DH参数矩阵,用于描述一个机械臂的状态





## 基于Matlab/robotics toolbox 的机械臂仿真

```matlab
startup_rvc
```

引入robotics库

```matlab
dh = [
0 0 pi/2 0
0 0.5 0 pi/2
0 0 pi/2 pi/4
1 0 -pi/2 0
0 0 pi/2 0
1 0 0 0
]%带球形腕的拟人臂

r=SerialLink(dh)
```

以DH矩阵形式构建一机械臂

```matlab
r.teach
```

<center>
<img width="50%" src="/home/linda/.config/Typora/typora-user-images/1543943686088.png">
</center>

https://blog.csdn.net/Kalenee/article/details/81990130  
matlab工具箱

https://blog.csdn.net/bjash/article/details/80117771  
工具箱下载地址：  
https://pan.baidu.com/s/1pLo7V7d








## reference:


[1] https://krasjet.github.io/quaternion/quaternion.pdf  
[2] https://www.zhihu.com/question/23005815  
[3]https://zh.wikipedia.org/wiki/%E5%9B%9B%E5%85%83%E6%95%B0%E4%B8%8E%E7%A9%BA%E9%97%B4%E6%97%8B%E8%BD%AC  
[4] https://www.cnblogs.com/wangxiaoyong/p/6238864.html  
[5]  https://zhuanlan.zhihu.com/p/24797661  
[6] https://blog.csdn.net/fendoubasaonian/article/details/78229445  
[7] https://mirsking.com/2014/02/05/learning_robotics_toolbox_for_matlab/  
[8] https://www.youtube.com/watch?v=-PiHM14OZhU