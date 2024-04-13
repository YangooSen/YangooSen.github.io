---
title: KGEmbedding
katex: true
date: 2024-04-12 08:58:32
excerpt: 知识图谱嵌入方法
tag: GNN
---
知识图谱嵌入有较多方法，下面主要介绍转移距离模型TransX，语义匹配模型RESCAL等，考虑附加信息的模型。开源实现可以看[OpenKE](https://github.com/thunlp/OpenKE)
# TransX
TransX是转移距离模型，这些方法的重点是设计得分函数$f(h,r,t)$（正确三元组接近0得分高，错误三元组是大负数得分低），再利用下面的损失函数迭代嵌入向量
$$
\mathcal L=\sum_{(\mathcal{(h,r,t)}\in S\quad(h',r,t')\in S'_{(h,l,t)}}+[\gamma+f(h',r,t')-f(h,r,t)]_+ 
$$
> 其中$\gamma$是间隔距离超参数，$S,S'$分别是正确三元组和替换头尾实体的错误三元组。$f(h,r,t)$是各个方法中的得分函数，$[x]_+=x\ if\ x>0\ else\ 0$是正值函数

> 这里的损失函数与文章中的不同是因为得分函数前加了负号,$a-b=(-b)-(-a)$

Trans的意义是利用关系把头实体转移到尾实体，讲解可以看[知识表示学习Trans系列梳理(论文+代码)](https://zhuanlan.zhihu.com/p/144412694)

## TransE
[Translating Embeddings](https://papers.nips.cc/paper_files/paper/2013/file/1cecc7a77928ca8133fa24680a88d2f9-Paper.pdf)的基本思想是头实体在嵌入空间中经过关系向量的偏移可以得到尾实体
$$
h+r\approx t
$$
![TransE](TransE.png)
得分函数是
$$
f(h,r,t)=-||h+r-t||_2^2\\
$$

## TransH
[Translalting on Hyperplanes](https://ojs.aaai.org/index.php/AAAI/article/view/8870)是为了针对TransE中多对多实体嵌入同质化而提出。基本思想是对于每一种关系$r$都在对应的超平面($w_r$是平面的法向量)上建模实体
$$
h_{\perp}+d_r\approx t_{\perp}
$$
> TransE的形象理解：假如有N对1的关系组，甲乙丙丁等头实体都有对尾实体戊的关系“喜欢”，当尾实体和关系相同时，不同的头实体因为$h=t-r$会有同质的嵌入表示

![TransH](TransH.png)
得分函数是超平面上差距
$$
f=-||(h-w_r^Thw_r)+d_r-(t-w_r^Ttw_r)||_2^2
$$
> $h_\perp=h-w_r^Thw_r$如何得到的推导可以看[投影矩阵](https://blog.csdn.net/weixin_52812620/article/details/122587293)，$h$投影到超平面法向量上是$xw=\frac{w^Th}{w^Tw}w$，由于$||w_r||^2_2=w^Tw=1$，因此垂直部分$h_\perp$是原向量减去投影的向量$w^Thw$

## TransR
[Relation Embeddings](https://ojs.aaai.org/index.php/AAAI/article/view/9491)在TransE和TransH的基础上提出，可以将实体和关系映射到不同的语义空间。在原来实体空间中相似的实体可能很接近，但对不同关系而言，它可能对应实体的不同属性，映射到语义空间后的头尾实体将不再接近，**表达了不同关系对相似实体的属性选择**，提高了模型的表达能力。对于每一种关系$r$存在对应映射矩阵$W_r$将实体映射到关系$r$所在的空间
$$
hM_r+r=tM_r
$$
![TransR](TransR.png)
对应的得分函数是
$$
f=-||hM_r+r-tM_r||_2^2
$$

## TransD
[Embedding via Dynamic Mapping Matrix](https://aclanthology.org/P15-1067.pdf)在TransR的基础上提出，为了解决TransR的问题：
- 同一关系$r$下头尾所在的语义空间可能不同，比如（美国，总统，奥巴马），美国是一个实体，代表国家，奥巴马是一个实体，代表的是人物。TransR在类似（甲，喜欢，乙）这样的头尾实体在同一空间中的三元组表现才合理
- 关系投影矩阵$M_r$只通过关系$r$得到不合理，因为具体关系涉及到头尾实体，应该考虑头尾实体的内容
- TransR参数太多，计算复杂度高

TransD将投影矩阵改进为$M_rh$和$M_rt$，每个对象（包括实体和关系）都对应两个向量：$h$或$t$或$r$用来表示语义信息，$h_p$或$t_p$或$r_p$用来表示投影向量($h,t,h_p,t_p\in \R^m,r,r_p\in\R^n$)。
$$
M_{rh}=r_ph_p^T+I^{m\times n}\\
M_{rt}=r_pt_p^T+I^{m\times n}\\
h_\perp=M_{rh}h\\
t_\perp=M_{th}t\\
h_{\perp}+r\approx t_{\perp}
$$
其中$I$是初始化矩阵
![TransD](TransD.png)
对应的得分函数是
$$
f=-||h_\perp+r-t_\perp||_2^2
$$
## TransA
[Adaptive Approach for Knowledge Graph Embedding](https://arxiv.org/pdf/1509.05490.pdf)为了解决以上模型的两个问题
- 得分函数太简单不具备灵活性
- 实体和关系向量的每一个维度都同等对待，不同维度的重要性应该不一样，有些维度可能是噪音
> 不同对待维度有点像attention?

TransA对嵌入向量的不同维度加权，调整了得分函数，在训练的时候对参数进行了L2范数惩罚
$$
f=(|h+r-t|)^TW_r(|h+r-t|)
$$
> 得分函数中包含取绝对值操作，原文对此进行了解释
![TransA](TransA.png)

## TransG
[Generative Model for Knowledge Graph Embedding](https://aclanthology.org/P16-1219.pdf)解决的问题与TransR类似，一种关系可能存在多种语义，TransG用生成模型产生嵌入向量，得分函数和也做了修改
![TransG](TransG.png)
![Loss](loss.png)

## TranSparse
[Adaptive Sparse Transfer Matrix](https://ojs.aaai.org/index.php/AAAI/article/view/10089)解决知识图谱嵌入中的两个问题：
- 异质性：某些关系链接了非常多的实体对是复杂关系，而有些关系链接的非常少，是简单关系，比如"包含"关系可能有非常多的实体链接情况，而"相等"关系链接的实体就很少 
- 不平衡性：某些关系链接的头尾实体的种类和数量差别大。比如"性别"关系，头节点可能包含大量人名，而尾节点只有"男"女"两种节点

TranSparse认为复杂的关系应该有更多的参数建模，而简单关系用更少的参数学习。TranSparse用稀疏矩阵代替TransR的稠密矩阵。稀疏矩阵稀疏度$\theta_r$可以针对任务需求：

- 针对异质性问题提出TranSparse(share)：
$$
\theta_r=1-\frac{(1-\theta_{min})N_r}{N_{r^*}}\\
h_p=M_r(\theta_r)h\\\
t_p=M_r(\theta_r)t
$$
> 每种关系对应一个稀疏矩阵，$N_r$是关系链接的实体对数量，$N_{r^*}$表示链接最多实体对的关系链接的实体对数量，$\theta_{min}$是超参数，取值在$[0,1]$
- 针对不平衡性提出TranSparse(separate)：
$$
\theta_r^l=1-\frac{(1-\theta_{min})N_r^l}{N_{r^*}^{l^*}}(l=h,t)\\
h_p=M_r^h(\theta_r^h)h\\\
t_p=M_r^t(\theta_r^t)t
$$
> 每种关系对应两个稀疏矩阵分别映射头尾节点，稀疏度也根据两个端点处实体的数量调整

损失函数和TrnasR一致
$$
f=-||h_p+r-t_p||_2^2
$$

