## Lenet-5
非常成功的神经网络模型 1998年Yann LeCun提出，90年代被很多美国银行使用
![[Pasted image 20231209111725.png]]

手写字体库 MNIST 错误率0.95%

# ImageNet Contest ILSVRC
ImageNet, 15,000,000 images, 22,000 categories
![[Pasted image 20231209111849.png]]
## Alex Net
GPU 并行训练，使用ReLU
5层卷积池化，3层全连接 ![[Pasted image 20231209111949.png]]![[Pasted image 20231209112032.png]]
## VGG
▪AlexNet的增强版，16、19层
just very deep，加深了。

## Inception
由多个**inception模块**和少量池化层堆叠而成
▪在该网络中，一个卷积层包含多个不同大小的卷积操作，称为Inception模块
▪Inception模块同时使用1×1、3×3、5×5等不同大小的卷积核，并将得到的特征映射在深度上拼接（堆叠）起来作为输出特征映射

- Inception-v3
	- 用多层的小卷积核来替换大的卷积核，以减少计算量和参数量
	- 使用两层3x3的卷积来替换v1中的5x5的卷积
	- 使用连续的nx1和1xn来替换nxn的卷积
 
## Resnet
- 残差网络（Residual Network，ResNet）是通过给非线性的卷积层增加直连边的方式来提高信息的传播效率
- 假设在一个深度网络中，我们期望一个非线性单元（可以为一层或多层的卷积层）f(x,θ)去逼近一个目标函数为h(x)
- 将目标函数拆分成两部分：恒等函数和残差函数
- ![[Pasted image 20231209112311.png]]

$$h(x)=x + (h(x)-x) $$
