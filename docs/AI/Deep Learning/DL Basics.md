近年机器学习最火热的分支
## 概念
A machine learning subfield of learning representations of data, which is exceptional effective at learning patterns.
Deep learning algorithms attempt to learn (multiple levels of) representation by using a hierarchy of multiple layers.
**End-to-End**
![[Pasted image 20231208232935.png]]


### Why Deep?
因为理想的特征是多维hierarchial的（从原始信号，逐步探明边缘和方向，再到轮廓和形状，再进一步抽象，判断是谁的脸）
从仿生学的角度，也是如此。大脑的多个区域处理不同的信息。逐层上升的智能。
神经元进行非线性变换，于是有了人工神经元

### ANN
人工神经网络主要由大量的神经元以及它们之间的有向连接构成。考虑三方面：
- 神经元的激活规则
	- 主要是指神经元输入到输出之间的映射关系，一般为非线性函数
- 网络的拓扑结构
	- 不同神经元之间的连接关系
- 学习算法
	- 通过训练数据来学习神经网络的参数

## 步骤
### 定义网络
### 损失函数
### 优化

## 几种神经网络的比较
- 前馈神经网络
	- 信息单向向前传递，层内无连接，适合于静态数据分析，如回归、分类等任务[[FNN]]
- 反馈神经网络
	- 全部或者部分神经元**可以接受来自其他神经元或自身**信号的神经网络结构
	- 拓扑为网状，有一定层级，被视为动态系统
- 递归神经网络
	- **至少包含一个反馈连接**的神经网络结构，适合处理时序数据[[RNN]]