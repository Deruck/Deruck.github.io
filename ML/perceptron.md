# 感知机
## 1 模型
函数
$$f(x)=sign(w\cdot x+b)$$
即为感知机。

其中分割超平面$w \cdot x+b=0$将样本点分为两类：

- $w \cdot x_0+b<0$时，$f(x_0)=-1$
- $w \cdot x_0+b>0$时，$f(x_0)=1$

模型的参数有：

- $w$：权值（weight）
- $b$：偏置（bias）


## 2 学习
学习感知机，即学习$w$，$b$。

首先可以作为损失函数的是错分类样本点的个数，但由于不是关于参数的连续可导函数，不易优化。可以使用错分类点到分割超平面的距离和作为损失函数，即：
$$L(w,b)=\frac{1}{||w||}\sum_{(x_i,y_i) \in M}|w \cdot x_i+b|$$

注意到对于每个被错误分类的点$(x_i,y_i)$，必有：
$$y_i(w \cdot x_i+b)<0$$
故可转化为最优化问题：
$$\min_{w,b} L(w,b)=-\sum_{(x_i,y_i)\in M} y_i(w\cdot x_i+b)$$


使用随机梯度下降算法进行优化，梯度为：
$$\nabla L(w,b)=\left[ \begin{matrix}
   \frac{\partial L(w,b)}{\partial w}  \\
   \frac{\partial L(w,b)}{\partial b}  \\
\end{matrix} \right]=\left[ \begin{matrix}
   -\sum\limits_{(x_i,y_i) \in M}y_i x_i  \\
   -\sum\limits_{(x_i,y_i) \in M}y_i  \\
\end{matrix} \right]$$

### 2.1 原始形式
选择初始参数值$w_0$，$b_0$，指定步长/学习率$\eta$，不断迭代。

每次迭代中，满足
$$y_i(w \cdot x_i+b) \le 0$$
的点即为被错误分类的点。

随机选择一个错误分类点$(x_i,y_i)$，对参数进行更新：
$$\left[ \begin{matrix}
   w  \\
   b  \\
\end{matrix} \right]\leftarrow \left[ \begin{matrix}
   w+\eta\ y_ix_i  \\
   b+\eta\ y_i  \\
\end{matrix} \right]$$
直到满足迭代条件，如迭代次数达到上限、无错误分类点等。

**实现代码：**
```python
class Perceptron:
    def fit(self,X,y,iter=500,l_rate=0.01):
        '''
        模型训练
        :param iter: 最大迭代次数，默认500
		:param l_rate：学习率，默认0.01
        :return: w,b
        '''
		
        self.b=0                                            # b初始化为0
        self.w=np.zeros(X.shape[1])                         # 权值w初始化为0
        
        for i in range(iter):                               # 限制迭代次数
            M=np.where(y*(X.dot(self.w)+self.b)<=0)[0]      # 错误分类点索引
            if M.size==0: break                             # 若无错误分类点，提前结束
            r=np.random.choice(M)                           # 随机抽取错分类数据
            
            self.w+=l_rate*y[r]*X[r]
            self.b+=l_rate*y[r]                             # 更新参数
        
        return self.w,self.b  
```

### 2.2 对偶形式
注意到$w$与$b$实际上只与每个样本点被错分类次数有关，即在$w_0=0$，$b=0$时，有：
$$w=\sum_{(x_i,y_i) \in M}n_i(\eta y_ix_i)=\eta \sum_{(x_i,y_i) \in M}n_iy_ix_i$$
$$b=\sum_{(x_i,y_i) \in M}n_i(\eta y_i)=\eta \sum_{(x_i,y_i) \in M}n_iy_i$$
所以只需学习$n$即可。

设定初始值$n_0=(0,0,\ldots,0)^T$，设定步长$\eta$，不断迭代。每次迭代中，满足
$$\begin{aligned}
y_j(w \cdot x_j+b)&=y_j\bigg(\eta \sum_{(x_i,y_i) \in M}n_iy_i(x_i\cdot x_j)+\eta \sum_{(x_i,y_i) \in M}n_iy_i\bigg)\\
&=\eta y_j\bigg(\sum_{(x_i,y_i) \in M}n_iy_i(x_i\cdot x_j)+\sum_{(x_i,y_i) \in M}n_iy_i\bigg)\\
&\leq0
\end{aligned}$$
的点即为被错误分类点，由此可见$\eta$在此处并不必要。

随机选择被错误分类的点$(x_i,y_i)$，更新参数：
$$n_i \leftarrow n_i+1$$
直到满足迭代条件。

对偶形式与原始形式等价，但是对偶性是训练过程中训练数据只以内积$x_i\cdot x_j$的形式出现，故可以先计算内积矩阵即Gram矩阵：
$$G=\Big[ x_i\cdot x_j \Big]_{n\times n}=X^TX$$
在自变量维数较高时可以显著降低计算量。

**实现代码：**
```python
class Perceptron:
    def fit(self,X,y,iter=500):
        '''
        模型训练
        :param iter: 最大迭代次数，默认500
        :return: w,b
        '''
        self.Gram=X.dot(X.T)                            # Gram矩阵
        self.n=np.zeros(X.shape[0])                     # 每个样本点被错分类次数

        for i in range(iter):
			                                            # 错误分类索引
            M=np.where(y*((self.n*y*Gram).sum(axis=1)+(self.n*y).sum())<=0)[0] 
            if M.size==0: break
            r=np.random.choice(M)                       # 随机抽取错分类数据
            self.n[r]+=1                                # 更新参数
			
        return (self.n*y*X.T).sum(axis=1),(self.n*y).sum() # 根据n计算w和b
```
在程序实现中，自变量一般由转置形式表示，即每一行为一个样本点。

