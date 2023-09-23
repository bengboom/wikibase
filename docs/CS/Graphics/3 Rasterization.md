Rasterize == drawing onto the screen 光栅化
![[Screenshot 2023-08-21 at 20.22.16.png]]

- Irrelevant to z
- Transform in xy plane: $[-1,1]^2$ to $[0$, width $] \times[0$, height $]$
- Viewport transform matrix:
$$
M_{\text {viewport }}=\left(\begin{array}{cccc}
\frac{\text { width }}{2} & 0 & 0 & \frac{\text { width }}{2} \\
0 & \frac{\text { height }}{2} & 0 & \frac{\text { height }}{2} \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1
\end{array}\right)
$$
## 显示方式
Rasterizing Triangles into Pixels
电视，阴极射线管，隔行扫描（一帧奇数一帧偶数行，利用视觉暂留）

Frame buffer -> DAC -> Analog signal(RGB)

LCD *Liquid Crystal Display* Pixel
block/transmit light by twisting polarization, backlight needed(LED/fluorescent)
光的偏振

LED *Light Emitting Diode*

Electrophoretic (Electronic Ink) 墨水屏
## Triangles
Fundamental Shape Primitives, Basic polygon
Unique characteristics: planer一定是平面，well-defined interior内外定义清晰，interpolating定义顶点后，可渐变

Sampling: 对于每个像素点进行采样，判断是在三角形内还是三角形外discretize
通过向量叉乘的方式判断点在三角形三个边的左边还是右边，如果都是在右侧，则说明该点在三角形内

Edge Cases? 如果点恰好在两个三角形的公共边上？不做处理或者特殊处理。不是很影响。

Bounding box 三角形的外接矩形，在矩形外的点肯定不会在三角形内，所以只对矩阵内的点采样，判断是在三角形内还是外。但对于斜长的三角形，这种方式bounding box很大，效率低，下面的方法更好。
Incremental Triangle Traversal ![[Screenshot 2023-08-21 at 20.54.03.png]]一种加速方法，还有很多的加速方法

## Antialiasing
抗锯齿Jaggles![[Screenshot 2023-08-21 at 20.56.00.png]]
通过低通滤波，在频域上处理，使其变得平滑。Blurring(Pre-Filtering) before sampling
![[Screenshot 2023-08-21 at 20.57.35 1.png]]
还可以一个像素周围区域取平均值
超采样：Antialiasing by supersampling MSAA *Multisample anti aliasing*,采样点的数量高于实际屏幕像素，例如16倍，再取平均。性能消耗大

today's antialiasing: 
FXAA *Fast Approximate* 图像后期处理 与采样
TAA *Temporal* 找上一帧的信息，temporal与时间相关，不是超采样，连续四帧在单个像素内不同位置采样，性能消耗分散在时间上，但对于运动物体比较麻烦，因为在动。

## Super-Resolution
与抗锯齿有一点关系，超分辨率
DLSS *Deep Learning Super Sampling*
## Z-buffering
回答可见性，物体之间的遮挡这一问题。
### Painter's Algorithm
对于一个物体，先渲染远的，近的会把远的覆盖掉。
问题：难以判断，三角形之间的重叠。需要排序O(nlogn)
![[Screenshot 2023-08-21 at 21.08.38.png]]
### Z-buffer Algorithm/ Depth Buffer
对于每一个像素点存一个深度值，如果当前三角形更近则渲染并更新最近深度值，如果当前三角形距离更远就说明被挡了，不渲染。
缺点：处理不了透明物体，需要特殊处理
O(n), implemented in hardware for All GPUs
```cpp
for (each triangle T)
	for (each sample (x,y,z) in T)
		if (z < zbuffer[x,y])
			framebuffer[x,y] = rgb;
			zbuffer[x,y] = Z;
		else nothing...
```


