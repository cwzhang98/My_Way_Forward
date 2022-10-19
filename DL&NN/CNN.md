

# Convolutional Neural Network

## Introduction

- CNN出现之前，在用全连接前馈网络来处理图像时，会存在以下两个问题：
    - 参数过多：图像的每个像素都有三个RGB颜色通道，若作为FN的输入，会导致FN有过多的参数
    - 局部特征不变形：FN很难提取局部不变性特征

- 网络结构可以简单分为：

    - 卷积层
    - 汇聚层
    - 全链接层

    一般由上述三种网络层交叉堆叠而成

- 特性：

    - 局部连接
    - 权重共享
    - 汇聚

    使得卷积神经网络能够提取局部不变性特征，同时减少网络的参数

## Convolution

### 1-D Convlution

经常用在信号处理中，用于计算信号的延迟累积。

假设信号序列为$x_1,x_2,\cdots,x_t$,其信息的衰减率为$w_k$,在$k-1$个时刻后，信息衰减为原来的$w_k$倍，则在时刻$t$收到的信号$y_t$为当前时刻产生的信息和以前时刻延迟信息的叠加：
$$
y_t=\sum_{k=1}^K w_k x_{t-k+1}
$$
$w_1,w_2$称为Filter或Convlutional Kernel,$y_t$为卷积的结果

信号序列$\mathbf{x}$和滤波器$\mathbf{w}$的卷积定义为:$\mathbf{y}=\mathbf{w}*\mathbf{x}$，一般情况下滤波器的长度$K$远小于信号序列$\mathbf{x}$的长度，所以卷积时需要移动窗口。

### 2-D Convlution

二维卷积常用在图像处理中，将一维卷积进行拓展：
$$
\mathbf{X} \in \mathbb{R}^{M \times N} \\
\mathbf{W} \in \mathbb{R}^{U \times V}
$$
其卷积为：
$$
y_{i,j}=\sum_{u=1}^U \sum_{v=1}^V W_{u,v} x_{i-u+1,j-v+1}
$$
输入信息$\mathbf{Y}$和滤波器$\mathbf{W}$的二维卷积定义为：$\mathbf{Y}=\mathbf{W}*\mathbf{X}$

> 在图像处理中常用的均值滤波就是将当前位置的像素值设为卷积核窗口中所有像素的平均值，即$w_{u,v}=\frac{1}{UV}$

**一幅图像在经过卷积操作后得到结果称为特征映射(Feature Map)**

### Cross-Correction

在上述的卷积操作中，可以注意到特征序列和卷积核的序列都是相反的，例如式$(1)$中，$x$的序列为${x_t,x_{t-1},\cdots,x_1}$而卷积核的序列为${w_1,w_2,\cdots,w_K}$

而在深度学习中，要对卷积核进行翻转，也就是使这两个序列都变成正向序列。

在完成翻转后，在进行“卷积”的操作，这样的操作称为**互相关**
$$
y_{i,j}=\sum_{u=1}^U \sum_{v=1}^V W_{u,v} x_{i+u-1,j+v-1}
$$

> 互相关操作也称为不翻转卷积。式$(4)$可以表述为：
> $$
> \begin{align}
> \mathbf{Y}&=\mathbf{W}\otimes\mathbf{X} \\
> &=rot180(\mathbf{W}*\mathbf{X})
> \end{align}
> $$
>

### 卷积的变种

在卷积运算的过程中，还需要定义卷积核的Stride和Zero Padding。以此增加卷积的多样性

- Stride是卷积核滑动的间隔
- Zero Padding是在输入向量上补零

若输入神经元(即特征数量)的个数为M，卷积大小为$K$，步长为$S$,两端的Zero Padding为$P$,那么卷积层的神经元数量(即卷积层的输出长度)为：
$$
L=\frac{M-K+2P}{S}+1
$$
常用卷积：

- Narrow Convolution: $S=1,P=0,L=M-K+1$
- Wide Convolution: $S=1,P=K-1,L=M+K-1$
- Equal-Width Convolution: $S=1,P=\frac{K-1}{2},L=M$

### 数学性质

 #### 交换性

真正的卷积运算是具有交换性的:
$$
x*y=y*x
$$
Cross-Corralation也有一定的*交换性*

定义，$\mathbf{W} \in \mathbb{R}^{U \times V}$,$\mathbf{\tilde{X}} \in \mathbb{R}^{(M+2U-2)\times(N+2V-2)}$,Wide Convolution: $\mathbf{W} \tilde{\otimes} \mathbf{X}=\mathbf{W} \otimes \tilde{\mathbf{X}}$

当输入信息和卷积核有固定长度时，它们依然有交换性：
$$
rot180(\mathbf{W}) \tilde{\otimes} \mathbf{X}=rot180(\mathbf{X}) \tilde{\otimes} \mathbf{W}
$$

#### 导数

假设$\mathbf{Y}=\mathbf{W}\otimes\mathbf{X}$,$\mathbf{X} \in \mathbb{R}^{M \times N}$,$\mathbf{W} \in \mathbb{R}^{U \times V}$,$\mathbf{Y} \in \mathbb{R}^{(M-U+1)\times(N-V+1)}$,函数$f(\mathbf{Y})\in \mathbb{R}$为一个标量函数，则有：
$$
\frac{\partial f(\mathbf{Y})}{\partial\mathbf{W}}=\frac{\partial f(\mathbf{Y})}{\partial\mathbf{Y}}\otimes\mathbf{W}
$$

$$
\begin{align}
\frac{\partial f(\mathbf{Y})}{\partial\mathbf{X}}&= \frac{\partial f(\mathbf{Y})}{\partial\mathbf{Y}} * \mathbf{W}\\
&=rot180(\frac{\partial f(\mathbf{Y})}{\partial\mathbf{Y}})\tilde{\otimes}\mathbf{W} \\
&=\frac{\partial f(\mathbf{Y})}{\partial\mathbf{Y}}\tilde{\otimes}rot180(\mathbf{W})
\end{align}
$$

## Convolutional Neural Network

用卷积来代替全连接，可以显著地减少参数量。$\mathbf{z^{(l)}}=\mathbf{w^{(l)}} \otimes \mathbf{a^{(l)}}+b^{(l)}$

卷积层的两个重要性质:

- 局部连接:由全连接的$M_{l}\times M_{l-1}$个参数缩减为$M_{l}\times K$个参数，$K$为卷积核大小
- 权重共享:一个卷积核上的权重是被同一层输入的所有像素共享的

### Convolutional Layer

通常，如果是彩色图片，会有RGB三个通道的Feature Map,灰度图片会有一个Feature Map

卷积层的结构如下：

- Input Feature Map Set:$X \in \mathbb{R}^{M\times N \times D}$ is a three dimension tensor, every slice$X^d \in \mathbb{R}^{M\times N}$ is a input feature map unit, $1 \le d \le D$
- Output Feature Map Set:$Y \in \mathbb{R}^{M'\times N' \times P}$ is a three dimension tensor,every slice$Y^p \in \mathbb{R}^{M'\times N'}$ is a output feature map unit, $1 \le p \le P$
- Convolutional Kernel: Every $Y^p \in \mathbb{R}^{M'\times N'}$ is a result of $\sum_{d=1}^D W^{p,d} \otimes X^d$,so Kernel is a four dimension tensor $W \in \mathbb{R}^{U\times V\times P\times D}$

P Feature Maps ${Y^1,Y^2,\cdots,Y^P}$ compose $Y$ 

### Pooling Layer

卷积运算之后，Feature Map中的参数数量并没有显著减少，通过Pooling(Subsampling Layer)进行特征选择，进一步减少特征数量。

假设Convolution Layer的输出，也就是Pooling Layer的输入Feature Map: $\mathcal{X} \in \mathbb{R}^{M\times N\times D}$,对于其中的每一个Feature Map $X^d \in \mathbb{R}^{M\times N}$,将其划分为很多不同的区域$R_{m,n}^d$,其中下标$m,n$为区域的标号。

对这些划分出来的区域做Subsampling,就是Pooling

- Maximum Pooling:选择这个区域的最大值作为这个区域的表示
    $$
    y_{m,n}^d = \max_{i \in R_{m,n}^d} x_i
    $$

- Mean Pooling:取区域内的均值作为这个区域的表示
    $$
    y_{m,n}^d=\frac{1}{\mid R_{m,n}^d\mid} \sum_{i \in R_{m,n}^d} x_i
    $$

对每一个Input Feature Map $X^d,d \in [1,D]$的$M' \times N'$个子区域 Subsampling,得到Pooling Layer的 output Feature Map set$Y \in \{y_{m,n}^d\}$

### The overall structure of convolutional networks

一个卷积块由连续的M个Convolutional Layer和b个Pooling Layer组成，通常M设置成2～5，b为0或1

> 也就是说，Pooling Layer不是必须的，神经网络更趋向于全卷积网络

一个卷积网络中会堆叠多个卷积块，后面再接Full-Connected Network

卷积网络的整体结构更倾向于使用更小的Convolutioal layer和更深的Network Structure
