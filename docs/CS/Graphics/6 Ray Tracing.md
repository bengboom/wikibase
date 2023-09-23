## Motivation
光栅化不好处理shadow
要用**shadow mapping** 
Key idea: points not in the shadow must be seen both by the light&camera
但仍然存在一些问题 只能处理点光源的阴影 只能做硬阴影
![[Screenshot 2023-08-31 at 19.45.10.png]]

Ray-Tracing: accurate but slow (often used in offline application)
10K cpu hour -> generate 1 frame
## Ray-Tracing

### Basic Algorithm
- light travels in straight lines
- light rays won't collide
- light from source to eyes **reciprocity光路可逆**

依然需要深度缓存
![[Screenshot 2023-08-31 at 20.06.02.png]]
但是光线可能是在各种物体上反射很多次才到眼镜

### Recursive (Whitted-Style) Ray Tracing
1980年代提出的算法
有的物质是玻璃材质，具有反射系数，折射系数，光打到物体上会折射反射很多次，在每一个折射点计算着色值![[Screenshot 2023-08-31 at 20.15.21.png]]
把亮度加到最后Image plane像素里面

#### Ray-Surface Intersection
Ray Equation: origin+direction vector
$r(t)=o+td, 0\leq t < \inf$
Sphere: 球的方程，两者联立解方程
隐式表面：point $p$, $f(p)=0$
$f(o+td)=0$

Plane Equation
$(p-p')\cdot N=0$
Ray Plane 联立 Intersection
$t=\frac{(p'-o)\cdot N}{d\cdot N}$

Moller Trumbore Algorithm
![[Screenshot 2023-08-31 at 21.41.03.png]]
三角形中心坐标表示三角形内的点，等号左边是光线的点，解方程。
克拉默法则 线性代数解出来

### Performance Challenges
Naive Algorithm:
$pixels*objects * bounces$ 计算量大

### Acceleration: Bounding Volumes

如果一个物体没碰到包围盒，那就一定碰不到里面的东西，类似于[[3 Rasterization]] bounding box
Box is the intersection of 3 pairs of slabs*厚板*
**Axis-Aligned** Bounding Box *AABB* *轴对齐包围盒*

>Why axis aligned? 好求！![[Screenshot 2023-08-31 at 21.58.03.png]]
>计算量更小

Ray Intersection with AABB
**the ray enters the box only when it enters all pairs of slabs**
**the ray exits the box as long as it exits any pair of slabs**
for each pair, calculate $t_{min},t_{max}$, (negative is fine)
$t_{enter}=max\{ t_{min} \}$,$t_{exit}=min\{t_{max}\}$
离开时间为负？盒子在光线后，无交点
离开时间为正，进入时间为负，在盒子里面
有交点：$t_{enter}<t_{exit} \&\& t_{exit}>0$


#### 空间划分
判断与盒子是否相交很快，只有先确定与盒子相交之后再判断是否与里面的物体相交
Grid resolution: not too small(no speedup), not too much(complexity)
网格数量在精度高的地方更密集，合理设定网格大小有利于提升性能![[Screenshot 2023-09-01 at 17.54.14.png]]

KD-Tree 横着一刀竖着一刀，有一定的二叉性质，simple 但算法不太好写，因为这是方形，而模型一般是一堆三角形
BSP-Tree 空间中砍一刀，不是Axis-Aligned 更麻烦，维度高后太复杂

#### 物体划分 Object Partition
Bounding Volume Hierarchy

> 这样的划分其实都体现出了一种思想
> 一种和前缀和类似的思想
> 考虑内部细小的信息太难了，于是在外面“总结简化”为一个方形（更简单的东西）从而实现一定程度上的剪枝