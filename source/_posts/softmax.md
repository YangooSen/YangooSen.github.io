---
title: softmax
katex: true
date: 2024-04-02 20:54:19
excerpt: softmax中最大化似然函数与最小化交叉熵的等价性，将sigmoid推广到softmax
tag: note
---

$softmax$回归是一个多分类模型
$$
f(X)=softmax(X_{n\times d}W_{d\times k}+b_{1\times k})
$$
它的输出是每个类别的概率，样本集的标签以独热编码$one-hot\ encoding$给出，这个模型认为给定输出特征$x_{1\times d}$，类别是$j$的概率是$softmax(xW+b)$的第$j$项，即
$$
P(y_j|x)=softmax(xW+b)_j=\hat{y}_j
$$
其中$y_j$是标签的独热编码向量，它的第$j$项不为$0$，其他项都是$0$，而$\hat{y}_j$是一个数值，是输出向量中的第$j$个分量。下面说明的是对这种分类任务而言，**最大化对数似然函数等价于最小化[交叉熵](https://zhuanlan.zhihu.com/p/149186719)损失**

> 尽管$softmax$是一个非线性函数
> $$
> softmax(o_i)=\frac{exp(o_i)}{\sum_k exp(o_k)}
> $$
> 但它不改变输出$o$的相对大小，分类结果$argmax_j\ \hat{y_j}=argmax_j\ \hat{o_j}$不会改变，因此$softmax$回归的输出仍然由输入特征的仿射变换决定，因此$softmax$回归是一个线性分类模型

假设模型给出的分类概率向量是$\hat{y}^{(i)}$，似然函数是
$$
P(Y|X)=\prod_{i=1}^nP(y^{(i)}_j|x^{(i)})
$$
**其中$x^{(i)}$是第$i$个样本向量，其中$y^{(i)}_j$是第$i$个标签的独热编码且第$j$项不为$0$，其他项都是$0$**（注意这里$y^{(i)}_j$是一个向量，而不是分量数值，它表示的是真实数据的标签），而最大化似然函数相当于最小化负对数似然函数
$$
-logP(Y|X)=\sum_{i=1}^n-logP(y^{(i)}_j|x^{(i)})=\sum_{i=1}^n-log\hat{y}_j^{(i)}
$$
而真实标签向量和预测标签向量的交叉熵
$$
H(y^{(i)},\hat{y}^{(i)})=-\sum_{j=1}^ky_j^{(i)}log\hat{y}_j^{(i)}
$$
由于独热编码中只有一项是$1$，因此
$$
H(y^{(i)},\hat{y}^{(i)})=-log\hat{y}_j^{(i)}
$$
其中$\hat{y}_j^{(i)}$是真实标签$y$中不为零的那一项对应的预测概率，是一个数值。因此得到$max\ P(Y|X)\iff\min\sum_{i=1}^nH(y,\hat{y})$
下面是损失函数$-\sum_{j=1}^ky_jlog\hat{y}_j$代入模型的过程
$$
l(y,\hat{y})=-\sum_{j=1}^ky_jlog\frac{exp(o_j)}{\sum_{i=1}^kexp(o_k)}\\
=\sum_{j=1}^ky_jlog\sum_{i=1}^kexp(o_k)-\sum_{i=1}^ko_jy_j\\
=log\sum_{i=1}^kexp(o_k)-\sum_{i=1}^ko_jy_j
$$
最后一个等式也是利用了$y$中只有一项是$1$，下面求偏导数
$$
\partial_{o_j}l(y,\hat{y})=\frac{exp(o_j)}{\sum_k exp(o_k)}-y_j=softmax(o)_j-y_j
$$
因此偏导就是输出中的每一个分量与真实独热标签直接的差异，这很类似回归问题中的绝对损失函数，其中梯度是观测值$y$和估计值$\hat{y}$之间的差异。[这不是巧合，在任何指数族分布模型中，对数似然的梯度正是由此得出的](https://d2l.ai/chapter_appendix-mathematics-for-deep-learning/distributions.html)，这使得梯度的计算在实践中变得容易很多
[这里](https://blog.csdn.net/weixin_52812620/article/details/128480083?spm=1001.2014.3001.5502)是$logistic$回归模型，$logistic$回归模型实际上是$softmax$回归损失函数的特殊情况，下面给出公式解释
下面的表述中忽略转置符号和偏置项(只需在w和x中多加一项即可表达)，在softmax回归中
$$
P(y=j|x)=softmax(wx)=\frac{e^{w_jx}}{\sum_{k=1}^{K}e^{w_kx}}
$$
当$K=2,j=0/1$时，
$$
P(y=1|x)=softmax(wx)=\frac{e^{w_1x}}{e^{w_0x}+e^{w_1x}}
$$
而在logistic回归中
$$
p(y=1|x)=sigmoid(wx)=\frac{1}{1+e^{-wx}}=\frac{e^{wx}}{1+e^{wx}}
$$
将logistic中的$w$理解为$w_1-w_0$（这里就是推广扩展的点），即可得到与softmax一致的表达式
$$
p(y=1|x)=\frac{e^{(w_1-w_0)x}}{1+e^{(w_1-w_0)x}}=\frac{e^{w_1x}}{e^{w_0x}+e^{w_1x}}
$$
关于理解w，下面做出解释：在sigmoid(wx)中，分类为0/1的临界点是wx=0，按照softmax中的思路,$w_1$表达了
类别1，而$w_0$表达了类别0，$w_1x$即表达了x为与类别1的相似度，那么分类临界点即应该是$w_1x$=$w_0x$，即$w$理解为$w_1-w_0$ 


