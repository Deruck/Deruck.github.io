# 奇异值分解

## 1 定义
任意$m \times n$实矩阵$A\in\mathbb R^{m \times n}$，均可分解为三个实矩阵的乘积
$$A=U\Sigma V^T$$
其中

- $U$为$m$阶正交矩阵，$V$为$n$阶正交矩阵，即
	$$\begin{aligned}
	U^TU&=I_m      \\\\
	V^TV&=I_n
	\end{aligned}$$
- $\Sigma$为$m \times n$对角矩阵，对角元素降序排列，即
	$$\begin{aligned}
	\Sigma&=\mathrm{diag}(\sigma_1,\sigma_2,\ldots,\sigma_p)\\\\
	\sigma_1&\geq\sigma_2\geq\cdots\geq\sigma_p\\\\
	p&=\min(m,n)
	\end{aligned}$$
- $\Sigma$唯一，$U$与$V$不唯一

![](https://xcdn.loli.top/gh/Deruck/Img/Img/20210617191150.png)

以上分解方式为完全奇异值分解（full svd），而更常用的是奇异值分解的截断形式（truncated）和紧凑形式（compact）。

在奇异值分解中，只取最大的$k$个奇异值所对应的部分即为**截断奇异值分解**，即
$$A\approx U_k\Sigma_kV_k^T$$
其中

- $U_K$、$V_k$分别为$U$、$V^T$的前$k$列、行
- $\Sigma_k$为由$\Sigma$的前$k$个对角元素组成的$k$阶对角矩阵

![](https://xcdn.loli.top/gh/Deruck/Img/Img/20210617191949.png)


特别地，$k$与矩阵$A$的秩相等时，为**紧奇异值分解**。

![](https://xcdn.loli.top/gh/Deruck/Img/Img/20210617193434.png)
<br><br>

## 2 理解

### 对线性变换的分解

对角矩阵可以视为一个沿坐标轴拉伸的线性变换，正交矩阵可以视为一个旋转的线性变换，故$m \times n$实矩阵$A$的奇异值分解
$$A=U\Sigma V^T$$
可以视为将从$\mathbb{R}^n$到$\mathbb{R}^m$的线性变换$A$分解为先进行旋转变换$V^T$，然后进行坐标轴缩放变换$\Sigma$，最后进行旋转变换$U$。
<br>

### 对矩阵的近似

由矩阵的外积展开式
$$A=U\Sigma V^T=\sum\sigma_iu_iv_i^T=\sum A_i$$
$$\begin{aligned}
U&=\Big[u_1, \ldots, u_k\Big]     \\\\
V&=\Big[v_1, \ldots, v_k\Big]
\end{aligned}$$
可以看出，矩阵$A$可看作$U$与$V^T$对应行、列向量$u_i$与$v_i^T$的乘积的加权和，权重为$\sigma_i$。由于奇异值通常衰减很快，保留前$k$个奇异值，即舍弃权重较小的$u_iv_i^T$，是对矩阵$A$很好的压缩与近似。

事实上，奇异值分解是在平方损失意义下对矩阵的最优近似。
<br>

### 与特征值分解的关系
$$\begin{aligned}
AA^T&=U\Sigma V^TV\Sigma^TU^T=U\Sigma^T\Sigma U^T     \\\\
A^TA&=V\Sigma^TU^TU\Sigma V^T=V\Sigma \Sigma^TV^T
\end{aligned}$$
可以看出

- $U$的列向量是$AA^T$的特征向量，$V$的列向量是$A^TA$的特征向量。
- 奇异值的平方为对应特征值，即
	$$\sigma_i=\sqrt{\lambda_i}$$


<br><br>

## 3 性质

### 性质一：左右奇异向量的关系

不妨设$m\geq n$
$$AV=U\Sigma$$

$$A[v_1,\cdots,v_n]=[u_1,\cdots,u_n,\cdots,u_m]	
	\left[ \begin{matrix}
\begin{matrix}
\sigma _1 & {} & {}  \\
{} & \ddots  & {}  \\
{} & {} & \sigma_n  \\
-&-&-\\
\end{matrix}  \\
0  \\
\end{matrix} \right]$$

$$[Av_1,\cdots,Av_n]=[\sigma_1u_1,\cdots,\sigma_nu_n]$$
可得
$$Av_i=\sigma_iu_i,\quad i=1,\cdots,n$$
类似地，
$$A^TU=V\Sigma^T$$
$$A^T[u_1,\cdots,u_n,\cdots,u_m]=[v_1,\cdots,v_n]
	\left[ \begin{matrix}
\begin{matrix}
\sigma_1 & {} & {} & |  \\
{} & \ddots  & {} & |  \\
{} & {} & \sigma_n & |  \\
\end{matrix} & 0  \\
\end{matrix} \right]$$
$$[A^Tu_1,\cdots,A^Tu_n,\cdots,A^Tu_m]=[\sigma_1v_1,\cdots,\sigma_nv_n,0,\cdots,0]$$
可得
$$A^Tu_i=
\begin{cases} 
\sigma_iv_i,&i=1,\cdots,n \\
0,&i=n+1,\cdots,m
\end{cases}$$

这条性质常用在奇异值分解的求解中。

### 性质二
$$v_i={\arg\max \atop {\displaystyle\big\{||x||=1\ |\ x\in \mathbb{R}^n,\ x\bot v_1,\cdots,v_{i-1}\big\}}}||Ax||$$
$$u_i={\arg\max \atop {\displaystyle\big\{||y||=1\ |\ y\in \mathbb{R}^m,\ y\bot u_1,\cdots,u_{i-1}\big\}}}||A^Ty||$$
<br>
$$\begin{aligned}
\sigma_i&=\max_{\displaystyle\big\{||x||=1\ |\ x\in \mathbb{R}^n,\ x\bot v_1,\cdots,v_{i-1}\big\}}||Ax|| \\
&=\max_{\displaystyle\big\{||y||=1\ |\ y\in \mathbb{R}^m,\ y\bot u_1,\cdots,u_{i-1}\big\}}||A^Ty||
\end{aligned}$$

**Proof**

...


## 4 计算

先求解较小的一侧，这里不妨设$m\ge n$。

1. 求解$A^TA$的特征值（$m\leq n$时先求$AA^T$）
	$$\lambda_1\ge\lambda_2\ge\cdots\ge\lambda_n\ge0$$
	>$A^TA$半正定，特征值非负。
1. 求解$\lambda_i$对应特征值的特征向量$v_i$，组成$n$阶正交矩阵$$V=[v_1,\cdots,v_n]$$
2. 根据特征值计算得
	$$\Sigma=	\left[ \begin{matrix}
		\begin{matrix}
		\sigma_1 & {} & {}  \\
		{} & \ddots  & {}  \\
		{} & {} & \sigma_n  \\
		-&-&-\\
	\end{matrix}  \\
		0  \\
	\end{matrix} \right]_{m\times n}$$
	其中$\sigma_i=\sqrt\lambda_i$。
3. 对前$r$个正奇异值，令
	$$u_i=\frac{1}{\sigma_i}Av_i,\quad i=1,\cdots,r$$
	得到$$U_1=[u_1,\cdots,u_r]$$
5. 求解$$A^Tu=0$$
	得到$m-r$个$A^T$零空间的标准正交基，构成
	$$U_2=[u_{r+1},\cdots,u_m]$$
6. 令
	$$U=[\ U_1\ \ U_2\ ]$$
	得到$A$的奇异值分解
	$$A=U\Sigma V^T$$
