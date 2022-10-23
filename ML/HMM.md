# Hidden Markov Model

## preliminaries

### Gaussian Distribution

若随机变量$X$服从一个参数为$\mu$,方差为$\sigma^2$的概率分布，且概率密度函数为：
$$
f(x)=\frac{1}{\sqrt{2\pi\sigma^2}}\exp(-\frac{(x-\mu)^2}{2\sigma^2})
$$
则这个随机变量就称为正态随机变量，随机变量的分布就是高斯分布，记做$X \sim \mathcal{N}(\mu,\sigma^2)$

$\mu=0,\sigma=1$时，称为标准高斯分布。

高斯分布的性质：

- 概率密度曲线在均值μ处达到最大，并且对于$x=\mu$对称
- 正态随机变量在特定区间上的取值概率由正态曲线下的面积给出，而且其曲线下的总面积等于1
- 标准差决定了概率密度曲线的“陡峭”或“扁平”程度，标准差越大，曲线越扁平；标准差越小，曲线越陡峭。这是因为，标准差越小，意味着大多数变量距离均值的距离越短，也就是说大多数变量都紧密地聚集在均值周围，图形所能覆盖的变量值就少些，图形上呈现瘦高型。相反，标准差越大，数据跨度就越大，分散程度越大，所覆盖的变量就越多，图形呈现“矮胖型”。

### Multivariate Gaussian Distribution

#### Mathematic Expectation

期望就是随机变量不同的取值与它们的概率的加权求和

- 离散型随机变量
    $$
    E(x)=\sum_{k=1}^\infty x_kp_k
    $$

- 连续型随机变量
    $$
    E(X)=\int_{-\infty}^{\infty} xf(x)dx
    $$
    

#### Variance

总体方差公式：
$$
\sigma^2=\frac{\sum(X-\mu)^2}{N}
$$
$\sigma^2$为样本方差,$X$为随机变量,$\mu$为总体均值,$N$为总体例数

实际情况中，难以对总体进行计算时，采用抽样的方法：
$$
S^2=\frac{\sum(X-\bar{X})^2}{n-1}
$$
$\bar{X}$为样本均值,$n$为总体例数

在概率分布中，期望的定义为：“*随机变量值与其期望值之差的平方*的期望值”

- 离散型随机变量
    $$
    D(X)=E[X-E(X)]^2=E(X^2)-E^2(X)
    $$
    
- 连续型随机变量
    $$
    D(X)=\int(x-\mu)^2f(x)dx
    $$

#### Covariance

协方差用于判断两个随机变量之间的相关性

期望值分别为$E(X)$与$E(Y)$的两个实随机变量*X*与*Y*之间的**协方差**$Cov(X,Y)$定义为:
$$
\begin{align}
Cov(X,Y)&=E[(X-E[X])(Y-E[Y])]\\
&=E[XY]-2E[Y]E[X]+E[X]E[Y]\\
&=E[XY]-E[X]E[Y]
\end{align}
$$
方差是协方差的特殊情况，即$Cov(X,X)=D(X)$

> 若两个随机变量X和Y相互独立，则$E[(X-E(X))(Y-E(Y))]=0$
>
> 若正相关，$Cov(X,Y)>0$;若负相关，$Cov(X,Y)<0$

与$(5)$式同样的，在实际情况中，协方差被定义为：
$$
Cov(X,Y)=\frac{\sum_{i=1}^n(x_i-\bar{x})(y_i-\bar{y})}{n-1}
$$

#### Covariance Metrix

根据协方差的定义，可以不同随机变量间的协方差矩阵

给定多维随机变量$(X_1,X_2,\cdots,X_N)^T$,其中两两之间的协方差：
$$
Cov(X_m，X_k)=\frac{\sum_{i=1}^n(x_{mi}-\bar{x_m})(x_{ki}-\bar{x_k})}{n-1}
$$
协方差矩阵为：
$$
\sum=
\begin{bmatrix}
Cov(X_1,X_1) & Cov(X_1,X_2) & \cdots & Cov(X_1,X_N) \\
Cov(X_2,X_1) & Cov(X_2,X_2) & \cdots & Cov(X_2,X_N) \\
\vdots & \vdots & \ddots & \vdots \\
Cov(X_N,X_1) & Cov(X_N,X_2) & \cdots & Cov(X_N,X_N)
\end{bmatrix}
\in \mathbb{R}^{N \times N}
$$

#### Multivariate Gaussian Distribution

$$
f_x(x_1,\cdots,x_N)=\frac{1}{\sqrt{(2\pi)^2 \mid\sum\mid}}\exp(-\frac{1}{2}(\mathbf{x}-\mathbf{\mu})^T\textstyle\sum^{-1}(\mathbf{x}-\mathbf{\mu}))
$$

