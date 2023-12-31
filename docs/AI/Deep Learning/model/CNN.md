卷积神经网络 CNN Convolutional Neural Network
Yann LeCun 1989
是一种FNN，对于图像处理特殊设计的网络结构
包括：卷积、池化、全连接
有了局部性，且节省了大量权重

## Bio Connection
▪卷积神经网络是受生物学上感受野（Receptive Field）的机制而提出的
▪在视觉神经系统中，一个神经元的感受野是指视网膜上的特定区域，只有这个区域内的刺激才能够激活该神经元

# Convolution
输入 核 输出 步幅 补零

输出大小=（N-F)/S+1
N: input, F: core, S: Stride
![[Pasted image 20231209143735.png]]

## 关键概念

### 输入
- 输入是一个二维数据矩阵，通常用于表示图像的像素值。
- 尺寸表示为$H \times W$，其中$H$是高度，$W$是宽度。

### 卷积核（Kernel）
- 卷积核是一个小型矩阵，用于在输入数据上滑动并提取特征。
- 卷积核的尺寸通常表示为$K \times K$。

### 步幅（Stride）
- 步幅是卷积核在输入数据上滑动时的移动距离。
- 步幅的大小直接影响输出数据的尺寸。

### 填充（Padding）(补零)
- 填充是在输入数据的边界添加的额外零值。
- 用于控制输出大小并保持边界信息。

### 输出
- 输出是卷积操作的结果，其尺寸取决于输入尺寸、卷积核尺寸、步幅和填充。

## 数量关系公式

输出矩阵的尺寸$O$可以通过以下公式计算：
- 高度：$O_{\text{height}} = \left\lfloor \frac{H - K + 2P}{S} + 1 \right\rfloor$
- 宽度：$O_{\text{width}} = \left\lfloor \frac{W - K + 2P}{S} + 1 \right\rfloor$

其中：
- $H$和$W$分别是输入矩阵的高度和宽度。
- $K$是卷积核的尺寸。
- $S$是步幅。
- $P$是填充。
- $\lfloor \cdot \rfloor$表示向下取整。

通过这个公式，我们可以理解在改变卷积核尺寸、步幅或填充时，输出尺寸是如何变化的。例如，增加步幅会减小输出尺寸，而增加填充则会增加输出尺寸。

## 两种卷积
![[Pasted image 20231209144035.png]]

1. **有效（valid）卷积**：
    - 在这种模式下，卷积核仅在输入数据的内部滑动，不超出边界。
    - 这意味着没有在输入数据的外部添加额外的零填充。
    - 结果是，输出特征图的尺寸会小于输入数据的尺寸。
    - 有效卷积更倾向于减少输出的尺寸，因为它不考虑边界周围的信息。
2. **相同（same）卷积**：
    - 在这种模式下，通过在输入数据的边界添加适当数量的零填充，卷积核可以滑动到输入数据的边缘。
    - 添加的填充保证了输出特征图的尺寸与输入数据的尺寸相同（当步幅为1时）。
    - 相同卷积通常用于希望输出层保持与输入层同样尺寸的情况，这对于构建深层网络结构特别有用，因为它允许多个卷积层的叠加而不会迅速减小空间维度。

## 特性
时间或空间上的下采样
局部连接
权重共享

# Pooling
▪池化函数使用某一位置的相邻输出的总体统计特征来代替网络在该位置的输出
▪最大池化（Max Pooling）函数给出相邻矩形区域内的最大值
▪其他常用的池化函数包括相邻矩形区域内的平均值、L2范数以及基于据中心像素距离的加权平均函数
缩小参数量

# Full Connected
可能使用多种卷积核对原数据卷积，这样Layer，通道数变多了。
卷积层的特例。Flatten后全连接
