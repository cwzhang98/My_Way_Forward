# k-nearest neighbour

## introduction

是一种基本的分类与回归方法。k-nn假设给定一个训练数据集，其中的实例类别已定。输入新的实例时，根据最近的k个训练实例的类别，同过多数表决等方式进行预测。

三个基本要素：

- k值的选择
- 距离度量
- 分类决策规则

主要用于解决分类和回归问题：

- 分类问题：对新的实例，根据与之相邻的k个训练实例的类别，通过多数表决等方式进行预测
- 回归问题：对新的实例，根据与之相邻的k个训练实例的标签，通过均值计算方式进行预测

> k-nn没有显式的学习过程

## algorithm

- 训练集：$T=\{ (x_1,y_1),(x_2,y_2),\cdots ,(x_N,y_N) \}$。其中，$x_i \in \chi \subseteq \mathbf{R}^n$,$y \in \{ c_1,c_2,\cdots,c_K\}$
- 输入：实例$x$
- 输出：实例$x$所属的类别$y$

(1)根据给定的距离度量，计算x与所有点的距离

(2)与x最近的k的点的领域记作$N_k(x)$

(3)根据分类决策规则，决定x的类别y
$$
y=\mathop{\arg\max}_{c_j}\sum_{x \in N_k(x)} I(y_i=c_j)
$$

## error rate

k=1时，对于新实例$x$，$x_i$为距离它最近的训练实例，两者所属类别分别记为$a$和$b$

误差率为：


$$
\begin{align}
Err(x,x_i)=P(a\ne b \mid x,x_i)&=\sum_{j=1}^{K}P(a=c_j,b\ne c_j\mid x,x_i) \\
&=\sum_{j=1}^{K}P(a=c_j\mid x)P(b\ne c_j\mid x_i) \\
&=\sum_{j=1}^{K}P(a=c_j\mid x)(1-P(b=c_j\mid x_i))
\end{align}
$$
当$K \rightarrow \infty$时，

>  $(5)$式说明当K趋近于无穷大时，$x$的类别由$x_i$的类别决定

$$
\lim_{K \to \infty}P(b=c_j\mid x_i)=P(a=c_j\mid x)
$$

$$
\begin{align}
Err(x,x_i)&=\sum_{j=1}^{K}P(a=c_j\mid x)(1-P(b=c_j\mid x_i)) \\
&\rightarrow \sum_{j=1}^K P(a=c_j\mid x)-\sum_{j=1}^K P^2(a=c_j\mid x) \\
&=1-\sum_{j=1}^kP^2(a=c_j\mid x)
\end{align}
$$

假设$x$的真实类别为$c^*$,
$$
c^*=\mathop{\arg\max}_{c_j}P(c_j\mid x)
$$
其对应的贝叶斯误差率为
$$
P^*(err\mid x)=1-P(c^*\mid x)
$$
$(8)$式中的第二项：
$$
\begin{align}
\sum_{j=1}^kP^2(a=c_j\mid x)&=P^2(c^*\mid x)+\sum_{c_j\ne c^*}P^2(c_j\mid x) \\
&\ge P^2(c^*\mid x)+\sum_{c_j\ne c^*}(\frac{1-P(c^*\mid x)}{K-1})^2 \\
&=P^2(c^*\mid x)+\frac{(1-P(c^*\mid x))^2}{K-1} \\
&=(1-P^*)^2+\frac{(P^*)^2}{K-1}
\end{align}
$$

>  注：
>
> $(19)$式中的$\sum_{c_j\ne c^*}P^2(c_j\mid x)$共有$K-1$项，当这$K-1$项都相等时，则此平方求和取最小值。这$K-1$项的均值即为$\frac{1-P(c^*\mid x)}{K-1}$
>
> $(21)$到$(22)$式的推导，由$(18)$式得出

则误差率：
$$
Err(x,x_i)\le 1-(1-P^*)^2-\frac{(P^*)^2}{K-1}=2P^*-\frac{K}{K-1}(P^*)^2
$$
当$P^*$较小时,$Err(x,x_i)$的上界近似于$2P^*$,则
$$
P^* \le Err(x,x_i)\le 2P^*
$$
推广到k>1的情况，当$N\rightarrow \infty$,且$K\rightarrow\infty$时，$Err(x)\rightarrow P^*$

>  k足够大，但相对于N足够小时，即使在大样本数量下，k-nn也近似于最邻近法(k=1时的情况)

## factors

- $L_p$距离

    特征空间假设为$R^n$,$\forall x_i,x_j,x_i=(x_i^{(1)},x_i^{(2)},\cdots,x_i^{(n)})^T,x_j=(x_j^{(1)},x_j^{(2)},\cdots,x_j^{(n)})^T$,则有
    $$
    L_p(x_i,x_j)=(\sum_{k=1}^n\mid x_i^{(k)}-x_j^{(k)}\mid^p)^{\frac{1}{p}}
    $$

    - Euclidean distance($L_2$ norm)

    $$
    L_2(x_i,x_j)=(\sum_{k=1}^n\mid x_i^{(k)}-x_j^{(k)}\mid^2)^{\frac{1}{2}}
    $$

    - Manhattan distance($p=1$)

    $$
    L_1(x_i,x_j)=\sum_{k=1}^n\mid x_i^{(k)}-x_j^{(k)}\mid
    $$

    

    - Chebyshev distance($p \rightarrow \infty$)

- 

$$
L_{\infty}(x_i,x_j)=\max_k\mid x_i^{(k)}-x_j^{(k)}\mid
$$

- Choice of k value
    - 较小的k值，训练误差小，测试误差大，模型更复杂，容易过拟合
    - 较大的k值，减少在验证集上的误差，但训练误差增大，模型更简单

> k的取值可以通过交叉验证来选择，一般低于训练集样本量的平方根

- Decision rule of classification

 多数表决规则：由输入实例的k个邻近的训练实例中的多数类来决定此输入实例的类。

分类函数：
$$
f:R^n \rightarrow \{c_1,c_2,\cdots,c_K \}
$$
损失函数：
$$
L(Y,f(X))=
\begin{cases}
1 &Y\ne f(X) \\
0 &Y=f(X)
\end{cases}
$$
误分类概率：
$$
P(Y\ne f(X))=1-P(Y=f(X))
$$
给定实例$x\in \chi$，相应的$N_k(x)$的类别为$c_j$,误分类率=误分类个数/k
$$
\frac{1}{k}\sum_{x_i\in N_k(x)}I(y_i\ne c_j)=1-\frac{1}{k}\sum_{x_i\in N_k(x)}I(y_i=c_j)
$$

> 最小化等式左侧的分类误差率，相当于最大化等式右侧的$\frac{1}{k}\sum_{x_i\in N_k(x)}I(y_i=c_j)$,即：
> $$
> \mathop{\arg\max}\sum_{x_i\in N_k(x)}I(y_i=c_j)
> $$

## kd tree
