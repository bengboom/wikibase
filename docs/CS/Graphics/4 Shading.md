> the darkening or coloring of an illustration or diagram with parallel lines or a block of color
> The process of applying a material to an object.

着色，材质，光
## Shading Model -- Blinn-Phong Reflectance Model
![[Screenshot 2023-08-21 at 21.13.39.png]]
高光，漫反射，间接光照，三项相加

Shading is local, shading point on the surface of an object
Shading is not shadow. No shadows will be generated.
![[Screenshot 2023-08-21 at 21.21.10.png]]

### 漫反射 Diffuse Term
光线被均匀地反射到各个方向上。需要计算光照的能量，与光线和表面的夹角有关，恰好垂直就更亮。![[Screenshot 2023-08-21 at 21.25.02.png]]

能量平方反比关系![[Screenshot 2023-08-21 at 21.31.38.png]]

![[Screenshot 2023-08-21 at 21.32.22.png]]

$L_d=k_d(I/r^2)max(0,n\cdot l)$
nl点乘负的：从后面来的光，忽略。

### Specular term
观察方向和镜面反射方向比较接近的时候可以观察到发光，也就是法线n方向和半程向量h很接近。加上一个指数：为了增加陡峭度，不然太平滑了变化小，要有突出。
![[Screenshot 2023-08-21 at 21.56.31.png]]
### Ambient term
环境光，空气中灰尘的反射，没有任何一个地方最后会是纯黑的。
add a constant, approximate/fake
$L_a=k_a*I_a$

## Shading Frequency
### Flat Shading
Shade each **triangle**(flat shading), not good for smooth surfaces
### Gouraud Shading
Interpolates colors from **vertices** across triangle
each vertex has a normal vector
怎么求点的法向量：周围的面的法向量的平均，可以用三角形面积来加权![[Screenshot 2023-08-21 at 22.22.41.png]]

### Phong shading
shade each **pixel**, compute full shading model at each pixel
interpolate normal vectors at each triangle
not the Blinn-Phone Reflectance model
![[Screenshot 2023-08-21 at 22.24.23.png]]
### Comparison
![[Screenshot 2023-08-21 at 22.19.05.png]]
## Graphics (Real-time Rendering) Pipeline 渲染管线
- Inputs: vertices in 3D space
- Vertex processing
- Triangle processing
- Rasterization (sampling)
- Fragment (pixel) processing (shading) 
- Frame buffer Operations
- Output: image
Implemented by hardware in GPU

> 现代的渲染管线中，有的部分是可编程的，代码来决定如何着色
> shader: 控制像素和顶点如何进行着色，在OpenGL/DirectX中提供了api
> shadertoy.com/view/ld3Gz2

Goal: complex scene, realtime, high resolution, high fps

GPU: Heterogeneous Multi-Core Processor
2-4T FLOPs to executing vertex and fragment shader programs
## Texture Mapping

### Texture
材质映射 different color at different places
材质本身有颜色，之前是考虑了光源的影响
物体的漫反射系数不一样，物体因材质不同，自身不同部位的漫反射系数等也不同
![[Screenshot 2023-08-21 at 22.49.51.png]]
图形坐标到纹理坐标的映射
![[Screenshot 2023-08-21 at 22.52.34.png]]
texture can be used multiple times, copy, repeat.

### Interpolation Across Triangles: Barycentric Coordinates 重心坐标
知道了三角形三个顶点的纹理坐标，如何在三角形内做一个平滑的过度
插值什么：texture coordinators, colors, normal vectors

Barycentric Coordinates: 重心坐标
$(x,y)=\alpha A + \beta B + \gamma C$
$\alpha +\beta +\gamma =1$
all three coordinates are non-negative(inside the triangle)
重心：$\alpha=\beta=\gamma=1/3$
![[Screenshot 2023-08-21 at 23.08.15.png]]
更简单的用坐标的计算方法，可以推导，和面积等价
![[Screenshot 2023-08-21 at 23.11.27.png]]
根据重心坐标做任何属性的插值，$\alpha \beta \gamma$即是权重，这样就得到了三角形内部每个点的属性信息。但在projection下，不能保证重心坐标不变，投影之后形状会发生变化，变形。

### Applying Textures

Diffuse Color

#### Texture Magnification
纹理放大 如果纹理太小了，高分辨率的墙，低分辨率的纹理，模糊，用上插值方法。
a pixel on a texture -- a texel 纹理元素

Bilinear interpolation 双线性插值
![[Screenshot 2023-08-22 at 17.08.37.png]]
lerp linear interpolation
$lerp(x,v_0,v_1)=v_0+x(v_1-v_0)$
two helper lerps
$u_0=lerp(s,u_{00},u_{10})$
$u_1=lerp(s,u_{01},u_{11})$
final result $f(x,y)=lerp(t,u_0,u_1)$

Bicubic interpolation 取周围临近16个，用了三次方不是线性

#### Texture Minification
(hard case) 如果纹理太大了怎么办，可能会引起更严重的问题，摩尔纹，锯齿![[Screenshot 2023-08-22 at 17.18.46.png]]
有时需要upsampling，有时需要downsampling

怎么antialiasing? supersampling costly, high frequency 算法太慢了
采样会影响，那就直接不使用sample(Point Query)的方法，just get the average of a range(Range Query)
点查询问题和范围查询问题，

mipmap: allowing (fast, approx. , square) range queries 可以做范围查询的东西，快、近似、方形
![[Screenshot 2023-08-22 at 17.29.39.png]]
一层一层的预计算下面一层几个像素的平均，图像金字塔。引入了多大的额外存储量？只多了1/3原始图像的存储量。
去哪一层查呢？把一个像素覆盖的区域近似为一个正方形
![[Screenshot 2023-08-22 at 17.46.40.png]]
有的时候说要查1.8层的值怎么办？再用差值
Trilinear interpolation
![[Screenshot 2023-08-22 at 17.50.58.png]]两层之间的差值

mipmap问题：会损失细节，overblur![[Screenshot 2023-08-22 at 17.54.42.png]]
问题来自他只能查询方形square区域，会导致这一现象。解决方式
**Anisotropic filtering 特向异性过滤**
![[Screenshot 2023-08-22 at 17.57.43.png]]
Ripmaps -- can look up axis-aligned **rectangular** zones
Diagonal footprints still a problem

EWA filtering
handle irregular footprints
multiple lookup, weighted average, mipmap hierarchy
![[Screenshot 2023-08-22 at 18.03.09.png]]

2x各向异性过滤：采几层，只要显存足够，计算量差不多，显存开销比较大

### Application of textures
texture = memory + range query
环境光照 light from the environment
rendering with the environment

Texture can affect shading
- store height/normal
- bump/normal mapping
- fake the detailed geometry

Bump mapping
Displacement mapping

games101 lec10 前半部分 没学完