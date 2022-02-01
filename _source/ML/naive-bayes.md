# 朴素贝叶斯

假设训练数据集由联合分布$P(X,Y)$产生。

<br>

给定$X=x$，预测值
$$\begin{aligned}
\hat{Y}&= \arg \underset{c_k}{\max} P(Y=c_k|X=x)\\\\
&=\arg \underset{c_k}{\max}  \frac{P(Y=c_k, X=x)}{P(X=x)}\\\\
&=\arg \underset{c_k}{\max}  P(Y=c_k,X=x)\\\\
&=\arg \underset{c_k}{\max}  P(X=x|Y=c_k)P(Y=c_k)\\\\
&=\arg \underset{c_k}{\max}  P(X^{(1)}=x^{(1)},\ldots,X^{(n)}=x^{(n)}|Y=c_k)P(Y=c_k)
\end{aligned}$$
由于$P(X^{(1)}=x^{(1)},\ldots,X^{(n)}=x^{(n)}|Y=c_k)$参数过多，故引入**条件独立性假设**：
$$P(X^{(1)}=x^{(1)},\ldots,X^{(n)}=x^{(n)}|Y=c_k)=\prod_iP(X^{(i)}=x^{(i)}|Y=c_k)$$
假设较强，故称”**朴素（Naive）**”。由此，
$$\hat{Y}= \arg \underset{c_k}{\max} P(Y=c_k)\prod_iP(X^{(i)}=x^{(i)}|Y=c_k)$$

<br>

故学习朴素贝叶斯模型，即学习$P(Y=c_k)$与$P(X^{(i)}=x^{(i)}|Y=c_k)$。采用极大似然估计：
$$\begin{aligned}
\hat{P}(Y=c_k)&=\frac{1}{N}\displaystyle\sum_i I\{y_i=c_k\}\\
\\
\hat{P}(X^{(j)}=a_{jl}|Y=c_k)&=\frac{\displaystyle\sum_i I\{x_i^{(j)}=a_{jl},\ \ y_i=c_k\}}{\displaystyle\sum_i I\{y_i=c_k\}}
\end{aligned}$$
其中$a_{jl}$为$X$第$j$个分量的第$l$个可能取值，$l=1,2,\ldots,S_j$。

<br>

由于训练数据可能存在$\displaystyle\sum_i I\{x_i^{(j)}=a_{jl},\ \ y_i=c_k\}=0$的情况，致使$\hat{P}(X^{(j)}=a_{jl}|Y=c_k)=0$，进而使$\displaystyle\prod_i\hat{P}(X^{(i)}=x^{(i)}|Y=c_k)=\hat{P}(X^{(1)}=x^{(1)},\ldots,X^{(n)}=x^{(n)}|Y=c_k)=0$，不符合逻辑，故引入**拉普拉斯平滑（Laplacian smoothing）**：
$$\hat{P}_\lambda(X^{(j)}=a_{jl}|Y=c_k)=\frac{\displaystyle\sum_i I\{x_i^{(j)}=a_{jl},\ \ y_i=c_k\}+\lambda}{\displaystyle\sum_i I\{y_i=c_k\}+\lambda S_j}$$
使
$$\hat{P}_\lambda(X^{(j)}=a_{jl}|Y=c_k)>0$$
且保证
$$\sum_{l=1}^{S_j} \hat{P}_\lambda(X^{(j)}=a_{jl}|Y=c_k)=1$$
一般取$\lambda=1$。

<br>

综上，学习算法为：

1. 给定训练集，对于$y$的每个取值，$X$的每个分量的每个取值，计算
	$$\begin{aligned}
	\hat{P}(Y=c_k)&=\frac{1}{N}\displaystyle\sum_i I\{y_i=c_k\}\\
	\\
	\hat{P}_\lambda(X^{(j)}=a_{jl}|Y=c_k)&=\frac{\displaystyle\sum_i I\{x_i^{(j)}=a_{jl},\ \ y_i=c_k\}+\lambda}{\displaystyle\sum_i I\{y_i=c_k\}+\lambda S_j}\\\\
	k=1,\ldots,K\ ; \quad j&=1,\ldots,n\ ;\quad l=1,\ldots,S_j
	\end{aligned}$$
2. 要预测实例$x$，对于$y$的每个可能取值，计算
	$$P(Y=c_k)\prod_iP(X^{(i)}=x^{(i)}|Y=c_k)$$
3. 取使以上概率最大的$y$的取值，即
	$$\hat{Y}= \arg \underset{c_k}{\max} P(Y=c_k)\prod_iP(X^{(i)}=x^{(i)}|Y=c_k)$$
	作为预测值。

