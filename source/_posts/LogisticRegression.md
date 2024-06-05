---
title: LogisticRegression
katex: true
date: 2024-04-02 20:21:14
excerpt: 逻辑回归串讲，涉及到softmax和logistic的统一，贝叶斯分类，玻尔兹曼机等
tag: MLBasis
---
**[博客园](https://www.cnblogs.com/pinard/)相关内容值得一看**。逻辑回归是一种输出标签是$0/1$的分类模型，将
$$
sigmoid(x)=\frac{1}{1+e^{-x}}=\frac{e^x}{1+e^x}
$$
套在线性回归
$$
f(x)=w^Tx+b
$$
上得到，即
$$
y=\frac{1}{1+e^{-(w^Tx+b)}}
$$
若模型为二分类$y=1/0$模型，输出的$y$就是$y=1$的概率，即
$$
p(y=1|x)=\frac{1}{1+e^{-(w^Tx+b)}}=\frac{e^{w^Tx+b}}{1+e^{w^Tx+b}}
$$
那么
$$
p(y=0|x)=1-p(y=1|x)=\frac{1}{1+e^{w^Tx+b}}
$$
以最大似然估计计算$w,b$，为了表述简单，设
$$
p(y=1|x)=\pi(x)\\
p(y=0|x)=1-\pi(x)
$$
共有$n$个样本点且标签为$y={0,1}$，那么似然函数
$$l'(w,b)=\prod_{i=1}^{n}[\pi(x_i)]^{y_i}[1-\pi(x_i)]^{1-y_i}
$$
（**这里是关键，因为这里的$y_i$和$1-y_i$都是1，相当于没加，但化简之后有抵消的作用**），取对数得到
$$
l(w,b)=\sum_{i=1}^{n}[y_ilog\pi(x_i)+(1-y_i)log(1-\pi(x_i))]
$$
合并同类项
$$
l(w,b)=\sum_{i=1}^{n}[y_ilog\frac{\pi(x_i)}{1-\pi(x_i)}+log(1-\pi(x_i))]
$$
把$\pi(x)$和$1-\pi(x)$的定义代入得到
$$
l(w,b)=\sum_{i=1}^{n}[y_i(w^Tx_i+b)-log(1+e^{w^Tx_i+b})]
$$
最大化此似然函数就是最小化
$$
l(w,b)=\sum_{i=1}^{n}[-y_i(w^Tx_i+b)+log(1+e^{w^Tx_i+b})]
$$
此函数为连续可导的凸函数，多种优化方法都可求得其最优解，设
$$
\beta=(w;b),x=(x;1)
$$
那么负对数似然函数的梯度是
$$\frac{\partial l(\beta)}{\partial \beta}=-\sum_{i=1}^{n}x_i(y_i-\frac{e^{\beta^Tx_i}}{1+e^{\beta^Tx_i}})
$$
梯度下降法，牛顿法都可解


> 设$X_1,\dots,X_n$是来自$f(x|\theta_1,\dots,\theta_k)$为概率密度函数的$iid$样本，似然函数的定义是$L(\theta|x)=L(\theta_1,\dots,\theta_k|x_1,\dots,x_n)=\prod_{i=1}^nf(x_i|\theta_1,\dots,\theta_k)$，即给定观测点$x$，参数$\theta$的似然函数是在参数确实是$\theta$的时候，确实观测到$x$的概率，最大化似然函数就是最大化观测到的实际情况的概率

机器学习实战中直接最大化对数似然函数，而不是最小化负对数似然函数，最大化方法用的是梯度上升，关键是计算了梯度
$$
\frac{\partial l(\beta)}{\partial \beta}=-\sum_{i=1}^{n}x_i(y_i-\frac{e^{\beta^Tx_i}}{1+e^{\beta^Tx_i}})
$$

```python
    for k in range(maxCycles):              #heavy on matrix operations
        h = sigmoid(dataMatrix*weights)     #matrix mult
        error = (labelMat - h)              #vector subtraction
        weights = weights + alpha * dataMatrix.transpose()* error #matrix mult
```

这里$h$得到的就是
$$
\frac{e^{\beta^Tx_i}}{1+e^{\beta^Tx_i}}
$$
$error$就是
$$
(y_i-\frac{e^{\beta^Tx_i}}{1+e^{\beta^Tx_i}})
$$

$dataMatrix.transpose()* error$就是

$$
x_i(y_i-\frac{e^{\beta^Tx_i}}{1+e^{\beta^Tx_i}})
$$

权重$\beta=\beta+\alpha\ grad\ l(\beta)$，优化权重$maxCycles$轮后返回。一次仅用一个样本点更新系数称为随机梯度上升，由于可以在新样本到来时对分类器进行增量式更新，因而随机梯度上升是一个在线学习算法，与之对应的，梯度上升一次处理所有数据被称作“批处理”

```python
        h = sigmoid(sum(dataMatrix[i]*weights))
        error = classLabels[i] - h
        weights = weights + alpha * error * dataMatrix[i]
```

改进随机梯度上升可以使步长 $\alpha$ 随迭代次数减小，防止越过最优点，还可以在随机梯度上升时随机选取样本，减少周期性的波动

```python
        for i in range(m):
            alpha = 4/(1.0+j+i)+0.0001    #apha decreases with iteration, does not 
            randIndex = int(random.uniform(0,len(dataIndex)))#go to 0 because of the constant
            h = sigmoid(sum(dataMatrix[randIndex]*weights))
            error = classLabels[randIndex] - h
            weights = weights + alpha * error * dataMatrix[randIndex]
            del(dataIndex[randIndex])
```
感知机
$$
f(x)=sign(w^Tx+b)
$$
也是一种线性分类模型，假设标签是$+1/-1$，数据中误分类点到超平面的[距离](https://blog.csdn.net/weixin_52812620/article/details/128519939?spm=1001.2014.3001.5502)之和是
$$
\sum_{x_i\in M}-\frac{1}{||w||}y_i(w^Tx_i+b)
$$
不考虑$\frac{1}{||w||}$，就得到了损失函数
$$
L=\sum_{x_i\in M}-y_i(w^Tx_i+b)
$$
求偏导得到
$$
\partial_wL=\sum_{x_i\in M}-y_ix_i\\
\partial_bL=\sum_{x_i\in M}-y_i
$$
优化算法用批量大小为$1$的批量随机梯度下降，[这里](https://blog.csdn.net/weixin_52812620/article/details/122792380)是代码实现，它不能拟合$XOR$函数而使得第一次$AI$寒冬

**[从逻辑回归到受限玻尔兹曼机](https://blog.csdn.net/xixiaoyaoww/article/details/105683422)，[softmax回归中最大化似然和最小化交叉熵的等价性，sigmoid推广到softmax](https://yangoosen.github.io/2024/04/02/softmax/)**
