## NLL（负对数似然）损失函数学习笔记

### 定义
负对数似然（NLL）损失函数是机器学习中常用的损失函数之一，特别是在分类问题中。它衡量的是模型输出概率分布和目标真实分布之间的不一致性。

### 基本原理
NLL损失函数计算的是模型预测的概率分布的对数值的负数。当模型的预测结果接近真实标签时，NLL损失较低；反之，则较高。

### 公式表示
假设有一个分类问题，其类别标签集合为 \( C \)，对于单一样本，NLL损失函数可表示为：

$$
L(y, \hat{y}) = - \log(\hat{y}_y)
$$

其中，\( y \) 是真实标签的类别索引，\( \hat{y} \) 是模型预测的概率分布，\( \hat{y}_y \) 是模型预测的对应于真实标签的概率。

### 应用场景
- **多分类问题**：NLL损失通常与Softmax激活函数结合使用，在多分类问题中表现良好。
- **概率模型**：在需要评估概率预测准确性的模型中使用。

### 与交叉熵损失的关系
NLL损失与交叉熵损失密切相关。
> Pytorch 中 CrossEntrypyLoss implicitly applies a softmax activation followed by a log transformation but NLLLoss does not.

### 实现
在Python的深度学习框架（如PyTorch）中，NLL损失可以直接通过内置函数实现。

#### PyTorch示例
```python
import torch
import torch.nn as nn

# 假设有3个分类，每个样本有一个真实标签
loss = nn.NLLLoss()
input = torch.randn(3, 5, requires_grad=True) # 模型输出
target = torch.tensor([1, 0, 4]) # 真实标签
output = loss(input, target)
output.backward()
