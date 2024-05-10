---
title: KGEmbedding
katex: true
date: 2024-04-12 08:58:32
excerpt: 知识图谱嵌入方法汇总和实现代码OpenKE的一些解释
tag: GNN
---
知识图谱嵌入有较多方法，下面主要介绍转移距离模型TransX，语义匹配模型RESCAL等，考虑附加信息的模型。开源实现可以看[OpenKE](https://github.com/thunlp/OpenKE)和[Pykg2vec](https://pykg2vec.readthedocs.io/en/latest/intro.html)
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

# 语义匹配模型
相比TransX模型，语义匹配模型更注重挖掘向量化后实体和关系的潜在语义，采用基于相似性(向量内积，而不是TransX中的向量加减)的打分函数，通过匹配实体和关系在嵌入向量空间中的潜在语义衡量三元组事实成立的可能性。该方向的模型主要是RESCAL以及它的延伸模型。解读可以看这个系列的[文章](https://www.cnblogs.com/fengwenying/tag/%E5%8F%8C%E7%BA%BF%E6%80%A7%E6%A8%A1%E5%9E%8B/)
> 基于相似性的得分函数慢慢发展为基于神经网络的模型
## RESCAL
[RESCAL](https://icml.cc/2011/papers/438_icmlpaper.pdf)是基于张量分解的模型，定义的$\chi\in R^{n\times n\times m}$张量如下
![tensor modl](tensor_model.png)
每种关系对应一个图的邻接矩阵
对$\chi$进行张量分解
$$
\chi_k \approx AR_kA^T,for k=1,...m
$$
得到的$A\in R^{n\times r}$是因子矩阵，每行代表一个实体嵌入向量$h\in R^r$（注意这里对所有的$R_k$用的是同一个$A$），$R_k^{r\times r}$是核心张量$R^{r\times r\times m}$中的一个切片，是关系$k$的嵌入表示矩阵，它是不对称矩阵，可以表示非对称关系。根据核心张量和因子矩阵还原的结果被看做对应三元组成立的概率，如果概率大于某个阈值则对应的三元组正确，对应的打分函数可以表示为
$$
f_k(h,t)=h^TR_kt
$$
> 训练模型类似TransX中的方法，只是打分函数替换为上面的$f_k$，而没有用下面的损失函数，只从代码看也体现不出原文表示的张量分解的意思在..

损失函数包括了正则化项
$$
\mathcal L=f(A,R_k)+g(A,R_k)\\
=\frac{1}{2}(\sum_k||\chi_k-AR_kA_T||_F^2)+\frac{1}{2}\lambda(||A||^2_F+\sum_k||R_k||_F^2)\\
=\frac{1}{2}(\sum_k||\chi_k-AR_kA_T||_F^2+\lambda_1||A||_F^2+\lambda_2\sum_k||R_k||_F^2)
$$

对每一种关系$R_k$分别进行分解都得到相同的实体表示向量$A$，这样的分解机制可用利用有相同关系的实体的信息建模，文章中称为`collective learning`，类似推荐系统中的协同过滤
![collective learning](collective.png)
对关系是$party$的关系表示矩阵$R_{party}$，建模`Bill`就能用到`John`和`Lyndon`的向量来表示
$$
\chi_{party} \approx AR_{party}A^T=h_{Bill}R_{party}h_{Party X}+h_{John}R_{party}h_{Party X}+h_{Lyndon}R_{party}h_{Party X}+...
$$

## DistMult
[DistMult](https://arxiv.org/pdf/1412.6575.pdf)通过限制RESCAL中的关系矩阵为对角阵$diag(k)$简化模型
> 解读文章中提到的个人感受很受用:（看 related work 有感：第一点是，一直不知道 related work 该写什么、怎么写。我写论文一个很大的问题是一开始的引入总是从很大很宏观的角度，本想由浅入深引到自己的工作上，但是由于一开始没有聚焦，并且一写开就刹不住车，所以会有一种顾左右而言他、文不对题的感觉，小论文、开题报告里都有这个问题。这篇的 related work 就是由浅入深，先列出 multi-relational learning 的一些方法，然后详细介绍了 NTN，然后引到自己的规则抽取工作（虽然中间少了点衔接）。以后我在写 related work 也要再聚焦一些，多说与自己工作有密切关联的相关工作；第二点是 DistMult 这篇文章，模型本身的改进几乎没有，很鸡肋，但是它把展示的重点放在了规则挖掘上，这就是扬长避短的作用了，就像衣服的穿搭，身材不好也没有关系，关键是如何凸显优势、弱化劣势。）

实体嵌入向量表示为实体表示矩阵和`one-hot`向量的乘积（查表）之后的转换向量，转换函数$f$可以是线性或非线性函数。关系嵌入矩阵是对角矩阵$M_r$，得分函数$g_r$与RESCAL一致，采用双线性函数
$$
y_e=f(Wx_e)\\
g_r(y_{e1},M_r,y_{e2})=y_{e1}^TM_ry_{e2}
$$
> 训练模型类似TransX中的方法，只是打分函数替换为上面的$f_k$。因为$M_r$是对角阵，实际代码实现的时候是直接把关系嵌入表示为向量而不是矩阵

## LFM
[LFM](https://papers.nips.cc/paper_files/paper/2012/file/0a1bf96b7165e962e90cb14648c9462d-Paper.pdf)想要解决的问题是
- 大量关系类型只是所有关系中的一小部分（长尾现象）
- 数据集质量差且数据少

LFM根据三元组成立的概率来进行打分，获得打分函数。如果三元组$(h,R,t)$成立，即是
$$
P(h,R,t)=1
$$
其中$h,t$是实体嵌入向量，$R$是关系嵌入矩阵
文章建模概率为下面的函数
$$
P(h,R,t)=\sigma(f(h,R,t))\\
\sigma(x)=\frac{1}{1+e^{-x}}
$$
文章主要重新定义了打分函数$f$，引入参数$y,y',z,z'\in R^p$
$$
f(h,R,t)=y^TRy'+h^TRz+z'^TRt+h^TRt
$$
为了解决长尾问题引起的过拟合（关系数量多的时候，某些关系下的样本数量少），文章提出将关系矩阵分解为$d$个[秩一矩阵](https://blog.csdn.net/weixin_52812620/article/details/122587293)，这样可以使得不同关系共享参数，对于$n$种关系，任一关系$j$的嵌入矩阵
$$
R_j=\sum_{i=1}^{d}\alpha^j_r\Theta_r,\Theta_r=u_rv_r^T,u_r,v_r\in R^p
$$
这样关系$j$只有参数$\alpha^j\in R^d$控制秩一矩阵的组成来构造嵌入表示，只要$d\ll n$就能保证关系矩阵不会过拟合，同时由于关系矩阵都是同一套秩一矩阵构造，可以做到参数共享，也能降低过拟合风险

模型训练是最大化似然概率，由于LFM建模概率是
$$
P(h,R,t)=\sigma(f(x))
$$
和[Logistic](https://yangoosen.github.io/2024/04/02/LogisticRegression/)模型类似，最后得到的目标函数也大同小异

## NTN
[Neural Tensor Networks](https://proceedings.neurips.cc/paper/2013/file/b337e84de8752b27eda3a12363109e80-Paper.pdf)提出了用于知识库补全的神经网络框架，打分函数中同时包含双线性函数和线性函数，用词向量的平均作为实体表示，打分函数是
$$
g(h,R,t)=u_R^Ttanh(h^TW_R^{[1:k]}t+V_R\begin{bmatrix}h\\t\end{bmatrix}+b_R)
$$
> $W_R$是张量，切片对应某个关系，$u_R,W_R,V_R,b_R$都是针对关系$R$的参数，$u_R$在[实现](https://pykg2vec.readthedocs.io/en/latest/intro.html)中是关系的嵌入向量

> 将打分函数中的双线性部分$h^TW_R^{[1:k]}t$去掉是SLM(Single Layer Model)模型，LFM是纯双线性函数，这里的SML是纯线性模型，因此NTN即是SLM和LFM的联合

## SME
[Semantic Matching Energy Function](https://arxiv.org/pdf/1301.3485.pdf)的得分函数形式是
$$
\varepsilon(h,r,t)=g_{left}(h,r)^Tg_{right}(r,t)
$$
> $h,r,t\in R^d$
根据$g$函数的区别有两个版本的SME
- 线性形式
$$
g(e_1,e_2)=W_1e_1^T+W_2e_2^T+b^T
$$
> $W_1,W_2\in R^{p\times d},b\in R^p$
- 双线性形式
$$
g(e_1,e_2)=(W_{\times 3}e_1^T)e_2^T+b^T
$$
> $W_1,W_2\in R^{p\times d\times d},b\in R^p$,$(W_{\times 3}e_1^T)$是沿着第三个维度的相乘,可以理解为以$e_1$的权重给$d$个$W\in R^{p\times d}$加权相加。[代码实现](https://www.cnblogs.com/fengwenying/p/15049736.html#%E4%BB%A3%E7%A0%81-2)中实现的不是上面的形式(可能是为了减少参数量?)而是
$$
g(e_1,e_2)=(W_1e_1^T)\cdot(W_2e_2^T)+b^T
$$

## TATEC
[Two And Three-way Embeddings Combination](https://link.springer.com/chapter/10.1007/978-3-662-44848-9_28)混合二元交互和三元交互，分别进行预训练并进行联合微调，并且没有参数共享。头尾实体和关系都对应两个嵌入向量，打分函数是
$$
s(h,r,t)=s_1(h_1,r,t_1)+s_2(h_2,r,t_2)\\
s_1(h_1,r,t_1)=r_1^Th_1+r_2^Tt_1+h_1^TDt_1\\
s_2(h_2,r,t_2)=h_2^TRt_2
$$
> $D$是与三元组无关的对角阵，$R$是与关系对应的矩阵

使用负采样训练模型，先分别训练二元和三元交互，用训练得到的权重初始化整个模型，之后用`SGD`微调模型

## HolE
[Holographic Embeddings](https://ojs.aaai.org/index.php/AAAI/article/view/10314)提出双线性模型的统一形式是
$$
Pr(h,r,t)=\sigma(r^T(h\circ t))
$$
其中各种方法的组合操作$\circ$不同，产生了不同的模型。本文提出的操作是循环关联`circular correlation`
$$
h\circ t= h\star t\\\
[h\star t]_k=\sum_{i=0}^{d-1}h_it_{(k+i)}
$$
循环关联操作可以被视为张量积（`hardamard`积）的压缩，可以保证有较少计算量的同时有更多的交互
![circular correlation](star.png)

## ComplEx

[Complex Embedding](https://arxiv.org/pdf/1606.06357.pdf)主要思想是引入复值向量，取点积的实部部分作为得分。三元组成立的概率是
$$
Pr(h,r,t)=\sigma(\phi(h,r,t))
$$
> $h,r,t\in C^k$，都是$k$维复向量

对于某个关系$l$而言，可以用邻接矩阵表示，在复空间中做特征分解
$$
X_l=EW_lE^H
$$
由于$X_l$是只有实部的对称阵，因此$X^H=X$，因此$X^HX=XX^H$，是正规矩阵。$E$是特征向量矩阵，只取特征值的前$k$个值，此时$W_l=diag(r),r\in C^k$,可以利用特征值分解压缩原矩阵，规定的得分函数是
$$
\phi(h,R_l,t)=Re(h^TW_l\bar{t})
$$
> 关于正规矩阵的内容看《矩阵分析与应用》2.3。嵌入向量是$E\in C^{n\times k}$的某一行，关系$l$的嵌入矩阵是$W\in C^{k\times k}$

推广到多类型关系，每个关系都有嵌入向量$r\in C^k$，得分函数是
$$
\phi(h,r,t)=Re(\sum_{k=1}^kh_kr_k \bar{t_k})
$$
复向量的内积$a^Hb\not= b^Ha$不具有交换性，因为实部相等而虚部不相等，实部对称建模对称关系，虚部不相等建模非对称关系（不是很理解?）

## NAM
[Neural Association Models](https://arxiv.org/pdf/1603.07704.pdf)是神经网络模型，将事件$E_1$输入，用$sigmoid$或$softmax$计算得到$E_2$的概率，损失函数是最大化似然函数。对知识图谱嵌入表示而言，$E_1=(h,r),E_2=t$，有两种计算结构：
![DNN](dnn.png) ![RMNN](rmnn.png)

## ANALOGY
[Analogical Inference](https://arxiv.org/pdf/1705.02426.pdf)将关系矩阵约束为正规矩阵$A^TA=AA^TT$，基于双线性类模型的体系，头实体经过关系映射后近似于尾实体，打分函数也是双线性函数
$$
h^TW\approx t^T\\
\phi(h,r,t)=h^TWt
$$
为了使得关系满足类比推理的情况，如下图
![analogical inference](ana.png)
> 理解类比推理 man is to king as woman is to queen.

建模得到的嵌入向量和关系矩阵应该满足应该满足$a\rightarrow d$的两条转换路径等价
$$
r\circ r'=r'\circ r
$$
因此ANALOGY建模时添加约束$W_rW_{r'}=W_{r'}W_r$
利用打分函数的得分计算三元组成立概率，模型训练为有约束条件的损失函数最小化
$$
l(\phi(h,r,t),y)=-log\sigma(y\phi(h,r,t))
$$
原文还提到了ANALOGY在某些情况下可以转化为其他模型的得分函数，因此覆盖范围更广，表达力更好。[ANALOGY实现](https://github.com/thunlp/OpenKE/blob/OpenKE-PyTorch/openke/module/model/Analogy.py)中似乎之和[ComplEx实现](https://github.com/thunlp/OpenKE/blob/OpenKE-PyTorch/openke/module/model/ComplEx.py)差最后一项

## CP
[Canonical Tensor Decomposition](https://arxiv.org/pdf/1806.07297.pdf)主要是对老方法的探究，新房法是核 p-范数 正则化
$$
l_{i,j,k}(X)=-X_{i,j,k}+log(\sum_{k'}exp(X_{i,j,k'}))\\
-X_{k,j+P,i}+log(\sum_{i'}exp(X_{k,j+P,i'}))
$$
> 没看懂(也没细看)这篇文章，$X$应该是像RESCAL中的图张量一样，这里是直接用$X_{i,j,k}$指代了分解得到的$h\otimes r\otimes t$吗?，第三项中的$P$和$k',i'$具体是什么没有弄懂，如果要细读文章优先解决这些问题


## SimplE 
[Simple Embedding](https://arxiv.org/pdf/1802.04868.pdf)对1927年的CP(Canonical Polyadic)做简化，CP为每个实体分配两个嵌入向量，即作为头尾实体的时候是不同的向量表示，两个向量分别学习。SimplE仍然保持为每个实体$e$分配两个向量$h_e,t_e\in R^d$，为每个关系$r$也分配两个向量$v_r,v_{r^{-1}}\in R^d$。对每个三元组$(e_i,r,e_j)$定义相似度函数（得分函数）
$$
\phi(e_i,r,e_j)=\frac{1}{2}(<h_{e_i},v_r,t_{e_j}>+<h_{e_j},v_{v^{r-1}},t_{e_i}>)
$$
> $<a,b,c>$根据[实现](https://github.com/thunlp/OpenKE/blob/OpenKE-PyTorch/openke/module/model/SimplE.py)来看似乎就是向量对应位置相乘之后相加

训练的时候用上面的得分函数，预测的时候值用前半部分（文章中叫SimplE-ignr），相当于三元组$(e_i,r,e_j)$定得到的嵌入向量还是$h_{e_i},v_r,t_{e_j}$，训练时采用softplux，没有选择margin-based 的 loss 是因为它比 log-likelihood 更容易导致过拟合（文章原话，有参考文献）
$$
loss=softplux(-l\phi(e_i,r,e_j))+\lambda ||\theta||^2_2
softplux(x)=log(1+exp(x))
$$
[实现](https://github.com/thunlp/OpenKE/blob/OpenKE-PyTorch/openke/module/model/SimplE.py)中没有明显有正则项的存在，$l\in \{1,-1\}$是label，实际就是softplux(-正得分)+softplux(负得分)


## CrossE
[Interaction Embeddings](https://arxiv.org/pdf/1903.04750.pdf)引入矩阵计算实体和关系的`crossover interaction`（即关系影响实体，实体影响关系），计算头实体和关系的组合表示与尾实体的相似性
![overview](overview.png)
打分函数是
$$
f(h,r,t)=\sigma(tanh(c_r\circ h+ c_r \circ h \circ r +b)t^T)
$$
> $\circ$是`hadamard`积，$c_r$是从`Interaction matrix` C中用r的`one-hot`得到的嵌入向量，实际实现就是有三个embedding层，实体，关系和交互矩阵的embedding


> 博主这里的感悟：在这篇文章中学到/想到的一点是，如果方法/idea不是那么高端的话，可以通过多做实验另辟蹊径来弥补，如本文如果只做了 crossover interaction 的工作的话，就会显得单薄和鸡肋，但是因为加上了 explanation 这样一个工作重点，就会显得比较详实。
## RotatE
[Rotation Embedding](https://arxiv.org/pdf/1902.10197.pdf)希望建模三种类型的关系：关系是对称的，某两个关系是相反的，某三个关系是可传递的
> 关系的性质可以看离散数学

RotatE是复空间中的双线性模型，希望三元组满足$t=h\circ r$
> $t_i=h_ir_i,h_i,r_i,t_i\in C, |r_i|=1$


距离函数是
$$
d(h,r,t)=||h\circ r-t||
$$
> 原文附录提到了为什么建模的RotatE可以表示三种类型的关系

训练类似TransX的方式，但对负样本进行了赋权$p(h',r,t')$（原文叫`self-adversarial`），[实现](https://github.com/thunlp/OpenKE/blob/OpenKE-PyTorch/openke/module/loss/SigmoidLoss.py)中是由adv_temperature代表的
$$
loss=-log\sigma(\gamma-d(h,r,t))-\sum_{i=1}^n p(h',r,t')log\sigma(d(h',r,t')-\gamma)
$$
## TuckER
[Tensor Factorization for entities relations](https://arxiv.org/pdf/1901.09590.pdf)可以看[这里](https://blog.csdn.net/qq_42397330/article/details/116290128)
