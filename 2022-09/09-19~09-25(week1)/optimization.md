# Momentum

在梯度下降的过程中，对抗local minima或saddle point。源于物理概念中的动量，在梯度下降的过程中，加入上一个阶段的梯度。借此走出局部最小值。

> Movement: movement of **last step** minus gradient at present

- Starting at $\theta^0$
- Movement $m^0 = \mathbf{0}$
- Compute gradient $g^0$
- Movement $m^q=\lambda m^0-\eta g^0$
- Move to $\theta^1=\theta^0+m^1$
- Compute gradient $g^1$
- Movement $m^2=\lambda m^1-\eta g^1$ 

则有：

$m^t=\lambda m^{t-1}-\eta g^{t-1}$,

$\theta^t=\theta^{t-1}+m^t$

# Adaptive Learning Rate

> 通常固定的学习率可能使得在梯度下降的过程中loss不能下降到crtical point。通过自适应学习率可以进行优化。

参数$\theta_j$的梯度下降策略优化为：$\theta _j^{t+1}\longleftarrow \theta _j^t-\frac{\eta}{\sigma _j^t}g_i^t$

其中的$\theta _j^t$是依赖于参数的，借此达到变化学习率的目的

- root mean square

$\theta _j^{1}\leftarrow \theta _j^0-\frac{\eta}{\sigma _j^0}g_i^0$	                $\sigma _j^0=\sqrt{(g_j^0)^2}=\lvert g_j^0\rvert$

$\theta _j^{2}\leftarrow \theta _j^1-\frac{\eta}{\sigma _j^1}g_i^1$                    $\sigma _j^1=\sqrt{\frac{1}{2}[(g_j^0)^2+(g_j^1)^2]}$

由此推得：

$\theta _j^{t+1}\leftarrow \theta _j^t-\frac{\eta}{\sigma _j^t}g_i^t$                 $\sigma _j^t=\sqrt{\frac{1}{t+1}\sum_{i=0}^{t}(g_i^t)^2}$

若梯度加大则梯度$g_i^t$的均值增大，学习率减小，反之同理。

- RMSProp

$\theta _j^{1}\leftarrow \theta _j^0-\frac{\eta}{\sigma _j^0}g_i^0$	              $\sigma _j^0=\sqrt{(g_j^0)^2}=\lvert g_j^0\rvert$

$\theta _j^{2}\leftarrow \theta _j^1-\frac{\eta}{\sigma _j^1}g_i^1$                  $\sigma _j^1=\sqrt{\alpha(\sigma _j^0)+(1-\alpha)(g_j^1)^2}$

$\theta _j^{t+1}\leftarrow \theta _j^t-\frac{\eta}{\sigma _j^t}g_i^t$                $\sigma _j^t=\sqrt{\alpha(\sigma _j^{t-1})+(1-\alpha)(g_j^t)^2}$

> **Adam:** RMSProp + Momentum

# Learning Rate Sshceduling

> Learning Rate Decay

As the training goes, we are closer to the destination, so we reduce the learning rate

> Warm Up

Increase and Decrease(implement in resent)