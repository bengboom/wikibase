## 多头注意力机制
- **为什么使用多头注意力机制？**
  - 多头注意力机制使得Transformer能够同时关注不同子空间的信息，从而捕捉更丰富的特征。
  - 类似于CNN中使用多个滤波器的效果。

## Q, K, V 权重矩阵
- **为什么Q和K使用不同的权重矩阵？**
  - 使用不同的权重矩阵可以在不同空间进行投影，增强表达能力和泛化能力。
  - 使用相同的权重矩阵会导致self-attention退化成线性映射。

## 点乘与加法
- **为什么选择点乘计算attention而不是加法？**
  - 点乘用于生成attention score矩阵，这种方法在计算上更高效。
  - 加法虽然简单，但整体计算量与点积相似。两者的效果和维度大小`dk`相关。

## Scaled Dot-Product Attention
- **为什么进行softmax之前需要对attention进行scaled？**
  - Softmax函数在处理大规模数值时会输出近似one-hot编码，导致梯度消失。
  - 通过除以`dk`的平方根，使softmax函数的输出更加平滑。

## Padding Mask
- **如何对padding进行mask操作？**
  - Padding位置置为负无穷（通常-1000），以此在计算时排除这些位置的影响。

## 降维操作
- **为什么在多头注意力中需要降维？**
  - 通过将高维空间转化为多个低维空间，然后再拼接，可以丰富特征信息。

## 词向量乘以开方
- **为什么获取输入词向量后要乘以embedding size的开方？**
  - 为了使embedding matrix的方差为1，有利于模型的收敛。

## 位置编码
- **Transformer的位置编码有什么意义和优缺点？**
  - 因为self-attention是位置无关的，位置编码用于引入位置信息。
  - 使用固定的位置编码表示token在句子中的绝对位置。

## 残差结构
- **Transformer中的残差结构及其意义？**
  - 类似于ResNet，用于解决梯度消失问题。

## LayerNorm vs BatchNorm
- **为什么Transformer使用LayerNorm而不是BatchNorm？**
  - LayerNorm针对每个样本序列进行归一化，适用于NLP中句子长度不一致的情况。

## BatchNorm技术
- **BatchNorm的优缺点是什么？**
  - 优点：解决内部协变量偏移问题，加快收敛。
  - 缺点：在batch_size较小或在RNN中效果差。

## 前馈神经网络
- **Transformer中前馈神经网络使用了哪种激活函数？**
  - 使用ReLU激活函数。

## Encoder和Decoder交互
- **Encoder和Decoder是如何交互的？**
  - 通过Cross Self-Attention，Decoder提供Q，而Encoder提供K和V。

## 并行化
- **Transformer的并行化体现在哪里？Decoder端可否并行化？**
  - 并行化主要体现在Encoder端。Decoder端由于sequence mask的引入，推理过程不能并行。

## WordPiece Model 和 Byte Pair Encoding
- **描述WordPiece Model和Byte Pair Encoding。**
  - 这两种方法都是为了更好地处理未知或罕见词汇。
  - BPE通过迭代合并频繁的字节对来减少OOV问题。

## 训练中的学习率和Dropout
- **Transformer训练时学习率和Dropout的设置是怎样的？**
  - Dropout测试时应对输入乘以dropout比率。
  - BERT与Transformer在attention屏蔽技巧上的差异主要源于它们不同的目标。

---

以上总结基于网页内容：[Transformer常见问题与回答总结](https://zhuanlan.zhihu.com/p/496012402?utm_medium=social&utm_oi=629375409599549440)
