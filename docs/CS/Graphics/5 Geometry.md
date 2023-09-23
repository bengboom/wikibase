## Representations 表示几何
Best Representation depends on the task
### Implicit 隐式
points 满足...关系，例如$x^2+y^2=1$ ，约束关系式，不直观。把点的坐标代入到关系式中判断点在物体内外。

- algebraic surface
- level sets 水平集![[Screenshot 2023-08-30 at 20.54.33.png]]这种方式比较精确，通过差值得到边缘曲线（等高线）
- construct solid geometry 通过基本几何之间的运算（布尔运算，交并补）来定义新的几何体![[Screenshot 2023-08-22 at 23.09.05.png]]	
 - distance functions, gradually blend surfaces together using Distance functions.
- Fractal 分形 不断重复，自身相似性 难以控制形状

#### Pros
compact description(a function)
certain query easy
good for ray-to-surface intersection
simple shape, no sampling error
easy to handle changes in topology
#### Cons
difficult to model complex components
### Explicit 显式
all points are given directly / given via parameter mapping (u,v)->(x,y,z)
这种的话，判断一个点在不在物体内外变困难了
- point cloud 现在显卡性能强了才有了点云
	- list of points, 物体的表面表示为一堆点，足够细就看不到缝隙
	- 三维空间的扫描结果通常都是点云 例如车载激光雷达
- polygon mesh
	- triangle meshes, usually
	- more complicated, most common
	- 把表面用很多的多边形，三角形来表示
	- ![[Screenshot 2023-08-30 at 21.05.57.png]]
- Bezier surfaces 贝塞尔曲面
- .obj format Wavefront Object File
	- a text file specifies vertices, normals, texture coordinates and their connectivities
	- 在图形学研究中常用
## Curves and Surfaces
曲线和曲面
### Curves
相机按照曲线移动（视角变化）
物体的运动轨迹
#### Bezier Curve
通过几个控制点的坐标定义一条曲线
定义切线方向、起始点、结束点
怎么画贝塞尔曲线？
#### **de Casteljau Algorithm**
- 先把绘制一条线的问题转换为绘制无数个点的问题，映射为（0，1），某个时间t
![[Screenshot 2023-08-30 at 21.13.42.png]]
- 然后得到了两个点，再通过t找到中间那个店，这就是那个点![[Pasted image 20230830211450.png]]
- run the same algorithm for $t\in(0,1)$ 
对于多个控制点的情况：类似的，四点三线中间差值得三点两线，再得两点一线，在根据t比例找出最终点![[Screenshot 2023-08-30 at 21.16.45.png]]

最终：![[Screenshot 2023-08-30 at 21.19.16.png]]
最后可以得到描述曲线的多项式
![[Screenshot 2023-08-30 at 21.19.39.png]]

凸包convex hull property
画出来的贝塞尔曲线一定在几个控制点围城的凸包之内

#### Piecewise Bezier Curves
把一条长的曲线分成很多个小段，分别用贝塞尔曲线描述，而不是直接描述那根很长的![[Screenshot 2023-08-30 at 21.23.20.png]]
这样可以更直观。chain many low-order Bezier curve
most common:**Piecewise Cubic Bezier**每四个控制点一条小曲线，used in fonts, paths, Keynote...
$C^1$连续：两小段之间，在控制点处的切线方向一致，这样光滑，曲线斜率导数连续![[Screenshot 2023-08-30 at 21.26.21.png]]
$C^0$连续：小段之间首尾相连就行
更高阶的：曲率连续，更加平滑
#### Other method
- splines, 由一系列控制点控制，满足一定的连续性，a curve under control
- B-splines 对贝塞尔曲线的扩展 调节一个控制点不会影响全局 具有局部性
### Surfaces
#### Bezier Surfaces
把贝塞尔曲线推广到曲面，一个个小平面之间无缝、连续
几个维度之间扫出一条线，那条线就会扫出一条面
https://acko.net/files/mathbox/MathBox.js/examples/BezierSurface.html
## Meshes
games101-lec12
### Mesh subdivision 网格细分
upsampling 增加三角形数量
- loop subdivision
	- split each triangle into four 先增大三角形数量
	- 再调节三角形位置assign new vertex positions according to weights
	- 新产生的顶点和以前就存在的顶点采用不同的方式改变位置
- Catmull-Clark Subdivision (General Mesh)
	- 如果不是三角形网格用这种
### Mesh simplification
downsampling 去掉一些三角形，但保持基本形状和外观
### Mesh Regularization
三角形有大有小，在数量不变的情况下让小三角形面积接近
![[Screenshot 2023-08-30 at 21.37.08.png]]
