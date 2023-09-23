## Why transformation

modeling
viewing: 3D->2D Projection
projection transformation
MVP

## 2D transformations
$$x' = M x + T...平移$$
### Rotating 旋转
![[Screenshot 2023-08-20 at 17.18.55.png]]

about the origin (0,0), CCW by default *Counter clock wise 逆时针*

旋转矩阵$\mathbf{R}_\theta=\left[\begin{array}{cc}\cos \theta & -\sin \theta \\ \sin \theta & \cos \theta\end{array}\right]$

### Scale 缩放
$x'=sx, y'=sy$
matrix multiply:
$\left[\begin{array}{l}x^{\prime} \\ y^{\prime}\end{array}\right]=\left[\begin{array}{ll}s & 0 \\ 0 & s\end{array}\right]\left[\begin{array}{l}x \\ y\end{array}\right]$

### Shear
![[Screenshot 2023-08-20 at 17.16.10.png]]
$\left[\begin{array}{l}x^{\prime} \\ y^{\prime}\end{array}\right]=\left[\begin{array}{ll}1 & a \\ 0 & 1\end{array}\right]\left[\begin{array}{l}x \\ y\end{array}\right]$

平移要额外加一个矩阵，能不能把这个也统一表示起来？
### Homogeneous coordinates
对于2D坐标的点，依然用三位矩阵来表示，增加了一个维度
Add a third coordinate (w-coordinate)
- 2D point $=(x, y, 1)^{\top}$
- 2D vector $=(x, y, 0)^{\top}$
Matrix representation of translations
$$
\left(\begin{array}{c}
x^{\prime} \\
y^{\prime} \\
w^{\prime}
\end{array}\right)=\left(\begin{array}{ccc}
1 & 0 & t_x \\
0 & 1 & t_y \\
0 & 0 & 1
\end{array}\right) \cdot\left(\begin{array}{l}
x \\
y \\
1
\end{array}\right)=\left(\begin{array}{c}
x+t_x \\
y+t_y \\
1
\end{array}\right)
$$

Scale
$$
\mathbf{S}\left(s_x, s_y\right)=\left(\begin{array}{ccc}
s_x & 0 & 0 \\
0 & s_y & 0 \\
0 & 0 & 1
\end{array}\right)
$$
Rotation
$$
\mathbf{R}(\alpha)=\left(\begin{array}{ccc}
\cos \alpha & -\sin \alpha & 0 \\
\sin \alpha & \cos \alpha & 0 \\
0 & 0 & 1
\end{array}\right)
$$
对于不是针对于原点的旋转，先把旋转点移到原点再旋转在反向平移回去，三步

Translation
$$
\mathbf{T}\left(t_x, t_y\right)=\left(\begin{array}{ccc}
1 & 0 & t_x \\
0 & 1 & t_y \\
0 & 0 & 1
\end{array}\right)
$$

### Inverse Transform
$M^{-1}$
矩阵的逆

**Transform Ordering Matters**
矩阵乘法不具有交换性
但是多步按顺序，可以合并为一个矩阵composing transforms 结合律
## 3D Transformation

- 3D point $=(x, y, z, 1)^{\top}$
- 3D vector $=(x, y, z, 0)^{\top}$

3D detailed Lecture04 未看

## Projection Transformation

### Orthographic projection 正交
忽略深度信息

### Perspective projection 透视

