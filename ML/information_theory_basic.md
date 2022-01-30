# 信息论基础

## 1 自信息

**自信息**（Self Infomation）表示一个**随机事件**包含的信息量。对于一个随机变量$X$，在$X=x$时的自信息表示为$I(x)$。

  

根据需求，自信息应当满足以下三条性质：

  

- $I(x)$随$p(X=x)$增大而减小，因为一个概率越小的事件发生，其中包含的信息量越大。

- 对于两个独立事件，观测到他们同时发生带来的信息量应当等于分别观测到他们各自发生的信息量之和，即

 $$p(X=x,Y=y)=p(X=x)p(Y=y)\ \ \Leftrightarrow \ \ I(x,y)=I(x)+I(y)$$

- 自信息的值大于0。

  

为满足以上性质，将自信息定义为：

$$I(x)=-\log p(x)$$

$\log$的底数可以任意选择，若为$2$时，单位为**bit**；为$e$时，单位为**nat**。

  

## 2 熵

一个随机变量的**熵**（Entropy）定义为其自信息的数学期望，用$H(X)$表示，即

$$H(X)=E_X[I(x)]=-\sum_{x \in \mathcal{X}}p(x)\log p(x)$$

其中，当$p(x_i)=0$时，定义$0\log 0=0$。

  

熵由$X$的分布确定，故也可记作$H(p)$。

  

熵刻画的是随机变量的**信息量**/**不确定性**/**无序程度**：

  

- 变量为常数时，分布本身含有所有信息，而随机变量没有不确定性，事件的发生也不带来额外信息，熵为$0$。

- 如果随机变量服从均匀分布，分布不含有任何信息，随机变量无序程度最大，随机事件的发生可以带来很多信息，故熵达到最大。

  

由数据估计得到的熵称为**经验熵**（Empirical Entropy）。

  

## 3 联合熵与条件熵

**联合熵**（Joint Entropy）与单个随机变量的熵类似，定义为

$$H(X,Y)=-\sum_{x \in \mathcal{X}}\sum_{y \in \mathcal{Y}}p(x,y)\log p(x,y)$$

  
  

**条件熵**（Conditional Entropy）$H(X|Y)$表示随机变量$Y$条件下随机变量$X$的不确定性，定义为

$$\begin{aligned}

H(X|Y)&=-\sum_{x \in \mathcal{X}}\sum_{y \in \mathcal{Y}}p(x,y)\log p(x|y)   \\

&=-\sum_{x \in \mathcal{X}}\sum_{y \in \mathcal{Y}}p(x,y)\log \frac{p(x,y)}{p(y)}\\

&=-\sum_{x \in \mathcal{X}}\sum_{y \in \mathcal{Y}}p(x,y)\log p(x,y)+\sum_{y \in \mathcal{Y}}p(y)\log p(y)\\

&=H(X,Y)-H(Y)

\end{aligned}$$

可知，当$X$与$Y$无关时，$H(X|Y)=H(X)$。

  

由数据估计得到的条件熵称为**经验条件熵**（Empirical Conditional Entropy）。

  

## 4 互信息与信息增益

### 4.1 互信息

随机变量$X$与$Y$的**互信息**（Mutual Information）衡量得知$X$的信息后使得$Y$的不确定性减少的程度，记为$I(X;Y)$，即
$$\begin{aligned}
I(X;Y)&= H(Y)-H(Y|X)     \\
&=-\sum_{y \in \mathcal{Y}}p(y)\log p(y)+\sum_{x \in \mathcal{X}}\sum_{y \in \mathcal{Y}}p(x,y)\log \frac{p(x,y)}{p(x)}\\
&=\sum_{x \in \mathcal{X}}\sum_{y \in \mathcal{Y}}p(x,y)\log \frac{p(x,y)}{p(x)}-\sum_{x \in \mathcal{X}}\sum_{y \in \mathcal{Y}}p(x,y)\log p(y)\\
&=\sum_{x \in \mathcal{X}}\sum_{y \in \mathcal{Y}}p(x,y)\log \frac{p(x,y)}{p(x)p(y)}
\end{aligned}$$

可知

  

- $X$与$Y$无关时，其互信息为$0$。

- $I(X;Y)=I(Y;X)$

  

### 4.2 信息增益

特别地，在模型训练中，数据特征$A$与训练数据集$D$的互信息称为**信息增益**（Information Gain），即数据集$D$的经验熵$H(D)$与在给定特征$A$的条件下的$D$的经验条件熵$H(D|A)$之差，记为$g(D,A)$，即

$$g(D,A)=\hat{H}(D)-\hat{H}(D|A)$$

表示由于特征$A$，使得对数据集$D$的分类的不确定性减少的程度，信息增益大的特征具有更好的分类能力。

  

**信息增益的算法：**

  

> - 数据集$D$的样本容量记为$N$
> - 数据集被分为$K$类，其中每一类的集合记为$D_k=\{(x_i,y_i)|y_i=c_k\}$，每一类的容量记为$N_k$
> - 特征$A$有$P$个取值，其中每一类的集合记为$D^p=\{(x_i,y_i)|x^A_i=a_p\}$，每一类的容量记为$N^p$
> - 记$C_k$类样本点中变量$A=a_p$的样本点集合为$D_k^p=D_K\cap D^p$，每一类的容量记为$N_k^p$

  

1. 计算数据集经验熵$\hat{H}$

 $$\hat{H}(D)=-\sum_k \frac{N_k}{N}\log \frac{N_k}{N}$$

2. 计算条件经验熵$\hat{H}$

$$\begin{aligned}

 \hat{H}(D|A)&=-\sum_k \sum_p \frac{N_k^p}{N} \log \frac{N_k^p/N}{N^p/N}      \\

 &=-\sum_k \sum_p \frac{N_k^p}{N} \log \frac{N_k^p}{N^p}

 \end{aligned}$$

3. 计算信息增益

 $$g(D,A)=\hat{H}(D)-\hat{H}(D|A)$$

  

### 4.3 信息增益比

一般地，变量$A$可能的取值越多，其信息增益越大，故引入**信息增益比**（Information Gain Ratio）来矫正这一影响。信息增益比$g_R(D,A)$定义为$A$对数据集$D$的信息增益与数据集$D$关于变量$A$的经验熵之比，即

$$g_R(A,D)=\frac{g(D,A)}{\hat{H}_A(D)}$$

其中

$$H_A(X)=-\sum_{p}p(X^A=a_p)\log p(X^A=a_p)$$

其经验熵为

$$\hat{H}_A(D)=-\sum_{p}\frac{N^p}{N}\log \frac{N^p}{N}$$
