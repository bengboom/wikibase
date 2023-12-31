> 机器学习 构件一个映射函数 $y=f(x;\theta)$
> 机器学习：从数据中获得决策（预测）函数使得机器可以根据数据进行自动学习，通过算法使得机器从大量历史数据中学习规律从而对新的样本做决策![[Pasted image 20231208153334.png]]

## 基本术语
- 泛化能力
- 数据
- 分类、回归、聚类
## 基本流程

### 数据采集
所有的机器学习算法都是**数据贪婪**的，任何一个算法都可以通过增加数据来达到更好的效果
采集方式：爬虫、公开数据集、数据库
### 数据预处理
缺失处理、异常处理
### 特征工程
特征构建：指的是从原始数据中**人工的构建新的特征**。需要人工的创建它们。这需要花大量的时间去研究真实的数据样本，思考问题的潜在形式和数据结构，同时能够更好地应用到预测模型中。
特征组合、特征拆分
### 模型的构建和调优
有无label，有监督还是无监督
分类问题还是回归问题
多模型融合

调优问题，可以采用交差验证，观察损失曲线，测试结果曲线等分析原因，调节参数：优化器、学习率、batchsize等。

### 模型的评价
▪模型选择是在某个模型类中选择最好的模型，而模型评价对这个最好的模型进行评价。模型评价阶段不做参数调整而是客观地评价模型的预测能力。
▪混淆矩阵：主要用于比较分类结果和实例的真实信息。矩阵中的每一行代表实例的预测类别，每一列代表实例的真实类别。![[Pasted image 20231208152016.png]]
真阳性（True Positive，TP）：样本的真实类别是正例，并且模型预测的结果也是正例
真阴性（True Negative，TN）：样本的真实类别是负例，并且模型将其预测成为负例
假阳性（False Positive，FP）：样本的真实类别是负例，但是模型将其预测成为正例
假阴性（False Negative，FN）：样本的真实类别是正例，但是模型将其预测成为负例

准确率 $Acc = \frac{TP+TN}{all}$
精确率 查准率 $Precision = \frac{TP}{TP+FP}$
召回率 $Recall = \frac{TP}{TP+FN}$

F1 score 精确率和召回率的调和值
$F_1=\frac{(1+\beta ^2) PR}{P+R}=\frac{2TP}{all-TP+TN}$
更多的，$F_\beta$
$F_{\beta} = \frac{(1 + \beta^2) \times Precision \times Recall}{(\beta^2 \times Precision) + Recall}$
β=1:标准F_1
β> 1：偏重查全率（逃犯信息检索）
β< 1：偏重查准率（商品推荐系统）

平均绝对值误差 $MAE = \frac{1}{m} \sum_{i=1}^{m} |h(x_i) - y_i|$
均方误差 $MSE = \frac{1}{m} \sum_{i=1}^{m} (h(x_i) - y_i)^2$
均方根误差   $RMSE = \sqrt{\frac{1}{m} \sum_{i=1}^{m} (h(x_i) - y_i)^2}$

## 模型评估与选择

### 经验误差与过拟合
错误率 误差
过拟合 欠拟合

### 评估方法
[[Hold out 留出法]]
[[Cross Validation]]
[[Leave-One-Out]]
[[Bootstrapping]]

### 性能度量
▪性能度量是衡量模型泛化能力的评价标准，反映了任务需求；使用不同的性能度量往往会导致不同的评判结果。
▪回归任务最常用的性能度量是“均方误差”。

P-R曲线：根据学习器的预测结果按正例可能性大小对样例进行排序，并逐个把样本作为正例进行预测，则可以得到查准率-查全率曲线。![[Pasted image 20231217161119.png]]

ROC曲线 Receiver Operating Characteristic
![[Pasted image 20231217161149.png]]

### 偏差与方差 拟合
模型不佳时，通常是出现两种问题，一种是高偏差问题，另一种是高方差问题，识别它们有助于选择正确的优化方式。
- 偏差度量了学习算法期望预测与真实结果的偏离程度；即刻画了学习算法本身的拟合能力。
- 方差度量了同样大小训练集的变动所导致的学习性能的变化；即刻画了数据扰动所造成的影响。

刚开始，误差都很大，次数刚好时，训练集和验证集误差都很小，次数过大时会产生过拟合，验证集误差变大。学习曲线是一个十分直观有效的工具。**学习曲线的横轴是样本数，纵轴为训练集和交叉验证集的误差。**
在一开始：训练误差基本没有，验证误差很大。
然后：训练误差不断增大，验证误差因为拟合数据更多，下降。
**高偏差**：训练误差和验证误差很接近，但都很大。这样的情况下，增加样本数不能给算法的性能带来提升。
**高方差**：训练误差较小，验证误差比较大。这是搜集更多的样本很可能带来帮助。

一般来说，偏差与方差是有冲突的，**称为偏差-方差窘境。**![[Pasted image 20231217161259.png]]

改进策略：
- 高方差
	- 采集更多样本
	- 减少特征数量
	- 增加正则化参数$\lambda$
- 高偏差
	- 引入更多特征
	- 采用多项式特征
	- 减少$\lambda$

## 单变量线性回归
$f(x)=w\cdot x + b$
learn a line, get $w, b$
predict $\hat{y}$ given new $x$
$Loss(w,b)=\frac{1}{2m} \Sigma_{i=1}^{m} (f(x_i)-y_i)^2$
> 分母中的 $\frac{1}{2}$ 用于在求导时简化计算，使得导数的表达式更简洁。

最小二乘法的思路，梯度下降
学习率的影响![[Pasted image 20231217162125.png]]


## 多变量线性回归
特征是多维了。$x_i=\left[\begin{array}{l}x_{i 1} \\ x_{i 2} \\ \ldots \\ x_{i n}\end{array}\right]$，同理，$w$也一样

不同的是：x上面补一个1，w里面第一项就能代表常数项
$f\left(x_i\right)=w^T x=\left(\begin{array}{llll}w_0 & w_1 & \ldots & w_n\end{array}\right)\left(\begin{array}{c}1 \\ x_{i 0} \\ \ldots \\ x_{i n}\end{array}\right)=w_0+w_1 x_{i 1}+\ldots+w_n x_{i n}$
梯度下降，多元函数求导

## 决策树
决策树学习的关键在于**如何选择最优划分属性**。一般而言，随着划分过程不断进行，我们希望决策树的分支结点所包含的样本**尽可能属于同一类别，即结点的“纯度”(purity)越来越高**
![[Pasted image 20231217165616.png]]

### 划分方法
#### 信息熵
信息熵是度量样本集合纯度最常用的一种指标，假定当前样本集D 中第 $\mathrm{k}$ 类样本所占的比例为 $\mathrm{p}_k=(K=1,2, \ldots|Y|)$,则 $\mathrm{D}$ 的信息摘定义为
$$
\operatorname{Ent}(D)=-\sum_{k=1}^{|Y|} p_k \log p_k
$$
$\operatorname{Ent}(D)$ 的值越小，则 $\mathrm{D}$ 的纯度越高
- 计算信息摘时约定: 若 $p=0$,则 $p \cdot \log p=0$
- $\operatorname{Ent}(D)$ 的最小值为 0 ，最大值为 $\log |Y|$
#### 信息增益
离散属性 $a$ 有 $\mathrm{V}$ 个可能的取值 $\left\{a^1, a^2, \ldots, a^V\right\}$, 用 $\mathrm{a}$ 来进行划分，则会产生 $\mathrm{V}$ 个分支结点，其中第 $\mathrm{v}$ 个分支结点包含了 D中所有在属性a上取值为 $\mathrm{a}^{\mathrm{v}}$ 的样本，记为 $D^V$ 。则可计算出用属性对样本集 $\mathrm{D}$ 进行划分所获得的“信息增益”:
- $\operatorname{Gain}(D, a)=\operatorname{Ent}(D)-\sum_{v=1}^V \frac{\left|D^v\right|}{|D|} \operatorname{Ent}\left(D^v\right)$
为分支结点权重，样本数越
多的分支结点的影响越大
- 一般而言，信息增益越大，则意味着使用属性 $a$ 来进行划分所获得的“纯度提升”越大
ID3决策树学习算法[Quinlan, 1986]以信息增益为准则来选择划分属性
#### 基尼系数
- **定义**：度量样本集合的不确定性或纯度的指标。
- **公式**：$\text{Gini}(D) = 1 - \sum_{k=1}^{|Y|} p_k^2$，其中$p_k$是第k类样本在样本集D中的比例。
- **意义**：
    - 基尼系数越小，样本集合的纯度越高。
    - 范围在0到1之间，0表示完全纯净，1表示最大不确定性。
- **应用**：在CART决策树算法中用于选择最佳划分属性。
- **计算方式**：对每个属性a，计算加权基尼系数
$\text{Gini\_index}(D, a) = \sum_{v=1}^{V} \frac{|D^v|}{|D|} \text{Gini}(D^v)$
- 选择使**基尼系数降低最多**的属性作为划分属性。
#### 关键点
- 基尼系数反映了样本集合的类别分布不均匀程度。
- 在决策树中，选择基尼系数最小的属性作为划分属性，以提高分类的准确性。
## K均值聚类
给定数据集 $D=\left\{x_1, x_2, \cdots, x_m\right\} ， k$ 均值算法针对聚类所得簇划分 $C=\left\{C_1, C_2, \cdots, C_k\right\}$ 最小化平方误差
$$
E=\sum_{i=1}^k \sum_{x \subset C_j}\left\|x-\mu_i\right\|_2^2
$$

其中， $\mu_i$ 是簇 $C_i$ 的均值向量。
$E$ 值在一定程度上刻画了簇内样本围绕簇均值向量的紧密程度, $E$值越小，则簇内样本相似度越高。

无监督学习，划分为cluster
![[Pasted image 20231217165138.png]]
标准算法![[Pasted image 20231217165507.png]]

## 支持向量机 SVM
找一个超平面，让间隔最大。![[Pasted image 20231217165449.png]]

凸二次规划QP的求解需要4个参数：Q(二次项系数), p(一次项系数), A(条件中的系数), c(条件中的常数)将SVM的表达形式与之一一对应就可以。
得出四个系数之后，使用现成的计算包就可以轻而易举的解决SVM问题。
![[Pasted image 20231217165243.png]]

