# 循环神经网络(RNN)

## 基本结构
- $x$：输入层节点；$o$：输出层节点
- $s$：隐层节点的激活值
- $U$：输入层到隐层的连接权重矩阵（input to hidden）
- $V$：隐层到输出层的连接权重矩阵（hidden to output）
- $W$：隐层到隐层之间的连接权重矩阵（hidden to hidden）
- $W$：循环矩阵（recurrent matrix）![[Pasted image 20231209145647.png]]

## 序列到序列学习(Sequence-to-sequence learning)
- $h_t = f(Ux_t + Wh_{t-1} + b)$
- $o_t = f(Vh_t + b')$
- $h_{t-1}$ 代表的是上一时间步的隐状态，而$h_t$ 代表当前时间步的隐状态。将$h_{t-1}$ 乘以权重矩阵$W$的意义在于，它允许网络将从之前时间步学习到的信息传递到当前时间步。

## 基于时间延迟展开
- 展开后可以看到每个时间步的循环网络结构，便于我们理解RNN如何处理序列数据。
- 每个时间步的节点不是完全独立，而是通过$W$连接起来的，这样可以传递序列信息。
![[Pasted image 20231209145712.png]]

## 通过时间反向传播算法（Backpropagation Through Time, BTT）
- 类似于传统神经网络的反向传播算法，但是要在时间序列上展开。
- ![[Pasted image 20231209145840.png]]
- 公式表示：
  - $U$：input-hidden
  - $V$：hidden-output
  - $W$：hidden-hidden
- $U, V, W$每经过一个time-step就会更新一次。
- $U, V, W$使用BP算法，在对时间序列进行学习时进行更新。

## 状态更新函数(State update function)
- $h_t = f_w(h_{t-1}, x_t)$
- $h_t$：new state
- $h_{t-1}$：old state
- $x_t$：input vector at some time step
- $f_w$：some function with parameters $W$
