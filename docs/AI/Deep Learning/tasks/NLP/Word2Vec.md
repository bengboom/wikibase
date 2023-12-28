## 1. Word2vec 概述
Word2vec 是一种流行的词嵌入技术，由 Google 的 Tomas Mikolov 等人于 2013 年提出。它能将词转换为向量形式，捕捉词之间的语义关系。

## 2. Word2vec 的两种模型
- **CBOW (Continuous Bag of Words)**
  - 预测目标词基于上下文。
  - 公式：$P(w|context) = \frac{\exp(v'_{w} \cdot \bar{v}_{context})}{\sum_{w' \in W} \exp(v'_{w'} \cdot \bar{v}_{context})}$
  - 其中 $v'_{w}$ 是目标词 $w$ 的输出向量，$\bar{v}_{context}$ 是上下文的平均向量。

- **Skip-Gram**
  - 从目标词预测上下文。
  - 公式：$P(context|w) = \prod_{w_{c} \in context} \frac{\exp(v'_{w_{c}} \cdot v_{w})}{\sum_{w' \in W} \exp(v'_{w'} \cdot v_{w})}$
  - 其中 $v_{w}$ 是目标词 $w$ 的输入向量，$v'_{w_{c}}$ 是上下文中每个词 $w_{c}$ 的输出向量。

## 3. 训练细节
- **优化**：使用负采样 (Negative Sampling) 或层次 Softmax (Hierarchical Softmax) 来提高效率。
- **负采样**：选取少量的负样本来更新权重，而非整个词汇表。
  - 公式：$\log \sigma(v'_{w_{O}} \cdot v_{w_{I}}) + \sum_{i=1}^{k} \mathbb{E}_{w_{i} \sim P_{n}(w)} [\log \sigma(-v'_{w_{i}} \cdot v_{w_{I}})]$
  - 其中 $\sigma$ 是 Sigmoid 函数，$w_{O}$ 是目标词，$w_{I}$ 是输入词，$k$ 是负样本的数量。

## 4. 应用
- Word2vec 模型在自然语言处理中广泛应用，如文本分类、情感分析和机器翻译等领域。

## 5. 引用
- Tomas Mikolov et al., "Efficient Estimation of Word Representations in Vector Space" (2013a)
- Tomas Mikolov et al., "Distributed Representations of Words and Phrases and their Compositionality" (2013b)
