# Introduction

perceptron是二类分类的线性**分类**模型，输入为实例的特征向量，输出为类别(+1 ,-1)

## preceptron

$$
f(x)=sign(\mathbf{w\cdot x}+b)=\begin{cases}
+1 & \mathbf{w\cdot x}+b \ge 0 \\
-1 & \mathbf{w\cdot x}+b < 0
\end{cases}
$$

其中$sign(x)$为符号函数:$sign(x)=\begin{cases} +1 & x>0 \\-1 & x<0 \end{cases}$

> 由模型的定义可知，perceptron是一种线性模型，具体一点，它是线性的分类模型。

感知机就是要找到n-1维的超平面来分离n维超平面$R^n$,把n维超平面上的点(即输入的特征向量)分为正负两部分。

## strategy

### 数据集的线性可分性

若存在某个超平面S,能够将数据集$T$的正负实例点完全正确的划分到超平面两侧，则称$T$为线性可分数据集，否则，称$T$线性不可分。

### 学习策略

$\forall x_o \in R^n$到超平面$S$的距离：

$$
\frac{1}{\parallel w \parallel} \mid w\cdot x_0+b\mid
$$

> n维空间上的点到超平面的距离公式，类似于几何中点到平面的公式，例如点$(x_0,y_0,z_0)$到平面$ax+by+cz+d=0$的距离公式为：$\frac{\mid ax_0+by_0+z_0+d \mid}{\sqrt{a^2+b^2+c^2}}$

对于误分类点$(x_i,y_i)$, 有：

$$
-y_i(w\cdot x_i+b)>0
$$

可得，误分类点$(x_i,y_i)$到超平面S的距离为：

$$
-\frac{1}{\parallel w \parallel}(w\cdot x_i+b)
$$

若误分类点的集合为$M$,则损失函数为：

$$
L(w,b)=-\sum_{x_i \in M}y_i(w\cdot x_i+b)
$$

参数估计：
$$
\mathop{\arg\min}\limits_{w,b}L(w,b)
$$


## algorithm

### 原始形式

求导计算$w$,$b$的梯度
$$
\nabla_w L(w,b)=-\sum_{x_i \in M}x_iy_i \\
\nabla_b L(w,b)=-\sum_{x_i \in M}y_i
$$

- Batch gradient descent

​	每次梯度下降都是使用batch里所有误分类点来计算loss
$$
w\leftarrow w+\alpha\nabla_w L(w,b) \\
b\leftarrow w+\alpha\nabla_b L(w,b)
$$


- Stochastic gradient descent

​	随机选取一个误分类点来计算loss
$$
w\leftarrow w+\alpha y_ix_i \\
b\leftarrow b+\alpha y_i
$$

### 对偶形式

对于某个误分类点$(x_i, y_i)$来说，对参数最终的影响为：在此误分类点进行梯度下降的次数$n_i$乘以在此点loss的梯度，即$n_i\cdot \alpha x_iy_i$ 

记$\eta_i=n_i\alpha$,则对于所有的误分类点$(x_i,y_i) \in M$,模型最终的参数为：
$$
w=\sum_{i=1}^{N}\eta_i y_ix_i \\
b=\sum_{i=1}^{N}\eta_i y_i
$$
则perceptron的对偶形式中，用上述最终的$w$替换原来的$w$,新的percetron模型为:
$$
f(x)=sign(\sum_{i=1}^{N}\eta_i y_ix_i\cdot x+b)
$$
误分类条件为：
$$
y_j(\sum_{i=1}^{N}\eta_i y_ix_i\cdot x_j+b)\le0
$$


这样就由输出$w$,变成输出$\eta=(\eta_1,\eta_2,...,\eta_N)$,其中$\eta_i$为第$i$个点被误分类的次数乘以学习率，即$n_i\alpha$

每次更新参数$\eta$只需让$n_i$自增,则更新参数的过程变为：
$$
\eta_i \leftarrow \eta_i +\alpha=(n_i+1)\cdot \alpha \\
b \leftarrow b+\alpha y_i=(n_i+1)\cdot \alpha y_i
$$
对于(12)式其中的$x_i\cdot y_i$,通过先计算gram矩阵来简化后续运算：
$$
G=[x_i \cdot y_i]_{N\times N}=
\begin{bmatrix}
x_1\cdot x_1 & x_1\cdot x_2 & \cdots & x_1 \cdot x_N \\
x_2\cdot x_1 & x_2\cdot x_2 & \cdots & x_2 \cdot x_N \\
\vdots & \vdots & \ddots & \vdots \\
x_N\cdot x_1 & x_N\cdot x_2 & \cdots & x_N \cdot x_N
\end{bmatrix}
$$
每轮对所有的样本都进行判断和参数更新，知道某一轮没有误分类点为止。

## 算法收敛性

> 记$\hat{w}=(w^T,b)^T$,$\hat{x}=(x^T,1)^T$,则分离超平面可以写为$\hat{w}\cdot \hat{x}=0$

### Novikkoff定理

若训练集$T=\{ (x_1,y_1),(x_2,y_2),\cdots ,(x_N,y_N) \}$线性可分。其中，$x_i \in \chi \subseteq \mathbf{R}^n$,$y \in \{ +1,-1 \}$,则，

(1)存在满足条件$\parallel \hat{w_{opt}} \parallel =1$的超平面$\hat{w_{opt}} \cdot \hat{x}=0$可将$T$完全正确分类；且$\exists \gamma > 0$，对所有$i=1,2, \cdots ,N$ 都有：
$$
y_i(\hat{w_{opt}}\cdot \hat{x_i}) \ge \gamma
$$
(2)令$R=\max_{1\le i\le N}\parallel \hat{x_i}\parallel$,则感知机算法在$T$上的误分类次数满足不等式：
$$
k \le(\frac{R}{\gamma})^2
$$


### 定理证明

(1)由于数据集线性可分，则存在超平面可将数据集完全分开，取此超平面为$\hat{w_{opt}}\cdot \hat{x}=w_{opt}\cdot x+b_{opt}=0$,等式两边整体加上系数不会影响等式的成立，不妨令$\parallel \hat{w_{opt}} \parallel=1$。

对于有限的被争取分类的点$i=1,2,\cdots,N$,均有：
$$
y_i(\hat{w_{opt}}\cdot x_i)=y_i(w_{opt}\cdot x_i+b_{opt})>0
$$
必然存在
$$
\gamma=\mathop{\min}\limits_{i}\{ y_i(w_{opt}\cdot x_i +b_{opt}) \}
$$
使得
$$
y_i(\hat{w_{opt}}\cdot x_i)=y_i(w_{opt}\cdot x_i+b_{opt})>\gamma
$$
(2)感知机算法从$\hat{w_0}$开始不断迭代更新权重向量，令$\hat{w_{k-1}}=(w^T_{k-1},b_{k-1})^T$

则第k个点被误分类的条件是：
$$
y_i(\hat{w_{k-1}}\cdot x_i)=y_i(w_{k-1}\cdot x_i+b_{k-1})\le0
$$
设$(x_i,y_i)$是被$\hat{w_{k-1}}$误分类的点，则$w_k$的更新为：
$$
\hat{w_k}=\hat{w_{k-1}}+\eta y_i\hat{x_i}
$$
下面推导两个不等式：
$$
\begin{cases}
\hat{w_k}\cdot\hat{w_{opt}}\ge k\eta \gamma \\
\parallel \hat{w_k} \parallel^2 \le k \eta^2R^2
\end{cases}
$$

- $\hat{w_k}\cdot\hat{w_{opt}}\ge k\eta \gamma$

$$
\begin{aligned}
\hat{w_k}\cdot\hat{w}_{opt}&=\hat{w}_{k-1}\cdot\hat{w}_{opt}+\eta {\color{red} y_i\hat{w_{opt}}\cdot\hat{x_i}} \\
&\ge\hat{w}_{k-1}\cdot\hat{w}_{opt}+\eta\gamma \\
&\ge\hat{w}_{k-2}\cdot\hat{w}_{opt}+2\eta\gamma \\
&\vdots \\
&\ge \hat{w}_0+k\eta\gamma=k\eta\gamma 
\end{aligned}
$$

- $\parallel \hat{w}_k \parallel^2 \le k\eta^2R^2$

$$
\begin{aligned}
\parallel \hat{w}_k \parallel^2&=\parallel \hat{w_{k-1}}+\eta y_i\hat{x_i}\parallel^2 \\
&=\parallel \hat{w}_{k-1} \parallel^2+2\eta {\color{red}y_i\hat{w}_{k-1}\cdot \hat{x}_i} +\eta^2\parallel \hat{x}_i\parallel^2 \\
&\le \parallel\hat{w}_{k-1} \parallel^2+\eta^2\parallel \hat{x}_i\parallel^2 \\
&\le \parallel\hat{w}_{k-1} \parallel^2+\eta^2R^2 \\
&\le \parallel\hat{w}_{k-2} \parallel^2+2\eta^2R^2 \\
&\vdots \\
&\le \parallel\hat{w}_0 \parallel^2+k\eta^2R^2=k\eta^2R^2
\end{aligned}
$$

其中，由于$(x_1y_i)$是误分类点，所以${\color{red}y_i\hat{w}_{k-1}\cdot \hat{x}_i}\le0$

结合(23)式和(24)式，得：
$$
k\eta\gamma\le\hat{w}_k\cdot\hat{w}_{opt}\le\parallel\hat{w}_k\parallel\cdot\parallel\hat{w}_{opt}\parallel\le\sqrt{k}\eta R
$$
则消去$\eta$,得
$$
k\gamma\le\sqrt{k}\eta R
$$
即
$$
k^2\gamma^2\le kR^2
$$
变换得
$$
k \le(\frac{R}{\gamma})^2
$$


