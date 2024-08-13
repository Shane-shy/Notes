# 归一化指数函数——Softmax函数

## 概念与应用

Softmax函数常用于多分类任务，将模型输出值归一化到[0,1]范围内，作为样本的概率。二分类可以看作是多分类的一种。因此，Softmax函数可以兼容logistics函数。logistics可以将输出归一化到[0,1]，但是仅输出正类的概率值。Softmax可以输出每一个样本对应的概率值。实际使用中，常在Softmax归一化结果中，选择概率最大值作为分类结果。

**基本特性：**

1. **归一化**：将数值限定在范围[0,1]内，并且所以数值之和为1。
2. **放大效果**：两个数值的大小差距不大，但是通过指数运算，会有明显的放大效果。

## 运算过程

### 公式

$$
softmax(X_j) = \frac {e^{X_j}}{\sum_{i = 1}^N e^{x_i}}
$$

### 示例

假设输入向量$\begin{pmatrix} 1.4 \\ -0.1 \\ 0.3 \end{pmatrix}$，经过softmax过程如下。
$$
\begin{pmatrix} 1.4 \\ -0.1 \\ 0.3 \end{pmatrix} 

\implies

\begin{pmatrix}
\frac{e^{1.4}}{e^{1.4} + e^{-0.1} + e^{0.3}} \\
\frac{e^{-0.1}}{e^{1.4} + e^{-0.1} + e^{0.3}} \\
\frac{e^{0.3}}{e^{1.4} + e^{-0.1} + e^{0.3}} 
\end{pmatrix}

\implies

\begin{pmatrix}
0.643 \\ 
0.143 \\
0.214
\end{pmatrix}
$$


## 损失函数

### 熵

定义：为了确保完整的信息被描述所需要的编码长度。来源于信息论。即，用N进制表示需要多少位（bit）。

例如：求26个字母的信息量。26个字母出现的概率相同，如果用一个信息位表示两个信息（二进制），则信息量为$log_x^{\frac{1}{p}} = log_2^{\frac{1}{26}} \approx 5$

### 交叉熵

实际情况都不是均匀分布或者是未知分布，因此公式改为$H(p,q) = \sum_{i=1}^N p_i log_x^{\frac{1}{q_i}}$，其中$p_i$表示真实概率，$q_i$表示预测概率。

## 参考文献

- [机器学习——softmax计算](https://www.chkui.com/article/tensorflow/math_in_machine_learning_softmax)
- [softmax介绍+公式+代码](https://blog.csdn.net/xu380393916/article/details/102496419)
- [【深度学习 搞笑教程】16 softmax回归 | 草履虫都能听懂 零基础入门 | 持续更新](https://www.bilibili.com/video/BV1Cw411C71D/?spm_id_from=333.337.search-card.all.click&vd_source=f5fa465fb229f9bbfa11556291b96b03)