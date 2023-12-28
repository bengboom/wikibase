前馈神经网络（Feedforward Neural Network）
也叫作深度前馈网络（Deep Feedforward Network）
或者多层感知机（Multi-layer Perceptron, MLP）
是典型的深度学习模型

## Concepts
FC
各神经元属于不同的层，层内无连接
相邻两层的神经元全部两两连接
整个网络**无反馈**，信号从输入层向输出层单向传播，可用有向无环图表示
![[Pasted image 20231209110325.png]]
## 传播过程
- $z^{(1)} = W^{(1)} a^{(0)} + b^{(1)}$
- $a^{(1)} = f_i(z^{(1)})$
- $x = a^{(0)}\rightarrow z^{(1)} \rightarrow a^{(1)} \rightarrow z^{(2)} \dots \rightarrow a^{(L-1)} \rightarrow z^{(L)} \rightarrow a^{(L)} = f(x; W, b)$

## 通用近似定理
![[Pasted image 20231209110738.png]]
神经网络视为万能函数来使用，对于分类问题，使用softmax归一化后可以作为后验概率。采用交叉熵损失函数。

## Back Propagation
反向传播 BP
链式法则
梯度下降可以学习分类器

## Code Example
Keras. a python lib supporting deep learning algorithm, providing framework

```python
from keras.models import Sequential
from keras.layers import Dense, Activation
from keras.optimizers import SGD
model = Sequential()
model.add(Dense(output_dim=64, input_dim=100))
model.add(Activation("relu"))
model.add(Dense(output_dim=10))
model.add(Activation("softmax"))
model.compile(loss='categorical_crossentropy',
   optimizer='sgd', metrics=['accuracy'])
model.fit(X_train, Y_train, nb_epoch=5, batch_size=32)
loss = model.evaluate(X_test, Y_test, batch_size=32)
```
## 问题
- 权重矩阵参数非常多，因为是全连接，非常密集
- 不满足局部不变性特征
	- 自然图像中的物体有局部不变性特征，比如尺度缩放平移旋转操作并不影响语义信息
	- 而全连接神经网络难以提取这些局部不变特征
	- 于是有了卷积神经网络 [[CNN]]
	- CNN是一种FNN