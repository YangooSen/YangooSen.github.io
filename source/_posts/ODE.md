---
title: ODE
katex: true
date: 2024-09-26 08:56:04
excerpt: 常微分方程笔记
tag: math
---


预备知识 [多元函数微分法及其应用](https://blog.csdn.net/linxilinxilinxi/article/details/80867708?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~default-1.pc_relevant_paycolumn_v2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~default-1.pc_relevant_paycolumn_v2&utm_relevant_index=2)
配合视频 [川大数学常微分方程](https://www.bilibili.com/video/BV1bx411s7pb?p=1&spm_id_from=333.1007.top_right_bar_window_history.content.click)


# 一、绪论

## 1. 微分方程的基本概念

- 定义：$F(x,y,y^{'},y^{''},y^{'''},\cdots,y^{(n)})=0$
- 常微分方程（$ODE$）：自变量只有一个；偏微分方程（$PDE$）：自变量有多个
- 阶（order）：微分方程未知函数的最高阶导数或微分的阶数，**二阶及以上是高阶**
- 线性：$y^{(n)}+a_1(x)y^{(n-1)}+a_2(x)y^{(n-2)}+a_3(x)y^{(n-3)}+\cdots+a_n(x)y$，幂指数最高是$1$
- 方程的解：$y=\varphi(x),x\in [a,b]$，**含有$n$个相互独立的常数为通解**；若不含有常数为特解，确定特解需要初值条件
- 通解可能是显式的（$y=\varphi(x)$），也可能是隐性的（$f(y)=g(x)$）
- 对于方程$\phi(t)=f(t,\phi(t))$方程的解$x=\phi(t)$在$x-t$平面上给出的光滑曲线$\digamma$，称为该方程的一条积分曲线
- 每一个点的斜率都一样的曲线是等倾线，它由$f(t,x)=k$确定
- [雅克比行列式（Jacobian）](https://blog.csdn.net/linkequa/article/details/87311456)是向量对应的函数（就是多变量函数，多个变量可以理解为一个向量，因此多变量函数就是向量函数）的一阶偏微分以一定方式排列形成的矩阵

- **当微分方程难以求得精确解时，需要知道方程的近似解，近似解的求法是利用了函数序列的逼近**
- 方程在没有给出解的精确表达式时，通过几何上的分析可以获得解的很多信息，推断解的某些重要属性，从而使微分方程问题在一定程度上得到解决
- **阶数和维数互换思想**，降低方程的阶数会使得空间的维数提高，往往用维数换阶数更好，通过增加变量的方式以较高维数的代价使微分方程的阶数降低

# 二、初等积分法

## 1.分离变量法

- 变量分离方程：$y^{'}=g(x)h(y)$，其中$g(x)$和$h(y)$是定义在某一区间上的连续函数

  具体解法：**（能干净地把关于$x$的函数和关于$y$的函数分开）**

  - 若$y=y_0,s.t.h(y_0)=0$，则$y=y_0$是方程的解（若求$y(x_0)=i$的特解，而$i\ne0$，就不需要考虑此处是否有$h(y)=0$的情况）
  - 若$h(y)\ne0$，分离变量后两端积分：$\int\frac{dy}{h(y)}=\int g(x)dx$
  - 得到$H(y)=G(x)+c$，是方程的隐式通解，**确定区间**

### 1.1.一阶线性方程

- 形如$\frac{dy}{dx}=p(x)y+q(x)$的方程是一阶线性方程，其中，$p(x),q(x)$是定义在某区间上的连续函数
  - 当$q(x)=0$时，$\frac{dy}{y}=p(x){dx}$，$y=ce^{\int p(x)dx}$
  - 当$q(x)\ne0$时，设$y=c(x)e^{\int p(x)dx}$是方程的某个解（常数变易法，将齐次方程结果中的$c$变为$c(x)$），$c'(x)e^{\int p(x)dx}+c(x)p(x)e^{\int p(x)dx}=p(x)c(x)e^{\int p(x)dx}+q(x)$，两边同时消去$p(x)c(x)e^{\int p(x)dx}$，得到$c'(x)=q(x)e^{-\int p(x)dx} \Rightarrow c(x)=\int q(x)e^{-\int p(x)dx}dx+c$，代入原方程得到$y=e^{\int p(x)dx}(\int q(x)e^{-\int p(x)dx}dx+c)=\underline{ce^{\int p(x)dx}}+e^{\int p(x)dx}\int q(x)e^{-\int p(x)dx}$，其中，划线部分就是$q(x)=0$时的通解，而没有下划线的部分是$q(x)\ne0$的特解（类似于非齐次方程的通解是基础解系和特解的和）

### 1.2.伯努利方程

- 形如$\frac{dy}{dx}=p(x)y+q(x)y^n(n \ne 0,1)$是伯努利方程，它是一阶线性方程的变体，一定能化为一阶线性方程

  - $y^{-n}\frac{dy}{dx}=p(x)y^{1-n}+q(x)$，$dy^{1-n}=(1-n)y^{-n}dy$ $\Rightarrow$  $(1-n)y^{-n}\frac{dy}{dx}=(1-n)p(x)y^{1-n}+(1-n)q(x)$

  - 令$y^{1-n}=z$，$\frac{dz}{dx}=(1-n)p(x)z+(1-n)q(x)$，即为一阶线性方程

## 2.变量替换法

### 2.1.齐次方程

- 齐次方程：形如$y^{'}=g(\frac{y}{x})$，叫齐次方程
- **齐次方程一定可以化为变量分离方程：**
  - 令$\frac{x}{y}=u$，则$dy=udx+xdu$，同除$dx$，得到$\frac{dy}{dx}=u+x\frac{du}{dx}=g(u)$
  - 若$g(u)-u\ne0$，得到$\frac{du}{g(u)-u}=\frac{dx}{x}$
  - 两端积分得到隐式通解$G(u)=ln|x|+c$
  - 若$u=u_0,s.t.g(u)-u_0=0$，那么$y=u_0x$也是方程的解

### 2.2.能化为齐次方程的方程

- 形如$y^{'}=f(\frac{a_1x+b_1y+c_1}{a_2x+b_2y+c_2})$的方程一定能化为齐次方程
  - 若$c_1=c_2=0$，那么分子分母同除$x$，直接得到齐次方程
  - 如果$c_1^2+c_2^2\ne0$，$a_1x+b_1y+c_1=0$和$a_2x+b_2y+c_2=0$两直线的关系只可能是：重合，相交，垂直
    - 若重合，则$\frac{a_1}{a_2}=\frac{b_1}{b_2}=\frac{c_1}{c_2}=k$，那么$y'=kf(\frac{a_2x+b_2y+c_2}{a_2x+b_2y+c_2})=kf(1)$，此时$y=ax+b$
    - 若平行，则$\frac{a_1}{a_2}=\frac{b_1}{b_2}\ne\frac{c_1}{c_2}=k$，那么$y'=f(\frac{k(a_2x+b_2y)+c_1}{a_2x+b_2y+c_2})$，令$a_2x+b_2y=u$，那么$du=a_2dx+b_2dy$，同除$dx$，$a_2+b_2\frac{dy}{dx}=\frac{du}{dx}$，$\frac{(\frac{du}{dx}-a_2)}{b_2}=f(\frac{ku+c_1}{u+c_2})=f(u)\Rightarrow \frac{du}{dx}=b_2f(u)+a_2\Rightarrow \frac{du}{b_2f(u)+a_2}=dx$，两端积分即可
    - 若相交，则$\frac{a_1}{a_2}\ne\frac{b_1}{b_2}\ne\frac{c_1}{c_2}$，解方程$a_1x+b_1y+c_1=0$和$a_2x+b_2y+c_2=0$，求交点得到$(\alpha,\beta)$，将坐标轴原点移动到交点处（此时两条直线即可过原点），令$x-\alpha=X$，$y-\beta=Y$，那么$\frac{dY}{dX}=\frac{a_1X+b_1Y}{a_2X+b_2Y}=\frac{a_1+b_1\frac{Y}{X}}{a_2+b_2\frac{Y}{X}}$，令$\frac{Y}{X}=u$，那么$dY=udX+Xdu \Rightarrow \frac{dY}{dX}=u+X\frac{du}{dX} \Rightarrow u+X\frac{du}{dX}=f(u) \Rightarrow \frac{du}{f(u)-u}=\frac{X}{dX}$，两端积分即可

- 化为齐次方程并不一定做$u=\frac y x$的替换：
  - $\frac {dy} {dx}=f(ax+by+c)$，令$ax+by=u \Rightarrow du=adx+bdy \Rightarrow \frac{du}{adx}-1=\frac{b}{a}\frac{dy}{dx}=\frac{b}{a}f(u) \Rightarrow \frac{du}{\frac{b}{a}f(u)+1}=adx$，两端积分即可
  - $f(xy)ydx+g(xy)xdy=0$，令$xy=z$，$dz=xdy+ydx$，$f(z)\frac z x dx=g(z)x\frac{dz-ydx}{x} \Rightarrow \frac{f(z)zdx}{x}=g(z)(dz-\frac z xdx) \Rightarrow f(z)zdx=g(z)(xdz-zdx) \Rightarrow (f(z)+g(z))zdx=g(z)xdz \Rightarrow \frac{(f(z)+g(z))z}{g(z)dz}=\frac{x}{dx}$，两端积分即可

## 3.积分因子法

### 3.1.全微分方程

- $M(x,y)dx+N(x,y)dy=0$，若存在$du(x,y)=Mdx+Ndy$叫全微分方程或恰当方程，它的通解是$u(x,y)=c$
- $\frac{\partial M(x,y)}{\partial y}=\frac{\partial N(x,y)}{\partial x} \iff M(x,y)dx+N(x,y)dy=0是全微分方程$，它的解是$\int_{x_0}^x M(x,y)dx+\int_{y_0}^y N(x_0,y)dy=c$
- 对称的结构可以**凑微分**：只与$x$相关的函数和$dx$凑在一块，只与$y$相关的函数和$dy$凑在一块单独求，既与$x$有关又与$y$有关的部分在遇到$dx$时把$x$放$d$里去，遇到$dy$把$y$放到$d$里去，形成**分组**形式
- 根据$u'_x=M(式1),u_y'=N(式2)$构造$u$
  - 对式$1$积分：$u(x,y)=\int M(x,y)dx+ \varphi(y)$，两边对$y$偏导后与利用式$2$： $\frac{\partial}{\partial y}\int M(x,y)dx+ \varphi'(y)=N$
  - $\varphi'(y)=N-\underline{\frac{\partial}{\partial y}\int M(x,y)dx}$，划线部分仅与$y$有关：
    - 对$x$求偏导得到$\frac{\partial N}{\partial x}-\frac{\partial}{\partial x}\frac{\partial}{\partial y}\int M(x,y)dx=\frac{\partial N}{\partial x}-\frac{\partial}{\partial y}\frac{\partial}{\partial x}\int M(x,y)dx=\frac{\partial N}{\partial x}-\frac{\partial M}{\partial y}=0$
  - $\varphi(y)=\int[N-\frac{\partial}{\partial y}\int M(x,y)dx]dy \Rightarrow u(x,y)=\int M(x,y)dx+ \int[N-\frac{\partial}{\partial y}\int M(x,y)dx]dy$

### 3.2.能化为全微分方程的方程

- 定义：若存在$\mu(x,y)$，使得$\mu(x,y)M(x,y)dx+\mu(x,y)N(x,y)dy=0$成为全微分方程或$\frac{\partial [\mu(x,y)M(x,y)]}{\partial y}=\frac{\partial [\mu(x,y)N(x,y)]}{\partial x}$，$\mu(x,y)$称为积分因子，**若其存在，它有无数个**
  - 若$\mu(x,y)$是积分因子，$u(x,y)$是原函数，对任何的$\mu(x,y)h(u(x,y))$也是其积分因子，其中$h(z)$是任意可微分的非零函数
- 积分因子可以通过偏微分方程求出：$M\frac{\partial \mu}{\partial y}+\mu\frac{\partial M}{\partial y}=N\frac{\partial \mu}{\partial x}+\mu\frac{\partial N}{\partial x}$，$N\frac{\partial \mu}{\partial x}-M\frac{\partial \mu}{\partial y}=(\frac{\partial M}{\partial y}-\frac{\partial N}{\partial x})\mu=E(x,y)\mu$
  - 若$\mu(x,y)$只与$x$有关，$\mu(x,y)=\mu(x)$，$\frac{\partial \mu}{\partial y}=0$
    - $N\frac{d \mu}{d x}=\mu(\frac{\partial M}{\partial y}-\frac{\partial N}{\partial x}) \Rightarrow \frac{d\mu}{\mu}=\underline{\frac{\frac{\partial M}{\partial y}-\frac{\partial N}{\partial x}}{N}}dx$，若划线部分只与$x$有关，两端积分即可得到积分因子
  - 若$\mu(x,y)$只与$y$有关，$\mu(x,y)=\mu(y)$，$\frac{\partial \mu}{\partial x}=0$
    - $M\frac{d \mu}{d y}=\mu(\frac{\partial N}{\partial x}-\frac{\partial M}{\partial y}) \Rightarrow \frac{d\mu}{\mu}=\underline{\frac{\frac{\partial N}{\partial x}-\frac{\partial M}{\partial y}}{M}}dy$，若划线部分只与$y$有关，两端积分即可得到积分因子
- **$\frac{\partial M}{\partial y}-\frac{\partial N}{\partial x}=E(x,y)$称为恰当判别式，若$E(x,y)=0$则方程恰当，否则，尝试$\frac{E(x,y)}{N(x,y)}$或$\frac{E(x,y)}{M(x,y)}$判断积分因子是否只与$x$或$y$有关，若只与$x$或$y$有关，根据上面的画线部分求出积分因子后将原方程化为恰当方程继而求得解**
- 对于一阶线性方程$y'=p(x)y+q(x)$
  - $[p(x)y+q(x)]dx-dy=0$，$M(x,y)-N(x,y)=p(x)$，它存在只与$x$有关的积分因子

## 4.参数法

- 对于隐式方程即难以解除$y'$而无法用上述方法解时，使用参数化来解决
- 参数方程实际表达了几何的形体，类似在二维平面上的一个圆（一维曲线），用两个参数表达$x=rcos\theta,y=rsin\theta$似乎更合适
- 对于$F(x,y,y')=0$，有三个变量，但有一个等式，类似于三维空间中的曲面，选择两个参数$s,t$。求解的思想是**将$p=y'$看成独立的变量**，将代数方程$F(x,y,p)=0$所定义的曲面参数化
- 具体求解方法**（引入参数 $\to$ 根据微分关系重写方程$\to$ 解重的写方程$\to$ 反解原方程）**：
  - 令$x=\phi(s,t),y=\psi(s,t),p=\kappa(s,t)$
  - $dx=\frac{\partial \phi}{\partial s}ds+\frac{\partial \phi}{\partial t}dt,dy=\frac{\partial \psi}{\partial s}ds+\frac{\partial \psi}{\partial t}dt,dy=\frac{dy}{dx}dx=\kappa dx$
  - $\frac{\partial \psi}{\partial s}ds+\frac{\partial \psi}{\partial t}dt=\kappa (\frac{\partial \phi}{\partial s}ds+\frac{\partial \phi}{\partial t})dt \Rightarrow (\frac{\partial \psi}{\partial s}-\kappa \frac{\partial \phi}{\partial s})ds+(\frac{\partial \psi}{\partial t}-\frac{\partial \phi}{\partial t})dt=0$，得到$s,t$的微分方程，利用其他方法求解

- 如果$F(x,y,y')=0$中解出$y$的函数$y=f(x,y')$，令$y'=p$
  - $p=\frac{dy}{dx}=\frac{\partial y}{\partial x}+\frac{\partial y}{\partial p}\frac{dp}{dx} \Rightarrow (p-\frac{\partial y}{\partial x})dx-\frac{\partial y}{\partial p}{dp}=0$（注意这里$y=f(x,y')$可以理解为$y$是$x$的复合函数，求导符合[链式法则](https://blog.csdn.net/sunbobosun56801/article/details/79161232?ops_request_misc=&request_id=&biz_id=102&utm_term=%E5%A4%9A%E5%85%83%E5%87%BD%E6%95%B0%E9%93%BE%E5%BC%8F%E6%B3%95%E5%88%99&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-4-79161232.nonecase&spm=1018.2226.3001.4187)，$\Rightarrow$可以理解为对等式两边分别对$x$积分）
  - 上述已经化为$x,p$的微分方程，利用其他方法进行积分
- 如果$F(x,y,y')=0$中解出$x$的函数$x=f(y,y')$，令$y'=p$
  - 两边对$x$求导，$1=\frac{\partial x}{\partial y}\frac{d y}{d x}+\frac{\partial x}{\partial p}\frac{\partial p}{\partial x} = \frac{\partial x}{\partial y}\frac{d y}{d x}+\frac{\partial x}{\partial p}\frac{\partial p}{\partial y}\frac{d y}{d x}=\frac{\partial x}{\partial y}p+\frac{\partial x}{\partial p}\frac{\partial p}{\partial y}p \Rightarrow \frac{dp}{dy}=\frac{\frac{1}{p}-\frac{\partial x}{\partial y}}{\frac{\partial x}{\partial p}}$，求得$p=P(y)$的方程
  - 继续对$p=P(y)$积分，将参数方程形式的解转化回$x,y$的解
- 如果$F(x,y,y')=0$不显含$y$，$F(x,y')=0$，令$y'=p$
  - $F(x,p)=0$表达的是$xp$平面上的一条曲线，引入参数$t$，设该曲线有参数表示（这个参数表示是根据观察得到的具体函数）$x=\varphi(t),p=\psi(t)=\frac{dy}{dx} \Rightarrow dy=\psi(t)\varphi'(t)dt$
  - $y=\int \psi(t)\varphi(t)'dt+c,x=\varphi(t)$，得到参数解
- 如果$F(x,y,y')=0$不显含$x$，$F(y,y')=0$，令$y'=p$
  - $F(y,p)=0$表达的是$yp$平面上的一条曲线，引入参数$t$，设该曲线有参数表示**（这个参数表示是根据观察得到的具体函数）$y=\varphi(t),p=\psi(t)\Rightarrow dy=\varphi(t)'dt,dy=pdx=\psi(t)dx\Rightarrow dx=\frac{\varphi(t)'}{\psi(t)}dt$，得到$x$的参数方程
  - 将参数形式的解转化为$x,y$的通解

## 5.初等积分法的应用

### 5.1.奇解、包络

- 我们希望方程的通解可以包含全部，但事实上很多时候无法办到；初等方程的解由于出现$c$确定了某些曲线族$\{\gamma_c\}$，这些曲线族也很难包含所有解
- 设$\phi(x,y,c)=0$是给定平面单参数$c$的曲线族，$\phi(x,y,c)=0$作为三元函数连续可微。曲线族的$\digamma$是本身不包含在曲线族中，但过曲线$\digamma$的每一点，有曲线族中的一条曲线和$\digamma$在这点相切
- 在某些微分方程中，存在一条特殊的积分曲线，它并不属于这个方程的积分曲线族，但在这条特殊的积分曲线上的每一点处，都有积分曲线族中的一条曲线与其在此点相切。这条特殊的积分曲线所对应的解成为方程的奇解**(奇解的定义有很多种)**，也可以说，对于方程$F(x,y,y')=0$的某个解，在它所对应的积分曲线上的每点处都不满足存在唯一性定理的条件，即解的唯一性被破坏，(例如若某微分方程的通解为$y=(x+c)^2(c为任意常数)$，在$(0,0)$处会有$y=0$与$y=x^2$两个解，唯一性被破坏）
- [另一种奇解的定义及奇解和包络的关系](https://wenku.baidu.com/view/bd08256248d7c1c708a14574.html)
  - 奇解给出的了通解与特解的关系
  - 奇解给出了解不唯一的例子，即存在唯一性问题
- [根据包络线的确定方程](https://zhuanlan.zhihu.com/p/118474711)得到**可能**的包络线$\Omega(x,y)=0$（$C$-判别曲线），$\Omega(x,y)=0$的哪一分支是包络还需要进一步验证；另外还有$P$-判别曲线，[一阶微分方程奇解的求法总结和例题](https://wenku.baidu.com/view/10dd18ff0242a8956bece4f2.html)，[奇解的存在性定理](https://zhuanlan.zhihu.com/p/97414951)

### 5.2.高阶微分方程

- 高阶微分方程通过以维度换阶数的方法降阶求解：
  - 不显含$y$的方程$F(x,\frac{d^ky}{dx^k},\cdots,\frac{d^ny}{dx^n})=0(k\ge1)$，令$p=\frac{d^ky}{dx^k}$，原方程降为关于$p$的$n-k$阶微分方程$F(x,p,\frac{dp}{dx},\cdots,\frac{d^{n-k}p}{dx^{n-k}})=0$
  - 不显含$x$的方程$F(y,\frac{dy}{dx},\cdots,\frac{d^ny}{dx^n})=0$（自治微分方程），令$p=\frac{dy}{dx}$，$\frac{d^ny}{dx^n}=\frac{d^{n-1}p}{dx}=\frac{d^{n-1}p}{dy}\frac{dy}{dx}=\frac{d^{n-1}p}{dy}p$原方程降为关于$p$的$n-1$阶微分方程$F(y,p,\cdots,\frac{d^{n-1}p}{dy}p)=0$
  - $F(x,y,\frac{dy}{dx},\cdots,\frac{d^ny}{dx^n})=0$是关于$y,\frac{dy}{dx},\cdots,\frac{d^ny}{dx^n}$的[零次齐次方程](https://zhidao.baidu.com/question/403510122.html)，若$y\ne0$它等价于$F(x,1,\frac{1}{y}\frac{dy}{dx},\cdots,\frac{1}{y}\frac{d^ny}{dx^n})=0$（将$y$提出去），令$p=\frac{1}{y}\frac{dy}{dx}$，那么$\frac{dy}{dx}=yp,\frac{d^2y}{dx^2}=\frac{dy}{dx}p+\frac{dp}{dx}y=yp^2+y\frac{dp}{dx}$，方程降低一阶变为$G(x,p,\frac{dp}{dx},\cdots,\frac{d^{n-1}p}{dx^{n-1}})=0$
  - $F(x,y,\frac{dy}{dx},\cdots,\frac{d^ny}{dx^n})=0$其中左边是某个形如$\phi(x,y,\frac{dy}{dx},\cdots,\frac{d^{n-1}y}{dx^{n-1}})$的表达式对$x$的全导数（或乘以积分因子后）即$F(x,y,\frac{dy}{dx},\cdots,\frac{d^ny}{dx^n})=\frac{d}{dx}\phi(x,y,\frac{dy}{dx},\cdots,\frac{d^{n-1}y}{dx^{n-1}})=\frac{\partial\phi}{\partial\ x_1}+\frac{\partial\phi}{\partial\ x_2}\frac{dy}{dx}+\cdots+\frac{\partial\phi}{\partial\ x_{n+1}}\frac{d^{n}y}{dx^{n}}$，其中函数$\phi(x_1,x_2,\cdots,x_{n+1})$对各变元的一阶偏导数在$(x_1,x_2,\cdots,x_{n+1})=(x,y,\frac{dy}{dx},\cdots,\frac{d^{n-1}y}{dx^{n-1}})$处取值，此时解原方程等价于解$\phi(x,y,\frac{dy}{dx},\cdots,\frac{d^{n-1}y}{dx^{n-1}})=c$，以此降为$n-1$阶

### 5.3.Riccati方程

- 形如$\frac{dx}{dt}=a(t)x^2+b(t)x+c(t)$，其中$a(t),b(t),c(t)$在区间$I$上连续且$a(t)\ne0$的方程是$Riccati$方程

- $Riccati$方程的求解是通过寻找一个特解，利用特解将方程变形为一个伯努利方程

  - 如果寻找到一个特解$x=\phi(t)$，令$y=x-\phi(t)$，$\frac{dy}{dt}=\frac{dx}{dt}-\phi'(t)=a(t)x^2+b(t)x+c(t)-[a(t)\phi^2(t)+b(t)\phi(t)+c(t)]=a(t)[y+\phi(t)]^2+b(t)[y+\phi(t)]+c(t)-[a(t)\phi^2(t)+b(t)\phi(t)+c(t)]=a(t)y^2+2a(t)\phi(t)y+b(t)y$

  - 一种特殊情况，$\frac{dx}{dt}=-x^2+ct^m$，$c$是常数且$t\ne0,x\ne0$

    - 当$m=0$时，方程可以化为变量分离的形式
    - 当$m=-2$时，通过变换$y=tx$，方程也可以化为变量分离的形式
    - 当$m=\frac{-4k}{2k+1}$时，通过变换$t=\tau^{-\frac{1}{m+1}},x=(\frac{c}{m+1}\frac{1}{\tau-y\tau^2})$，方程化为$\frac{dy}{dt}=-y^2+c'\tau^n$，其中$c'=\frac{c}{(m+1)^2},n=\frac{-4(k-1)}{2(k-1)+1}$，重复$k$次，$n$的分母变位$1$，分子变为$0$，此时得到第一种情况
    - 当$m=\frac{-4k}{2k-1}$时，与情况三类似，变化略微不同

  - 总结：$m=0,-2,\frac{-4k}{2k\pm1}(k=1,2,\cdots)$是可以方程$\frac{dx}{dt}=-x^2+ct^m$使用初等积分法求解的充要条件，这表明不是所有的微分方程都能使用初等积分法求解

    - 仅通过方程来准确判断具体过程的性质，例如周期性，稳定性等
    - 对于方程寻找近似解来逼近也是一种选择



# 三、线性方程
- 线性方程不一定是一个方程，可能是一个方程组
- 非线性方程可以通过泰勒展开来近似线性方程，得到解后再返回原方程研究相关性质
- 线性方程的研究对象是$x'=A(t)x+f(t)$，其中$A(t)=\begin{bmatrix}a_{11}(t)&a_{12}(t)&...&a_{1n}(t)\\a_{21}(t)&a_{22}(t)&&\\...&&\ddots\\&\\a_{n1}(t)&...&...&a_{nn}(t)\end{bmatrix},f(t)=\begin{bmatrix}f_1(t)\\f_3(t)\\f_1(t)\\...\\f_n(t)\end{bmatrix}$

## 1.存在性与唯一性
- 判断解的唯一性和方向性可以帮助寻找近似解

- 假设系数矩阵$A(t)$是区间$[\alpha,\beta]$上的$n\times n$阶**连续**矩阵函数，$f(t)$是区间$[\alpha,\beta]$上的$n$维**连续**列向量函数，则对于区间$[\alpha,\beta]$上的任意实数$t_0$及任意$n$维常向量$x_0$，方程组$\frac{dx}{dt}=A(t)x+f(t)$在区间$[\alpha,\beta]$上存在**唯一解**$x(t)$满足初值条件$x(t_0)=x_0$

  - $x(t)$存在是有范围的，它与$A(t)$和$f(t)$存在的区间是一致的，都是$[\alpha,\beta]$

- 矩阵函数$A(t)$的性质满足：

  - $\frac{d}{dt}A(t)=(\frac{d}{dt}a_{ij}(t))$，即矩阵函数的导数是每个位置的元素求导，位置不变得到的导数矩阵函数
  - $\int^{\beta}_{\alpha}A(t)dt=(\int^{\beta}_{\alpha}a_{ij}(t))$，即矩阵函数的积分是每个位置的元素积分，位置不变得到的积分矩阵函数
  - $\frac{d}{dt}A(t)B(t)=\frac{dA}{dt}B(t)+\frac{dB}{dt}A(t)$
  - $\int^{\beta}_{\alpha}kA(t)dt=k\int^{\beta}_{\alpha}A(t)dt$

- 关于范数有：

  - $||A||=\sum_{i=1,j=1}^{n}|a_{ij}|$
  - $||A+B||\le||A||+||B||$
  - $||rA||=|r|||A||$
  - $||AB||\le||A||||B||$
  - $||\int^{\beta}_{\alpha}A(t)dt||\le\int^{\beta}_{\alpha}||A(t)||dt$，需要$\beta>\alpha$保证非负。此不等式类似于积分估值不等式$m(b-a)\le\int^{b}_{a}f(x)\le M(b-a)$

- 矩阵函数序列$\{A_k(t)\}_{k=1}^{\infty}$收敛，有等价的[坐标收敛（点点收敛）和范数收敛](https://blog.csdn.net/kodoshinichi/article/details/109612420)，函数序列的收敛还有条件更强的[**一致收敛**](https://blog.csdn.net/qq_45011547/article/details/90641858?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164269493616780264032051%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=164269493616780264032051&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-90641858.first_rank_v2_pc_rank_v29&utm_term=%E4%B8%80%E8%87%B4%E6%94%B6%E6%95%9B&spm=1018.2226.3001.4187)，一般的收敛定义中，$N$既与$t$有关，也与$\varepsilon$有关，一致收敛中$N$只与$\varepsilon$有关，而与$t$无关

- 证明**存在唯一性定理的目标**是证明满足$\begin{cases} \begin{aligned} x'=A(t)x+f(t)\\ x(t_0)=x_0 \quad\end{aligned} \end{cases}$的解是存在的且是惟一的，它主要使用了$Picard$逼近法，它主要包括：

  - **化为等价积分方程的连续解**：$x(t)$是初值问题$\begin{cases} \begin{aligned} x'=A(t)x+f(t)\\ x(t_0)=x_0 \quad\end{aligned} \end{cases}$定义于$[\alpha,\beta]$的解 $\iff$ $x(t)$是积分方程$x(t)=x_0+\int_{t_0}^{t}(A(\tau)x(\tau)+f(\tau))d\tau$在区间$[\alpha,\beta]$上的解
    - $\Rightarrow$证明：

      - 设$x(t)$是初值问题的解，则$x'=A(t)x+f(t),x(t_0)=x_0$
      - $\int^{t}_{t_0}x'dt=x-x_0=\int_{t_0}^{t}(A(\tau)x(\tau)+f(\tau))d\tau \Rightarrow x=x_0+\int_{t_0}^{t}(A(\tau)x(\tau)+f(\tau))d\tau$
      - 因为$x(t)$是初值问题的解，那么它在区间$[\alpha,\beta]$上可导，那么它连续
      - $x(t)$连续，且满足$x(t)=x_0+\int_{t_0}^{t}(A(\tau)x(\tau)+f(\tau))d\tau$，$Q.E.D.$

    - $\Leftarrow$证明：

      - 设$x(t)$是积分方程的解，则$x(t)=x_0+\int_{t_0}^{t}(A(\tau)x(\tau)+f(\tau))d\tau$，且$\varphi(t)$连续，那么$A(\tau)x(\tau)+f(\tau)$连续，那么可导$A(\tau)x(\tau)+f(\tau)$
      - $x'=A(t)x+f(t),x(t_0)=x_0$，$Q.E.D.$
  - **构造向量函数序列$\{x_k(t)\}$**：递推函数序列$\{x_k(t)\}$在$[\alpha,\beta]$上有定义，连续，其中$x_k(t)=\begin{cases} \begin{aligned}x_0 \quad\quad\quad\quad\quad k=0 \\ x_0+\int_{t_0}^{t}(A(\tau)x_{k-1}(\tau)+f(\tau))d\tau\quad k>0\end{aligned} \end{cases}$
  - **证明向量函数序列$\{x_k(t)\}$的一致收敛性**
    - $x_k(t)=x_0(t)+\sum_{j=1}^{k}[x_j(t)-x_{j-1}(t)]$，如$x_3(t)=x_0(t)+x_3(t)-x_2(t)+x_2(t)-x_1(t)+x_1(t)-x_0(t)$
    - $A(t),f(t)$都在闭区间上的连续，因此必有界，即$\exist M,||A(t)||\le M,||f(t)||\le M$
    - $||x_1(t)-x_0(t)||=||\int_{t_0}^{t}(A(\tau)x_{0}(\tau)+f(\tau))d\tau||\le\int_{t_0}^{t}(M||x_0(\tau)||+M)d\tau=M(||x_0||+1)|t-t_0|$（不等式的成立使用上面范数的第五个性质）
    - $||x_2(t)-x_1(t)||=||\int_{t_0}^{t}A(\tau)(x_{1}(\tau)-x_{0}(\tau))d\tau||\le\int_{t_0}^{t}[M^2(||x_0||+1)|\tau-t_0|]d\tau=(||x_0||+1)\frac{M^2}{2}|t-t_0|^2$（不等式的成立使用了上述$||x_1(t)-x_0(t)||$的结果）
    - $\cdots$，数学归纳法证明下述结果
    - $||x_j(t)-x_{j-1}(t)||\le(||x_0||+1)\frac{M^j}{j!}|t-t_0|^j\le(||x_0||+1)\frac{M^j}{j!}(\beta-\alpha)^j=(||x_0||+1)\frac{[M(\beta-\alpha)]^j}{j!}$
    - 根据[比值法](https://blog.csdn.net/qq_23940575/article/details/84582275?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164270048516780271984404%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=164270048516780271984404&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-2-84582275.first_rank_v2_pc_rank_v29&utm_term=%E6%AF%94%E5%80%BC%E6%B3%95%E5%88%A4%E6%96%AD%E6%94%B6%E6%95%9B&spm=1018.2226.3001.4187)可以判断$j\to\infty$，$\sum_{j=1}^{\infty}\frac{[M(\beta-\alpha)]^j}{j!}$收敛，此时根据[优级数判别法](https://blog.csdn.net/qq_45011547/article/details/90641858?ops_request_misc=%7B%22request%5Fid%22%3A%22164269493616780264032051%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=164269493616780264032051&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-90641858.first_rank_v2_pc_rank_v29&utm_term=%E4%B8%80%E8%87%B4%E6%94%B6%E6%95%9B&spm=1018.2226.3001.4187)，得到$x_0(t)+\sum_{j=1}^{\infty}||x_j(t)-x_{j-1}(t)||$是一致收敛的，因此$x_k(t)=x_0(t)+\sum_{j=1}^{k}[x_j(t)-x_{j-1}(t)]$当$k\to \infty$是一致收敛的，即$\{x_k(t)\}$是一致收敛，$Q.E.D.$
  - **证明$\{x_k(t)\}$的极限是等价连续积分方程的解，即原方程的解**
    - [一致收敛保证了函数序列的极限$x(t)$是连续的](https://blog.csdn.net/u010182633/article/details/56682133?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164270122816780271944686%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=164270122816780271944686&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-3-56682133.first_rank_v2_pc_rank_v29&utm_term=%E4%B8%80%E8%87%B4%E6%94%B6%E6%95%9B&spm=1018.2226.3001.4187)
    - $lim_{k\to\infty}x_k(t)=lim_{k\to\infty}[x_0+\int_{t_0}^{t}(A(\tau)x_{k-1}(\tau)+f(\tau))d\tau]=x_0+lim_{k\to\infty}\int_{t_0}^{t}(A(\tau)x_{k-1}(\tau)+f(\tau))d\tau=x_0+\int_{t_0}^{t}lim_{k\to\infty}(A(\tau)x_{k-1}(\tau)+f(\tau))d\tau=x_0+\int_{t_0}^{t}(A(\tau)x(\tau)+f(\tau))d\tau$，即$lim_{k\to\infty}x_k(t)=x_0+\int_{t_0}^{t}(A(\tau)x(\tau)+f(\tau))d\tau=x(t)$，$Q.E.D.$
  - **通过反证法证明唯一性**
    - 设$\tilde x(t),x(t)$都满足$\begin{cases} \begin{aligned} x'=A(t)x+f(t)\\ x(t_0)=x_0 \quad\end{aligned} \end{cases}$，令$y(t)=\tilde x(t)-x(t)$
    - $||y(t)||$连续有界，因此$||y(t)||\le L$，而$y'=A(t)y(t)$，类似第三步$y(t)=\int_{t_0}^{t}A(\tau)y(\tau)d\tau$，记此式为$*$
    - 对式$*$两边同时取范数，$||y(t)||=||\int_{t_0}^{t}A(\tau)y(\tau)d\tau||\le\int_{t_0}^{t}MLd\tau=ML|t-t_0|$，因此得到了$||y(t)||$新的界
    - 继续对式$*$两边同时取范数，并且代入上一次得到的新的界，得到$||y(t)||\le\int_{t_0}^{t}M(ML|\tau-t_0|)d\tau=\frac{LM^2}{2}|t-t_0|^2$
    - $\cdots$，重复上述步骤$k$次后，得到下述结果
    - $||y(t)||\le\frac{LM^k}{k!}|t-t_0|^k\le \frac{LM^k}{k!}(\beta-\alpha)^k$
    - $lim_{k\to\infty}||y(t)||\le lim_{k\to\infty}[\frac{LM^k}{k!}(\beta-\alpha)^k]=0$，$||y(t)||$与$k$无关，因此$||y(t)||=lim_{k\to\infty}||y(t)||=0$
    - $||y(t)||$的计算方法是$y(t)$的每一个函数取绝对值后相加，而它为$0$，因此它的每一个函数都是$0$，即$y(t)=\tilde x(t)-x(t)=0$，即$\tilde x(t)=x(t)$，$Q.E.D.$

## 2.齐次线性方程组的通解结构

- 对于$x'=A(t)x+f(t)$，如果$f(t)\equiv 0$是齐次方程，即$x'=A(t)x$，其中，$A(t)$是区间$[\alpha,\beta]$上的$n\times n$阶**连续**矩阵函数以保证解的存在和唯一

- 给定定义在区间$[\alpha,\beta]$上的$k$个向量函数$x_1(t),x_2(t),\cdots,x_k(t)$，如果存在$k$个不全为零的常数$c_1,c_2,\cdots,c_n$，使得线性组合$\sum_{k=1}^{n}c_kx_{k}(t)\equiv0,\forall t\in[\alpha,\beta]$，则称$k$个向量函数$x_1(t),x_2(t),\cdots,x_k(t)$线性相关

- 叠加原理：如果$x_1(t),x_2(t)$是齐次线性微分方程组$x'=A(t)x$的两个解，则$x(t)=c_1x_1(t)+c_2x_2(t)$也是其解，$c_1,c_2$是任意常数

- 由于叠加原理的存在，方程$x'=A(t)x$所有的解构成集合$\{x_k(t)\}$可以构成一个线性空间，它维数是$n$，$n$指的是$x(t)$中函数的个数。证明这个定理是思路是：

  - 在区间$[\alpha,\beta]$上一定存在$n$个线性无关的解$x_1(t),x_2(t),\cdots,x_n(t)$
    - 假设有$x_1(t),x_2(t),\cdots,x_n(t)$共$n$个解，它们的初始值是$x_1(t_0)=\begin{bmatrix}1\\0\\...\\0\end{bmatrix},x_2(t_0)=\begin{bmatrix}0\\1\\...\\0\end{bmatrix},\cdots,x_n(t_0)=\begin{bmatrix}0\\0\\...\\1\end{bmatrix}$
    - 显然$x_1(t_0),x_2(t_0),\cdots,x_n(t_0)$线性无关，假设存在$c_1,c_2,\cdots,c_n$使得$x_1(t),x_2(t),\cdots,x_n(t)$线性相关，那么$\sum_{k=1}^{n}c_kx_{k}(t)\equiv0,\forall t\in[\alpha,\beta]$
    - 当$t=t_0$时，只能$c_1=c_2,\cdots=c_k=0$，因此矛盾，故$x_1(t),x_2(t),\cdots,x_n(t)$线性无关，$Q.E.D.$
  - 方程组$x'=A(t)x$的任一解$x(t)$都可以表示为上述$n$个无关解的线性组合$x(t)=\sum_{k=1}^{n}c_kx_{k}(t),\forall t\in[\alpha,\beta]$
    - 假设有一解是$x(t)$，$x(t_0)\in \R^n$，因此，$\exists c_1,c_2,\cdots,c_n$，$x(t_0)=\sum_{k=1}^{n}c_kx_{k}(t_0)$
    - 令$y(t)=\sum_{k=1}^{n}c_kx_{k}(t)$，根据叠加原理$y(t)$也是$x'=A(t)x$的解，而$y(t_0)=x(t_0)$
    - 根据解的唯一性，$x(t)=y(t)=\sum_{k=1}^{n}c_kx_{k}(t)$，$Q.E.D.$

- $x'=A(t)x$的$n$个线性无关的解构成$x'=A(t)x$的基本解组，它们组成的矩阵$X(t)=\begin{bmatrix}x_{11}(t)&x_{12}(t)&...&x_{1n}(t)\\x_{21}(t)&x_{22}(t)&&\\...&&\ddots\\&\\x_{n1}(t)&...&...&x_{nn}(t)\end{bmatrix}$是基本解矩阵，它的每一列是一个解，其行列式$det(X(t))$是这个解组的$Wronski$行列式。如果在某点$t_0$处使得$X(t_0)=E$，则称$X(t)$是标准解矩阵

- $Liouville$定理：**解组**$x_1(t),x_2(t),\cdots,x_n(t)$**线性无关**的**充要**条件是它们的$Wronski$行列式在**某点处**取值不为零，进一步$det(X(t))=det(X(t_0))e^{\int^{t}_{t_0}tr(A(t))dt}$，$tr(A(t))=\sum_{k=1}^{n}a_{kk}(t)$

  - 根据行列式的定义以及函数和、积的求导公式，只需证明$\frac{d}{dt}det(X(t))=\sum_{k=1}^{n}a_{kk}(t)det(X(t))=tr(A(t))det(X(t))$，它的解即为$det(X(t))=det(X(t_0))e^{\int^{t}_{t_0}tr(A(t))dt}$，下面是证明过程：

    - 根据拉普拉斯展开，$det(X(t))=x_{11}(t)□+x_{12}(t)□+\cdots+x_{1n}(t)□$，其中$□$是$n-1$阶矩阵的行列式
    - 根据数学归纳法可以证明下面的式子：
    - $\frac{d}{dt}det(X(t))=[x_{11}'(t)□+x_{11}(t)□']+[x_{12}'(t)□+x_{12}(t)□']+\cdots+[x_{1n}(t)'□+x_{1n}(t)□']=det(\begin{bmatrix}x_{11}'(t)&x_{12}'(t)&...&x_{1n}'(t)\\x_{21}(t)&x_{22}(t)&&\\...&&\ddots\\&\\x_{n1}(t)&...&...&x_{nn}(t)\end{bmatrix})+det(\begin{bmatrix}x_{11}(t)&x_{12}(t)&...&x_{1n}(t)\\x_{21}'(t)&x_{22}'(t)&&\\...&&\ddots\\&\\x_{n1}(t)&...&...&x_{nn}(t)\end{bmatrix})+\cdots+det(\begin{bmatrix}x_{11}(t)&x_{12}(t)&...&x_{1n}(t)\\x_{21}(t)&x_{22}(t)&&\\...&&\ddots\\&\\x_{n1}'(t)&...&...&x_{nn}'(t)\end{bmatrix})$
    - 其中，第一个解满足$x_1'=A(t)x_1$，即$\begin{bmatrix}x_{11}'(t)\\x_{21}'(t)\\...\\x_{n1}'(t)\end{bmatrix}=\begin{bmatrix}a_{11}(t)&a_{12}(t)&...&a_{1n}(t)\\a_{21}(t)&a_{22}(t)&&\\...&&\ddots\\&\\a_{n1}(t)&...&...&a_{nn}(t)\end{bmatrix}\begin{bmatrix}x_{11}(t)\\x_{21}(t)\\...\\x_{n1}(t)\end{bmatrix}$，它的第一个分量$x_{11}'(t)=a_{11}(t)x_{11}(t)+a_{12}(t)x_{21}(t)+\cdots+a_{1n}(t)x_{n1}(t)=\sum_{j=1}^{n}a_{1j}(t)x_{j1}(t)$，而第二个解满足$\begin{bmatrix}x_{12}'(t)\\x_{22}'(t)\\...\\x_{n2}'(t)\end{bmatrix}=\begin{bmatrix}a_{11}(t)&a_{12}(t)&...&a_{1n}(t)\\a_{21}(t)&a_{22}(t)&&\\...&&\ddots\\&\\a_{n1}(t)&...&...&a_{nn}(t)\end{bmatrix}\begin{bmatrix}x_{12}(t)\\x_{22}(t)\\...\\x_{n2}(t)\end{bmatrix}$，它的第一个分量$x_{12}'(t)=a_{11}(t)x_{12}(t)+a_{12}(t)x_{22}(t)+\cdots+a_{1n}(t)x_{n2}(t)=\sum_{j=1}^{n}a_{1j}(t)x_{j2}(t)$，同理可以推得$x_{1k}'(t)=\sum_{j=1}^{n}a_{1j}(t)x_{jk}(t)$
    - 因此$det(\begin{bmatrix}x_{11}'(t)&x_{12}'(t)&...&x_{1n}'(t)\\x_{21}(t)&x_{22}(t)&&\\...&&\ddots\\&\\x_{n1}(t)&...&...&x_{nn}(t)\end{bmatrix})=det(\begin{bmatrix}\sum_{j=1}^{n}a_{1j}(t)x_{j1}(t)&\sum_{j=1}^{n}a_{1j}(t)x_{j2}(t)&...&\sum_{j=1}^{n}a_{1j}(t)x_{jn}(t)\\x_{21}(t)&x_{22}(t)&&\\...&&\ddots\\&\\x_{n1}(t)&...&...&x_{nn}(t)\end{bmatrix})=det(\begin{bmatrix}a_{11}(t)x_{11}(t)&a_{11}(t)x_{12}(t)&...&a_{11}(t)x_{1n}(t)\\x_{21}(t)&x_{22}(t)&&\\...&&\ddots\\&\\x_{n1}(t)&...&...&x_{nn}(t)\end{bmatrix})+det(\begin{bmatrix}\sum_{j=2}^{n}a_{1j}(t)x_{j1}(t)&\sum_{j=2}^{n}a_{1j}(t)x_{j2}(t)&...&\sum_{j=2}^{n}a_{1j}(t)x_{jn}\\x_{21}(t)&x_{22}(t)&&\\...&&\ddots\\&\\x_{n1}(t)&...&...&x_{nn}(t)\end{bmatrix})=a_{11}(t)det(X(t))+(0 \times j)=a_{11}(t)det(X(t))$
    - 因此可以归纳只有第二行求导的$det(\begin{bmatrix}x_{11}(t)&x_{12}(t)&...&x_{1n}(t)\\x_{21}'(t)&x_{22}'(t)&&\\...&&\ddots\\&\\x_{n1}(t)&...&...&x_{nn}(t)\end{bmatrix})=a_{22}(t)det(X(t))$
    - 因此$\frac{d}{dt}det(X(t))=tr(A(t))det(X(t))$，$Q.E.D.$
  - 从刘维尔公式可以判断当$X(t_0)=0$时，对区间内的其他$t$，朗斯基行列式$det(X(t))\equiv0$；而$X(t_0)\ne0$时，对区间内的其他$t$，朗斯基行列式$det(X(t))\not\equiv 0$，即**齐次线性方程组的解只有任意$t$代入都为$0$或任意$t$代入都不为$0$两种情况**

  - 现在证明**解组**$x_1(t),x_2(t),\cdots,x_n(t)$**线性无关**的**充要**条件是它们的$Wronski$行列式在**某点处**取值不为零：
    - 首先证明充分性$\Leftarrow$，即若解组的$Wronski$行列式在某点处取值不为零，证明它们线性无关：
      - 假设$x_1(t),x_2(t),\cdots,x_n(t)$线性相关，即存在不全为零的数$c_1,c_2,\cdots,c_n$使得$\sum_{k=1}^{n}c_kx_{k}(t)\equiv0,\forall t\in[\alpha,\beta]$，那么$\sum_{k=1}^{n}c_kx_{k}(t_0)=0$，那么$det(X(t_0))$为零，而已知条件是$Wronski$行列式在某点处取值不为零，根据**齐次线性方程组的解只有任意$t$代入都为$0$或任意$t$代入都不为$0$两种情况**，即出现矛盾，故$x_1(t),x_2(t),\cdots,x_n(t)$线性无关，$Q.E.D.$
    - 证明必要性$\Rightarrow$，即若解组$x_1(t),x_2(t),\cdots,x_n(t)$线性无关，证明$Wronski$行列式在某点处取值不为零
      - 设$\forall t\in[\alpha,\beta],det(X(t))\equiv0$，取$t=t_0$，$\begin{bmatrix}x_{11}(t_0)&x_{12}(t_0)&...&x_{1n}(t_0)\\x_{21}(t_0)&x_{22}(t_0)&&\\...&&\ddots\\&\\x_{n1}(t_0)&...&...&x_{nn}(t_0)\end{bmatrix}=0$，那么随便取一组不全为零的数$\begin{bmatrix}c_1\\c_2\\c_3\\...\\c_n\end{bmatrix}$，它们相乘$\begin{bmatrix}x_{11}(t_0)&x_{12}(t_0)&...&x_{1n}(t_0)\\x_{21}(t_0)&x_{22}(t_0)&&\\...&&\ddots\\&\\x_{n1}(t_0)&...&...&x_{nn}(t_0)\end{bmatrix}\begin{bmatrix}c_1\\c_2\\c_3\\...\\c_n\end{bmatrix}=0$，即存在一组非零的数使得$x_1(t),x_2(t),\cdots,x_n(t)$线性组合为$0$，那么它们线性相关，与已知条件线性无关矛盾，因此假设不成立，即不可能对所有$t$都使得$det(X(t))\equiv0$，即必存在某点处，使得$Wronski$行列式取值不为零，$Q.E.D.$
- 方程$x'=A(t)x$的通解可以表示为$x(t)=X(t)c$，且根据初值条件$x(t_0)=x_0$，可以得到$c=x_0X^{-1}(t_0)$，即$x(t)=X(t)c=x_0X(t)X^{-1}(t_0)=x_0X(t_0,t)$，此式就是满足初始条件的通解形式
## 3.非齐次线性方程组的通解
- 若$x^*(t)$是方程组$x'=A(t)x+f(t)$的解，则方程组的任意解$x(t)$都可以表示为$x(t)=X(t)c+x^*(t)$，其中$c$是$n$维常数列向量，$X(t)$是相应齐次方程组的基解矩阵
  - $x'=A(t)x+f(t)$的通解结构是$x(t)=X(t)c+x^*(t)$，即特解$+$基础解系
- $x'=A(t)x+f(t)$的解的线性组合是非齐次项的同样线性组合
- 通解定理：若矩阵函数$X(t)$是齐次方程组的基解矩阵，则非齐次线性方程组的通解为$x(t)=X(t)c+X(t)\int^t_{t_0}X^{-1}(\tau)f(\tau)d\tau$，方程组满足初值条件$x(t_0)=x_0$的解为$x(t)=X(t,t_0)x_0+\int^t_{t_0}X(t,\tau)f(\tau)d\tau$，其中$X(t,t_0)=X(t)X^{-1}(t_0)$
  - 设$x(t)=X(t)c(t)$是方程$x'=A(t)x+f(t)$的一个特解，求导得$X'(t)c(t)+X(t)c'(t)=A(t)X(t)c(t)+f(t)$
  - $X(t)$是基解矩阵，因此每个列都是方程$x'=A(t)x$的解，因此满足$x_{k}'(t)=A(t)x_{k}(t)$，由此可以得到$X'(t)=[x_1'(t),x_2'(t),\cdots,x_n'(t)]=[A(t)x_1(t),A(t)x_2(t),\cdots,A(t)x_n(t)]=A(t)[x_1(t),x_2(t),\cdots,x_n(t)]=A(t)X(t)$
  - 因此$X(t)c'(t)=f(t)$，$X(t)$是基解矩阵，因此有逆，$c'(t)=X(t)^{-1}f(t)$，两边积分$c(t)=\int^{t}_{t_0}(X(\tau)^{-1}f(\tau))d\tau+c$，取$c=0$，$x'=A(t)x+f(t)$的通解即为$x(t)=X(t)c+X(t)\int^{t}_{t_0}(X(\tau)^{-1}f(\tau))d\tau$
  - 若满足初始条件$x(t_0)=x_0=X(t_0)c+X(t_0)\int^{t_0}_{t_0}(X(\tau)^{-1}f(\tau))d\tau=X(t_0)c$，因此$c=X^{-1}(t_0)x_0$
  - 代入通解形式$x(t)=X(t)X^{-1}(t_0)x_0+X(t)\int^{t}_{t_0}(X(\tau)^{-1}f(\tau))d\tau=X(t,t_0)x_0+\int^t_{t_0}X(t,\tau)f(\tau)d\tau$,$Q.E.D.$
- 根据通解形式，求解线性方程组都归结为求解$X(t)$




## 4.高阶一维线性方程

- $n$阶线性微分方程的一般形式是$\frac{d^{n}x}{dt^{n}}+a_1(t)\frac{d^{n-1}x}{dt^{n-1}}+\cdots+a_n(t)x=f(t)$，其中$a_k(t)$在$[\alpha,\beta]$是连续函数，且$t$也定义在$[\alpha,\beta]$上，当$f(t)\equiv0$时，是齐次方程，否则是非齐次方程
- 令$x_1=x,x_2=\frac{dx}{dt},\cdots,x_n=\frac{d^{n-1}x}{dt^{n-1}}$
  - $x_1'=x'=x_2,x_2'=x_3,\cdots,x_{n-1}'=x_{n},x_n'=-a_1(t)x_n-\cdots-a_n(t)x_1+f(t)$
  - 显然，每个导数都是$x_1,x_2,\cdots,x_n$的线性组合，因此$\begin{bmatrix}x_1'(t)\\x_2'(t)\\...\\x_n'(t)\end{bmatrix}=A(t)\begin{bmatrix}x_1(t)\\x_2(t)\\...\\x_n(t)\end{bmatrix}+\begin{bmatrix}0\\0\\...\\f(t)\end{bmatrix}$，其中$A(t)=\begin{bmatrix}0&1&0&...&0\\0&0&1&...&0\\...\\0&0&0&...&1\\-a_n(t)&-a_{n-1}(t)&-a_{n-2}(t)&...&-a_1(t)\end{bmatrix}$
  - 以此**高阶一维**方程转化为了**一阶高维**线性方程组$\frac{dy}{dt}=A(t)y+g(t)$，其中$y=\begin{bmatrix}x_1(t)\\x_2(t)\\...\\x_n(t)\end{bmatrix},g(t)=\begin{bmatrix}0\\0\\...\\f(t)\end{bmatrix}$
- 由于最开始令$x_1=x$，因此$\frac{d^{n}x}{dt^{n}}+a_1(t)\frac{d^{n-1}x}{dt^{n-1}}+\cdots+a_n(t)x=f(t)$的解是$\frac{dy}{dt}=A(t)y+g(t)$的解的第一个元素$x_1(t)$
- 剩余的$\begin{bmatrix}x_2(t)\\x_3(t)\\...\\x_n(t)\end{bmatrix}=\begin{bmatrix}x'(t)\\x''(t)\\...\\x^{(n-1)}(t)\end{bmatrix}=\begin{bmatrix}x_1'(t)\\x_2'(t)\\...\\x_{n-1}'(t)\end{bmatrix}$，因此$\frac{d^{n}x}{dt^{n}}+a_1(t)\frac{d^{n-1}x}{dt^{n-1}}+\cdots+a_n(t)x=f(t)$的解和$\frac{dy}{dt}=A(t)y+g(t)$的解可以互相推出，即齐次高阶一维线性方程的解矩阵$\begin{bmatrix}x_{1}(t)&x_{2}(t)&...&x_{n}(t)\\x_{1}'(t)&x_{2}'(t)&&\\...&&\ddots\\&\\x_{1}^{n-1}(t)&...&...&x_{n}^{n-1}(t)\end{bmatrix}$对应转化为齐次一阶高维线性方程的基解矩阵$X(t)=\begin{bmatrix}x_{11}(t)&x_{12}(t)&...&x_{1n}(t)\\x_{21}(t)&x_{22}(t)&&\\...&&\ddots\\&\\x_{n1}(t)&...&...&x_{nn}(t)\end{bmatrix}$
- 满足初始条件$x(t_0)=x_0,x'(t_0)=x_1,\cdots,x^{n-1}(t_0)=x_{n-1}$的方程$\frac{d^{n}x}{dt^{n}}+a_1(t)\frac{d^{n-1}x}{dt^{n-1}}+\cdots+a_n(t)x=f(t)$的解是**存在的且唯一的**，可以通过转化为$\frac{dy}{dt}=A(t)y+g(t)$来证明
- 从解的角度上$\frac{dy}{dt}=A(t)y+g(t)$和$\frac{d^{n}x}{dt^{n}}+a_1(t)\frac{d^{n-1}x}{dt^{n-1}}+\cdots+a_n(t)x=f(t)$是“等价”的
- 定义$W(t)=det(\begin{bmatrix}x_{1}(t)&x_{2}(t)&...&x_{n}(t)\\x_{1}'(t)&x_{2}'(t)&&\\...&&\ddots\\&\\x_{1}^{n-1}(t)&...&...&x_{n}^{n-1}(t)\end{bmatrix})$是解组$\{x_k(t),k=1,2,\cdots,n\}$的$Wronski$行列式，其中的矩阵正是$\frac{d^{n}x}{dt^{n}}+a_1(t)\frac{d^{n-1}x}{dt^{n-1}}+\cdots+a_n(t)x=0$的解矩阵
- $x_1(t),x_2(t),\cdots,x_n(t)$线性无关 $\iff$ 若$x_1^{(k)}(t),x_2^{(k)}(t),\cdots,x_n^{(k)}(t)$线性无关，因为求导之后的式子等价
- $Liouville$定理：$n$阶齐次线性微分方程$\frac{d^{n}x}{dt^{n}}+a_1(t)\frac{d^{n-1}x}{dt^{n-1}}+\cdots+a_n(t)x=0$的解组$\{x_k(t),k=1,2,\cdots,n\}$线性无关的充要条件是它的$Wronski$行列式$W(t)$在区间$[\alpha,\beta]$上恒不为零，这等价于$W(t)$在区间$[\alpha,\beta]$上的某点$t_0$处不为零。方程的任一解组$\{x_k(t),k=1,2,\cdots,n\}$的$Wronski$行列式满足$Liouville$公式，$W(t)=W(t_0)e^{-\int_{t_0}^ta_1(\tau)d\tau}$
  - 这表明$\frac{d^{n}x}{dt^{n}}+a_1(t)\frac{d^{n-1}x}{dt^{n-1}}+\cdots+a_n(t)x=0$解组对应的$W(t)$只有两种情况：在任一点$t$都不为零；在任一点$t$都为零
  - 此处$Liouville$定理的证明可以通过[$x'=A(t)x$的$Liouville$定理](https://blog.csdn.net/weixin_52812620/article/details/122613501)来推导记忆：此处的$A(t)=\begin{bmatrix}0&1&0&...&0\\0&0&1&...&0\\...\\0&0&0&...&1\\-a_n(t)&-a_{n-1}(t)&-a_{n-2}(t)&...&-a_1(t)\end{bmatrix}$，因此原公式中的$tr(A(t))$显然就是此公式中的$-a_1(t)$，对应的此处的$\begin{bmatrix}x_{1}(t)&x_{2}(t)&...&x_{n}(t)\\x_{1}'(t)&x_{2}'(t)&&\\...&&\ddots\\&\\x_{1}^{n-1}(t)&...&...&x_{n}^{n-1}(t)\end{bmatrix}$中的$n$个函数$x_1(t),\cdots,x_n(t)$就是方程$\frac{d^{n}x}{dt^{n}}+a_1(t)\frac{d^{n-1}x}{dt^{n-1}}+\cdots+a_n(t)x=0$的$n$个解，$W(t_0)$就是原方程的$det(X(t_0))$
- 齐次线性微分方程$\frac{d^{n}x}{dt^{n}}+a_1(t)\frac{d^{n-1}x}{dt^{n-1}}+\cdots+a_n(t)x=0$在区间$[\alpha,\beta]$上存在$n$个线性无关的解$x_1(t),\cdots,x_n(t)$，且方程的通解是$x(t)=\sum_{k=1}^nc_kx_k(t)$，其中，$c_1,\cdots,c_n$是任意常数
- 非齐次线性微分方程的通解是$x(t)=\sum_{k=1}^nc_kx_k(t)+x^*(t)$，其中$c_1,\cdots,c_n$是任意常数，而$x^*=\sum_{k=1}^n\int^{t}_{t_0}\frac{x_k(t)W_k(\tau)}{W(\tau)}f(\tau)d\tau$是非齐次方程的一个特解，$W(t)$是解组的$Wronski$行列式，$W_k(t)$是$W(t)$中第$n$行第$k$列元素的代数余子式
  - 此处的非齐次高阶一维线性方程也可以对比[非齐次一阶高维线性方程的解](https://blog.csdn.net/weixin_52812620/article/details/122613501)（由于最开始时令$x_1=x$，即非齐次一阶高维线性方程的特解的第一行对应非齐次高阶一维线性方程的特解），非齐次一阶高维线性方程的特解是$X(t)\int^{t}_{t_0}(X(\tau)^{-1}f(\tau))d\tau$，将非齐次高阶一维线性方程转化为对应的非齐次一阶高维线性方程$\frac{dy}{dt}=A(t)y+g(t)$后，可以推出它的特解是$\begin{bmatrix}x_{1}(t)&x_{2}(t)&...&x_{n}(t)\\x_{1}'(t)&x_{2}'(t)&&\\...&&\ddots\\&\\x_{1}^{n-1}(t)&...&...&x_{n}^{n-1}(t)\end{bmatrix}\int^{t}_{t_0}\begin{bmatrix}x_{1}(\tau)&x_{2}(\tau)&...&x_{n}(\tau)\\x_{1}'(\tau)&x_{2}'(\tau)&&\\...&&\ddots\\&\\x_{1}^{n-1}(\tau)&...&...&x_{n}^{n-1}(\tau)\end{bmatrix}^{-1}\begin{bmatrix}0\\0\\0\\0\\...\\f(\tau)\end{bmatrix}d\tau$，由于$g(\tau)$的前$n-1$个元素都是$0$，因此逆矩阵只需要计算最后一列，而$A^{-1}=\frac{1}{det(A)}A^*$，因此根据[伴随矩阵的定义和求法](https://blog.csdn.net/weixin_52812620/article/details/122587375)，$\begin{bmatrix}x_{1}(\tau)&x_{2}(\tau)&...&x_{n}(\tau)\\x_{1}'(\tau)&x_{2}'(\tau)&&\\...&&\ddots\\&\\x_{1}^{n-1}(\tau)&...&...&x_{n}^{n-1}(\tau)\end{bmatrix}^{-1}=\frac{1}{W(\tau)}\begin{bmatrix}*&*&...&W_1(\tau)\\*&*&&\\...&&\ddots\\&\\*&...&...&W_n(\tau)\end{bmatrix}$，其中$W_k(\tau)$是$W(\tau)$中第$n$行第$k$列元素的代数余子式，例如根据公式，$W_1(t)$的位置应该是$A_{n1}$，即划去第$n$行第$1$列后的代数余子式，因此特解$\begin{bmatrix}x_{1}(t)&x_{2}(t)&...&x_{n}(t)\\x_{1}'(t)&x_{2}'(t)&&\\...&&\ddots\\&\\x_{1}^{n-1}(t)&...&...&x_{n}^{n-1}(t)\end{bmatrix}\int^{t}_{t_0}\frac{1}{W(\tau)}\begin{bmatrix}*&*&...&W_1(\tau)\\*&*&&\\...&&\ddots\\&\\*&...&...&W_n(\tau)\end{bmatrix}\begin{bmatrix}0\\0\\0\\0\\...\\f(\tau)\end{bmatrix}d\tau=\begin{bmatrix}x_{1}(t)&x_{2}(t)&...&x_{n}(t)\\x_{1}'(t)&x_{2}'(t)&&\\...&&\ddots\\&\\x_{1}^{n-1}(t)&...&...&x_{n}^{n-1}(t)\end{bmatrix}\int^{t}_{t_0}\frac{1}{W(\tau)}\begin{bmatrix}W_1(\tau)f(\tau)\\W_2(\tau)f(\tau)\\W_3(\tau)f(\tau)\\W_4(\tau)f(\tau)\\\cdots\\W_n(\tau)f(\tau)\end{bmatrix}d\tau=\begin{bmatrix}x_{1}(t)&x_{2}(t)&...&x_{n}(t)\\x_{1}'(t)&x_{2}'(t)&&\\...&&\ddots\\&\\x_{1}^{n-1}(t)&...&...&x_{n}^{n-1}(t)\end{bmatrix}\begin{bmatrix}\int^{t}_{t_0}\frac{1}{W(\tau)}W_1(\tau)f(\tau)d\tau\\\int^{t}_{t_0}\frac{1}{W(\tau)}W_2(\tau)f(\tau)d\tau\\\int^{t}_{t_0}\frac{1}{W(\tau)}W_3(\tau)f(\tau)d\tau\\\int^{t}_{t_0}\frac{1}{W(\tau)}W_4(\tau)f(\tau)d\tau\\\cdots\\\int^{t}_{t_0}\frac{1}{W(\tau)}W_n(\tau)f(\tau)d\tau\end{bmatrix}$，因为只需要得到第一行的$x_1(t),\cdots,x_n(t)$，因此对应相乘后再相加，即得到$x^*=\sum_{k=1}^n\int^{t}_{t_0}\frac{x_k(t)W_k(\tau)}{W(\tau)}f(\tau)d\tau$是非齐次方程的一个特解
## 5.复值解和级数解法

- 方程在$R^n$内无解，而在$C^n$内可能有解，扩大考虑范围可能得到更多解
- 对于复值矩阵函数$A(t)=A_R(t)+iA_I(t)$，定义求导与积分
  - $\frac{dA}{dt}=\frac{dA_R(t)}{dt}+i\frac{dA_I(t)}{dt}$
  - $\int^{\beta}_{\alpha}A(t)dt=\int^{\beta}_{\alpha}A_R(t)+i\int^{\beta}_{\alpha}A_I(t)$
- 欧拉公式：$\lambda=a+ib,e^{\lambda t}=e^{a t}(cosbt+isinbt)$
- 设$z(t)=x(t)+iy(t)$，复值函数$z(t)$与其共轭$\bar{z(t)}$线性无关 $\iff$ 它们的实部$x(t)$和虚部$y(t)$线性无关
- 设$A(t)=A_R(t)+iA_I(t)$是复值连续矩阵，$f(t)=f_R(t)+if_I(t)$是复制连续列向量，若$z(t)=x(t)+iy(t)$是线性方程组$\frac{dz}{dt}=A(t)z+f(t)$的复值解，则实连续函数$x=x(t),y=y(t)$是$\frac{d}{dt}\begin{bmatrix}x\\y\end{bmatrix}=\begin{bmatrix}A_R(t)&-A_I(t)\\A_I(t)&A_R(t)\end{bmatrix}\begin{bmatrix}x\\y\end{bmatrix}+\begin{bmatrix}f_R(t)\\f_I(t)\end{bmatrix}$的解
  - 注意$x(t)$和$y(t)$都是多维列向量函数，而不是一般函数
  - 复值向量函数$z(t)=x(t)+iy(t)$是实系数矩阵（即$A(t)=A_R(t)$）齐次线性方程组$\frac{dz}{dt}=A(t)z$的解 $\iff$ 实部$x(t)$和虚部$y(t)$都是方程$\frac{dz}{dt}=A(t)z$的解，$\bar{z(t)}$也是$\frac{dz}{dt}=A(t)z$的解，可以理解为齐次线性方程组的叠加原理
- $Cauchy$定理：如果函数$f(x,t)$在矩阵区域$R=\{(t,x)\in\R^2:|t-t_0|<\alpha,|x-x_0|<\beta\}$上可以展开成$f(t,x)=\sum^{\infty}_{k,l=0}\gamma_{kl}(t-t_0)^k(x-x_0)^l$（此时$f(t,x)$可解析，即多元函数可展开为收敛的幂级数），则初值问题$\frac{dx}{dt}=f(t,x),x(t_0)=x_0$在$t_0$附近**存在唯一的**解析解（解析解指解函数可以展开成幂级数）

  - 证明存在唯一性，即已知有解析解，证明它唯一：

    - 将解$x(t)$在$t_0$处泰勒展开，$x(t)=x(t_0)+x'(t_0)(t-t_0)+\cdots+\frac{x^{(n)}(t_0)}{n!}(t-t_0)^n=x_0+\sum^{\infty}_{j=1}c_j(t-t_0)^j$是解析解所泰勒展开的幂级数，要证明解的唯一性只需证明$c_1,c_2,\cdots,c_{n}$是惟一的
    - 因为$x(t)$是方程$\frac{dx}{dt}=f(t,x)$的解即满足$\frac{dx}{dt}=f(t,x(t))$，$c_1=x'(t_0)=f(t_0,x(t_0))=f(t_0,x_0)$，因为$f(t,x)$是可解析函数，根据[多元函数的泰勒展开](https://blog.csdn.net/waihekor/article/details/104955678?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164284296616780269882526%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=164284296616780269882526&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-6-104955678.first_rank_v2_pc_rank_v29&utm_term=%E5%A4%9A%E5%85%83%E5%87%BD%E6%95%B0%E6%B3%B0%E5%8B%92%E5%85%AC%E5%BC%8F&spm=1018.2226.3001.4187)，$f(t,x)$在点$(t_0,x_0)$处的泰勒展开为$f(t,x)=f(t_0,x_0)+(t-t_0)f'_t(t_0,x_0)+(x-x_0)f'_x(t_0,x_0)+\frac{1}{2!}(t-t_0)^2f''_{tt}(t_0,x_0)+\frac{1}{2!}(t-t_0)(x-x_0)f''_{xt}(t_0,x_0)+\frac{1}{2!}(t-t_0)(x-x_0)f''_{tx}(t_0,x_0)+\frac{1}{2!}(x-x_0)^2f''_{xx}(t_0,x_0)+O^n$，因此$c_1=f(t_0,x_0)+0=f(t_0,x_0)$，即$f(t,x)=\sum^{\infty}_{k,l=0}\gamma_{kl}(t-t_0)^k(x-x_0)^l$中$k=l=0$的情况（此时只有常数项），而$c_2=\frac{1}{2!}x''(t_0)=\frac{1}{2!}[f'_t(t_0,x(t_0))+f'_x(t_0,x(t_0))f(t_0,x_0)]$（$x''(t)$相当于$x'(t)$对$t$求导，即$f(t,x(t))$对$t$求导，根据链式法则，$\frac{\partial f}{\partial t}=\frac{\partial f}{\partial t}+\frac{\partial f}{\partial x}\frac{dx}{dt}$），对比式子$f(t,x)=\sum^{\infty}_{k,l=0}\gamma_{kl}(t-t_0)^k(x-x_0)^l$和$f(t,x)=f(t_0,x_0)+(t-t_0)f'_t(t_0,x_0)+(x-x_0)f'_x(t_0,x_0)+\frac{1}{2!}(t-t_0)^2f''_{tt}(t_0,x_0)+\frac{1}{2!}(t-t_0)(x-x_0)f''_{xt}(t_0,x_0)+\frac{1}{2!}(t-t_0)(x-x_0)f''_{tx}(t_0,x_0)+\frac{1}{2!}(x-x_0)^2f''_{xx}(t_0,x_0)+O^n$，$f'(t_0,x_0)$对应的$\gamma$是$t-t_0$取$1$，而$x-x_0$取$0$的时候，即$f'(t_0,x_0)=\gamma_{10}$，以此类推，$f'_x(t_0,x_0)=\gamma_{01}$，因此$c_2=\frac{1}{2!}(\gamma_{10}+\gamma_{01}\gamma_{00})$，归纳推得$c_n=P(\gamma_{00},\gamma_{00},\gamma_{01},\cdots,\gamma_{n-1,0})$，其中$P$是一个正系数多项式，因此每个$c_k$都是惟一确定的公式计算得到的，$Q.E.D.$
  - [具体证明$Cauchy$定理方法](https://zhuanlan.zhihu.com/p/104265080)
  - 推论：如果方程$\frac{d^2x}{dt^2}=a(t)\frac{dx}{dt}+b(t)x=0$的系数$a(t)$和$b(t)$在区间$|t-t_0|<\rho$内都可以展开成$(t-t_0)$的收敛的幂级数，则方程$\frac{d^2x}{dt^2}+a(t)\frac{dx}{dt}+b(t)x=0$在区间$|t-t_0|<\rho$上存在收敛的幂级数解$x(t)=\sum^{\infty}_{j=0}c_j(t-t_0)^j$
    - $\frac{d^2x}{dt^2}+a(t)\frac{dx}{dt}+b(t)x=0$可以写成$\begin{bmatrix}x\\x'\end{bmatrix}'=\begin{bmatrix}0&1\\-b(t)&-a(t)\end{bmatrix}\begin{bmatrix}x\\x'\end{bmatrix}$，而利用$Cauchy$定理可以证明高维的一阶的方程如果满足条件，仍然会有幂级数解
    - 为了计算其解$x(t)$，先把$a(t)$，$b(t)$展开成幂级数$a(t)=\sum_{j=0}^{\infty}a_j(t-t_0)^j$，$b(t)=\sum_{j=0}^{\infty}b_j(t-t_0)^j$，然后代入$\frac{d^2x}{dt^2}+a(t)\frac{dx}{dt}+b(t)x=0$，利用待定系数法，对应同幂次的系数，即可确定幂级数的系数$c_1,\cdots,c_n$



# 四、常系数线性方程

## 1.高阶一维齐次方程的基解矩阵

- 考虑$n$维线性微分方程$\frac{d^{n}x}{dt^{n}}+a_1\frac{d^{n-1}x}{dt^{n-1}}+\cdots+a_nx=f(t)$，如果引入微分算子$D=\frac{d}{dt}$的记号，可以化简为$P(D)x=f(t)$，其中$P(D):=D^n+a_1D^{n-1}+\cdots+a_{n-1}D+a_n$，$P(D)$是关于$D$的多项式，称为算子多项式
- 微分算子$D$可以看作是连续函数空间$C^0(J)$上函数到函数的映射，这里$J$是函数的定义域。算子$D$的定义域为连续可微函数空间$C^1(J)$，这是$C^0(J)$的一个子空间。可以看出，$C^0(J)$和$C^1(J)$都是线性空间，而算子$D$是上面的一个线性算子
- [算子多项式可以进行类似于代数运算的因式分解或因式相乘展开](https://www.doc88.com/p-4925081754353.html)，因此微分算子$D$运算中也满足二项式定理等，即$(D+\lambda)^m=\sum^m_{k=0}C^k_mD^k\lambda^{m-k}$，其中$D$是微分算子，$\lambda$是常数

- 对于齐次方程$\frac{d^{n}x}{dt^{n}}+a_1\frac{d^{n-1}x}{dt^{n-1}}+\cdots+a_nx=0$的通解，可以采用$Euler$待定指数函数法，也就是寻求形如$x(t)=e^{\lambda t}$的解，其中$\lambda$是待定指数

- 设多项式$P(\lambda)=\lambda^n+a_1\lambda^{n-1}+\cdots+a_{n-1}\lambda+a_n=0$有$n$个互异$\lambda_1,\cdots,\lambda_n$的根，则$e^{\lambda_1 t},\cdots,e^{\lambda_n t}$是方程$\frac{d^{n}x}{dt^{n}}+a_1\frac{d^{n-1}x}{dt^{n-1}}+\cdots+a_nx=0$的解

  - $De^{\lambda t}=\lambda e^{\lambda t},D^ne^{\lambda t}=\lambda^n e^{\lambda t}$，那么$P(D)e^{\lambda t}=P(\lambda)e^{\lambda t}=0$，只有$P(\lambda)=0$
  - $P(\lambda)$是特征多项式，$P(\lambda)=0$是特征方程，$P(\lambda)=0$的根是特征值

  - 朗斯基行列式$W(t)$化简后出现范德蒙行列式，可以证明$n$个互异$\lambda_1,\cdots,\lambda_n$的$e^{\lambda_1 t},\cdots,e^{\lambda_n t}$是方程$\frac{d^{n}x}{dt^{n}}+a_1\frac{d^{n-1}x}{dt^{n-1}}+\cdots+a_nx=0$的解

  - 互异的根可以是复根，使用欧拉公式展开后分别取实部和虚部，因为[复值向量函数$z(t)=x(t)+iy(t)$是实系数矩阵（即$A(t)=A_R(t)$）齐次线性方程组$\frac{dz}{dt}=A(t)z$的解 $\iff$ 实部$x(t)$和虚部$y(t)$都是方程$\frac{dz}{dt}=A(t)z$的解，$\bar{z(t)}$也是$\frac{dz}{dt}=A(t)z$的解，可以理解为齐次线性方程组的叠加原理](https://blog.csdn.net/weixin_52812620/article/details/122632185)

- 设多项式方程$P(\lambda)=\lambda^n+a_1\lambda^{n-1}+\cdots+a_{n-1}\lambda+a_n=0$只有$r$个互异的根，它们分别有重数$n_1,\cdots,n_r$（自然$n_1+\cdots+n_r=n$，则$\begin{matrix}e^{\lambda_1 t}&te^{\lambda_1 t}&\cdots&t^{n_1-1}e^{\lambda_1 t}\\\cdots&\cdots&\cdots&\cdots&\\e^{\lambda_r t}&te^{\lambda_r t}&\cdots&t^{n_r-1}e^{\lambda_r t}&\end{matrix}$构成齐次方程$\frac{d^{n}x}{dt^{n}}+a_1\frac{d^{n-1}x}{dt^{n-1}}+\cdots+a_nx=0$的基本解组

  - $\begin{matrix}e^{\lambda_1 t}&te^{\lambda_1 t}&\cdots&t^{n_1-1}e^{\lambda_1 t}\\\cdots&\cdots&\cdots&\cdots&\\e^{\lambda_r t}&te^{\lambda_r t}&\cdots&t^{n_r-1}e^{\lambda_r t}&\end{matrix}$表示的是$n$个函数，而不是一个矩阵，因为第一行有$n_1$个函数，最后一行有$n_r$个函数，重数不一定相等，$n_1+\cdots+n_r=n$，每一行的函数个数相加就是$n$个线性无关的解函数

  - 证明一般形式$e^{\lambda t}t^l$是$\frac{d^{n}x}{dt^{n}}+a_1\frac{d^{n-1}x}{dt^{n-1}}+\cdots+a_nx=0$方程的解，只需证明$P(D)(e^{\lambda t}t^l)=0$

    - [乘积函数求导的$Leibniz$公式$(uv)^{(m)}=\sum^m_{k=0}C^k_mu^{(k)}v^{(m-k)}$](https://blog.csdn.net/weixin_39669147/article/details/112767453)

    - $D^m(e^{\lambda t}t^l)=\sum_{k=0}^mC^k_m(e^{\lambda t})^{(k)}(t^l)^{(m-k)}=\sum_{k=0}^mC^k_m\lambda^ke^{\lambda t}D^{m-k}t^l=e^{\lambda t}(\sum_{k=0}^mC^k_m\lambda^kD^{m-k}t^l)=e^{\lambda t}(\sum_{k=0}^mC^k_m\lambda^kD^{m-k})t^l=e^{\lambda t}(D+\lambda)^mt^l$
    - $P(D)(e^{\lambda t}t^l)=e^{\lambda t}P(D+\lambda)t^l$
    - 算子多项式$P(D)$作用于$t^l$时，求导次数$m>l$时失效，即$D^m(t^l)=0,m>l$，$P(D+\lambda)$是关于$D$的多项式函数，将其在$D=0$处展开且略去$D$的次数大于$l$的情况，$P(D+\lambda)=P(\lambda)+\frac{P'(\lambda)}{1!}D+\cdots+\frac{P^{(l)}(\lambda)}{l!}D^l$
    - $P(D)(e^{\lambda t}t^l)=e^{\lambda t}P(D+\lambda)t^l=e^{\lambda t}(P(\lambda)t^l+lP'(\lambda)t^{l-1}+\cdots+P^{(l)}(\lambda))$
    - $P(\lambda)=(\lambda-\lambda_i)^{n_i}Q(\lambda)\Rightarrow P(\lambda_i)=0$，$P'(\lambda)=n_i(\lambda-\lambda_i)^{n_i-1}Q(\lambda)+(\lambda-\lambda_i)^{n_i}Q'(\lambda)\Rightarrow P'(\lambda_i)=0$，$\cdots$，$P^{(n_i-1)}(\lambda_i)=0$，因此只需$l\le n_i-1$，$P^{l}(\lambda_i)=0$
    - 当$l\le n_i$时，$P(D)(e^{\lambda_i t}t^l)=e^{\lambda_i t}P(D+\lambda_i)t^l=e^{\lambda_i t}(P(\lambda_i)t^l+lP'(\lambda_i)t^{l-1}+\cdots+P^{(l)}(\lambda_i))=e^{\lambda_i t}(0\times t^l+l\times0\times t^{l-1}+\cdots+0)=0$，此时，$e^{\lambda_i t}t^l$满足$P(D)e^{\lambda_i t}t^l=0$，即此时$e^{\lambda_i t}t^l$是齐次方程$P(D)x=0$的解
    - 因此，只需$t$的次数$l$小于等于对应特征值$\lambda_i$的重数$n_i$时，$e^{\lambda_i t}t^l$就是齐次方程的解，因此$\begin{matrix}e^{\lambda_1 t}&te^{\lambda_1 t}&\cdots&t^{n_1-1}e^{\lambda_1 t}\\\cdots&\cdots&\cdots&\cdots&\\e^{\lambda_r t}&te^{\lambda_r t}&\cdots&t^{n_r-1}e^{\lambda_r t}&\end{matrix}$是方程$P(d)x=0$的解
- 设实系数齐次方程$P(D)x=0$有$r$个互异的实特征根$\lambda_1,\cdots,\lambda_r$及$l$对互异的复特征根$\alpha_1\pm i\beta_1,\cdots,\alpha_l\pm i\beta_l$，重数分别为$n_1,\cdots,n_r$和$m_1,\cdots,m_l$，且满足$\sum^r_{k=1}n_k+2\sum^l_{k=1}m_k=n$，则齐次方程$P(D)x=0$有以下实解并组成基本解组$\begin{matrix}e^{\lambda_kt}&te^{\lambda_kt}&\cdots&t^{n_k-1}e^{\lambda_kt}&k=1,2,\cdots,r\\e^{\alpha_jt}cos\beta_jt&te^{\alpha_jt}cos\beta_jt&\cdots&t^{m_j-1}e^{\alpha_jt}cos\beta_jt&j=1,2,\cdots,l\\e^{\alpha_jt}sin\beta_jt&te^{\alpha_jt}sin\beta_jt&\cdots&t^{m_j-1}e^{\alpha_jt}sin\beta_jt&j=1,2,\cdots,l\end{matrix}$
## 2.微分算子法，比较系数法，拉普拉斯变换法
- 非齐次方程$P(D)x=f(t)$的解可以通过基础解$+$特解表示，通过[将高阶一维方程转化为一阶高维方程](https://blog.csdn.net/weixin_52812620/article/details/122632185?spm=1001.2014.3001.5502)，[只需求得基解矩阵即可根据公式求出通解的一般表示](https://blog.csdn.net/weixin_52812620/article/details/122613501?spm=1001.2014.3001.5502)
- 对于特殊的$f(t)$有其他更简单的方法：[微分算子法](https://zhuanlan.zhihu.com/p/429780174)，比较系数法，[拉普拉斯变换](https://zhuanlan.zhihu.com/p/107378411)和[拉普拉斯变换法求解常微分方程](https://zhuanlan.zhihu.com/p/107466016)
- 关于微分算子（$P(D)$是求微分，而$\frac{1}{P(D)}$是求积分，$\frac{1}{P(D)}/P^{-1}(D)$也是一种特殊的微分算子）：
  - 对$k$次多项式$f_k(t)$，如果$\frac{1}{P(x)}$在$x=0$处解析，且可以展开成$\frac{1}{P(x)}=Q_k(x)+H_k(x)$，其中$Q_k(x)$是$k$次多项式，而$H_k(x)$是$k+1$次以上的所有高次项，则$\frac{1}{P(D)}f_k(t)=Q_k(D)f_k(t)$，即$\frac{1}{P(D)}$中包含了求$k$次及以下导的部分$Q_k(D)$和求$k+1$及以上次导的部分$H_k(x)$，而对于最高次为$k$的$f_k(t)$,求$k+1$及以上次导后就会变成$0$而失效
    - $\frac{1}{1-D}f_k(t)=(1+D+D^2+\cdots+D^k)f_k(t)$，$\frac{1}{1-x}$的展开式实际有无数多项$1+x+x^2+\cdots+x^n$，而只有前$k$项作用于$f_k(t)$之后才会产生非零的式子
  - 若$P(\lambda)\ne0$，那么$\frac{1}{P(D)}e^{\lambda t}=\frac{1}{P(\lambda)}e^{\lambda t}$
    - $\frac{1}{P(D^2)}e^{iat}=\frac{1}{P(-a^2)}e^{iat}$，只要把$D$替换为$\lambda$即可
  - $\frac{1}{P(D)}e^{\lambda t}v(t)=e^{\lambda t}\frac{1}{P(D+\lambda)}v(t)$
    - 当$P(\lambda)=0$时，$\frac{1}{P(D)}e^{\lambda t}=\frac{1}{P(D)}e^{\lambda t}t^0=e^{\lambda t}\frac{1}{P(D+\lambda)}t^0=e^{\lambda t}\frac{1}{P(D+\lambda)}1$，如果$\frac{1}{P(D+\lambda)}$在$D=0$处可以展开，那么根据第一个性质，$\frac{1}{P(D+\lambda)}$作用于$1$之后，只有$D$为$0$次的项才有效，$\frac{1}{P(D+\lambda)}$根据泰勒公式在$D=0$处展开后，第一项就是$\frac{1}{P(\lambda)}$即$\frac{1}{P(D)}e^{\lambda t}=e^{\lambda t}\frac{1}{P(D+\lambda)}1=e^{\lambda t}\frac{1}{P(\lambda)}1=\frac{1}{P(\lambda)}e^{\lambda t}$
- 对于$P(D)x=f(t)$下面两个类型适合使用比较系数法：
  - $f(t)=(b_0t^k+b_1t^{k-1}+\cdots+b_{k-1}t+b_k)e^{\lambda_0 t}$，，$\lambda_0$是$P(\lambda)$的$m$重特征根（$m$可以是$0$）
    - 此时有特解$x^*(t)=t^m(c_0t^k+c_1t^{k-1}+\cdots+c_{k-1}t+c_k)e^{\lambda_0 t}$，其中$c_0,\cdots,c_k$是待定的常数，通过比较系数可以确定待定的常数
  - $f(t)=[p_k(t)cos\beta t+q_k(t)sin\beta t]e^{\lambda t}$，$\alpha$和$\beta$是实数，$p_k(t)$和$q_k(t)$都是最高为$k$次的多项式，$\alpha+i\beta$是$P(\lambda)=0$的$m$重特征根（$m$可以是$0$）
    - 此时有特解$x^*(t)=t^m(A_k(t)cos\beta t+B_k(t)sin\beta t)e^{\alpha t}$，其中$A_k(t)$和$B_k(t)$都是最高为$k$次的待定多项式
## 3.一阶高维线性方程组
- 对于常系数一阶高维线性方程$x'=Ax+f(t)$，[最后都落到求解齐次方程$x'=Ax$的基解矩阵上](https://blog.csdn.net/weixin_52812620/article/details/122613501)

- 如果$A$有$n$个互异的特征值$\lambda_1,\cdots,\lambda_n$和其对应的特征向量$c_1,\cdots,c_n$，则$x'=Ax$的基解矩阵是$\phi(t)=\begin{bmatrix}c_1e^{\lambda t}&c_2e^{\lambda t}&\cdots&c_ne^{\lambda t}\end{bmatrix}$，这里的基解矩阵可能是复矩阵

- 齐次方程组$x'=Ax$有基解矩阵$X(t)=e^{At}$，（此处的$A$不是系数矩阵），它是[标准解矩阵](https://blog.csdn.net/weixin_52812620/article/details/122613501)，因为它满足$X(0)=E$；由[推论](https://blog.csdn.net/weixin_52812620/article/details/122613501)相应得到非齐次线性方程组$x'=Ax+f(t)$的通解是$x(t)=e^{At}c+\int^t_{t_0}e^{A(t-s)}f(s)ds$

- 由于每个特征值$\lambda_i$所能解出的特征向量的个数$\le$重数$n_i$（即[几何重数 $\le$代数重数](https://blog.csdn.net/kodoshinichi/article/details/109351452)），若每个特征值都可以解出$n_i$个线性无关的特征向量（即对于每个特征值$\lambda_i$都有几何重数$=$代数重数，矩阵可对角化），求解基解矩阵与存在$n$个互异的特征值$\lambda_1,\cdots,\lambda_n$类似，但有些时候并非如此，**一般特征子空间**（非广义）与[线性变换的特征子空间](https://blog.csdn.net/kodoshinichi/article/details/109351452)类似，无法对全空间特征分解，这时就需要引入[广义特征向量](https://zhuanlan.zhihu.com/p/373513480)

- 假设$\lambda_i$是$A$的$n_i$重特征根（$i=1,2,\cdots,s$，$n_1+n_2+\cdots+n_s=n$），若满足$V_i=\{c\in C^n|(A-\lambda_iE)^{n_i}c=0\}$（$(\lambda_iE-A)^{n_i}x=0$一定能找到$n_i$个线性无关的解向量$x$，称为广义特征向量，[对广义特征向量的一些计算和相关证明](https://www.docin.com/p-1025805464.html)）称$V_i$是$\lambda_i$对应的**广义特征子空间**，对空间$C^n$有直和分解$C^n=V_1\oplus V_2\oplus \cdots\oplus V_s$ ，$\forall \eta\in C^n,\eta=v_1+v_2+\cdots+v_s$，$v_1\in V_1,\cdots,v_s\in V_s$

- 如果出现了重根，则$x'=Ax$通解的求解方法有如下几种：

  - 空间分解法：

    - $e^{At}\eta=e^{At}(v_1+\cdots+v_s)=e^{At}v_1+\cdots+e^{At}v_s$

    - $e^{At}v_i=e^{(A+\lambda_iE-\lambda_iE)t}v_i=e^{\lambda_iEt+(A-\lambda_iE)t}v_i$，由于$\lambda_iEt$是数量矩阵，因此[与任意矩阵可交换](https://blog.csdn.net/weixin_52812620/article/details/122587152#1_174)，根据[矩阵函数的定义和性质：若$AB=BA$，则$e^{A+B}=e^Ae^B$](https://blog.csdn.net/kodoshinichi/article/details/109612420)
    - $e^{At}v_i=e^{\lambda_iEt}e^{(A-\lambda_iE)t}v_i=e^{\lambda_iEt}[I+\frac{(A-\lambda_iE)t}{1!}+\frac{(A-\lambda_iE)^2t^2}{2!}+\cdots+\frac{(A-\lambda_iE)^nt^n}{n!}]v_i$
    - 因为$v_i\in V_i$，所以$(A-\lambda_iE)^{n_i}v_i=0$，因此尽管$e^{(A-\lambda_iE)t}$有无穷多项（矩阵函数的定义），但它作用于$v_i$后，如果$n\ge n_i$，就会变成$0$，所以无穷多项可以截断到$n_i-1$，另外利用[Jordan块法](https://blog.csdn.net/weixin_52812620/article/details/122587477)可以证明$e^{\lambda_iEt}=e^{\lambda_it}E$
    - $e^{At}v_i=e^{\lambda_it}E[I+\frac{(A-\lambda_iE)t}{factorial(1)}+\frac{(A-\lambda_iE)^2t^2}{factorial(2)}+\cdots+\frac{(A-\lambda_iE)^{n_i-1}t^{n_i-1}}{factorial{(n_i-1)}}]v_i=e^{\lambda_it}[I+\frac{(A-\lambda_iE)t}{factorial(1)}+\frac{(A-\lambda_iE)^2t^2}{factorial(2)}+\cdots+\frac{(A-\lambda_iE)^{n_i-1}t^{n_i-1}}{factorial{(n_i-1)}}]v_i$
    - $e^{At}\eta=\sum^s_{i=1}e^{At}v_i$，令$\eta=e_1=\begin{bmatrix}1\\0\\\cdots\\0\end{bmatrix}$（$v_i$是不同的），则$e^{At}e_1$可以根据公式得到$e^{At}$的第一列，以此类推可以得到$e^{At}$的所有列
    - 因此空间分解法可以直接得到$e^{At}$这个比较好的基本解矩阵

  - 待定系数法：

    - [任意矩阵都可以相似于$Jordan$矩阵，即$PAP^{-1}=J$，且对解析函数$f(x)$有$f(A)=Pf(J)P^{-1}$，其中$P$是可逆矩阵](https://blog.csdn.net/kodoshinichi/article/details/109612420)。考虑函数$f(x)=e^{tx}$，那么$f(A)=Pf(J)P^{-1}=e^{tA}$，**基解矩阵与可逆矩阵的乘积仍然是基解矩阵**，而$e^{tA}P=Pf(J)$，因此$Pf(J)=Pe^{tJ}$也是基解矩阵，**因此求解基解矩阵，可以求解$Pe^{tJ}$**
    - $f^{(n)}(x)=t^ne^{tx}$，因此对每个特征值$\lambda_i$，$f^{(n)}(\lambda_i)=t^ne^{t\lambda_i}$，[根据$Jordan$块法](https://blog.csdn.net/weixin_52812620/article/details/122587477)，若$\lambda_i$对应的重数是$n_i$，$f(J)=e^{tJ}=\begin{bmatrix}f(\lambda_i)&\frac{f^{'}(\lambda_i)}{1!}&&\cdots&\frac{f^{(n_i-1)}(\lambda_i)}{(n_i-1)!}\\&f(\lambda_i)&\frac{f^{'}(\lambda_i)}{1!}&\cdots&\\&&f(\lambda_i)&\cdots&\\\vdots&\vdots&\vdots&\ddots&\\&&&&f(\lambda_i)\end{bmatrix}=\begin{bmatrix}e^{t\lambda_i}&\frac{te^{t\lambda_i}}{1!}&&\cdots&\frac{t^{n_i-1}e^{t\lambda_i}}{(n_i-1)!}\\&e^{t\lambda_i}&\frac{te^{t\lambda_i}}{1!}&\cdots&\\&&e^{t\lambda_i}&\cdots&\\\vdots&\vdots&\vdots&\ddots&\\&&&&e^{t\lambda_i}\end{bmatrix}=e^{t\lambda_i}\begin{bmatrix}1&\frac{t}{1!}&&\cdots&\frac{t^{(n_i-1)}}{(n_i-1)!}\\&1&\frac{t}{1!}&\cdots&\\&&1&\cdots&\\\vdots&\vdots&\vdots&\ddots&\\&&&&1\end{bmatrix}$
    - $Pe^{tJ}=[P_1e^{tJ_1},P_2e^{tJ_2},P_3e^{tJ_3},\cdots,P_se^{tJ_s}]$，其中$P_i=[p_1,p_2,\cdots,p_{n_i}]$为$P$的$n\times n_i$阶子阵
    - $P_ie^{tJ_i}=e^{\lambda_it}(p_1,tp_1+p_2,\cdots,\frac{t^{n_i-1}}{(n_i-1)!}p_1+\cdots+p_{n_i})$，$P_ie^{tJ_i}$的每一列形如$x(t)=e^{\lambda_it}(c_0+\frac{t}{1!}c_1+\cdots+\frac{t^{n_i-1}}{(n_i-1)!}c_{n_i-1})$，若$x(t)=e^{\lambda_it}(c_0+\frac{t}{1!}c_1+\cdots+\frac{t^{n_i-1}}{(n_i-1)!}c_{n_i-1})$是齐次线性方程组$x'=Ax$的一个非零解，当且仅当$c_0,c_1,\cdots,c_{n_i-1}$满足$(A-\lambda_iE)^{n_i}c_0=0(c_0\ne0),c_1=(A-\lambda_iE)c_0,c_2=(A-\lambda_iE)c_1,\cdots,c_{n_i-1}=(A-\lambda_iE)c_{n_i-2}$
    - 假设$n\times n$阶矩阵$A$有互不相同的特征根$\lambda_1,\cdots,\lambda_s$，重数为$n_1,\cdots,n_s$，且$n_1+\cdots+n_s=n$，则$x'=Ax$有基本解组$\begin{matrix}e^{\lambda_1t}P^{(1)}_1(t)&\cdots&e^{\lambda_1t}P^{(1)}_{n_1}(t)\\e^{\lambda_2t}P^{(2)}_1(t)&\cdots&e^{\lambda_2t}P^{(2)}_{n_2}(t)\\\cdots&\cdots&\cdots\\e^{\lambda_st}P^{(s)}_1(t)&\cdots&e^{\lambda_st}P^{(s)}_{n_s}(t)\end{matrix}$，其中$P(t)^{(i)}_j$是待定的，$i$表示的是特征值$for(i=1;i<=s;i++)$，$j$表示的是$for(j=1;j<=n_i;j++)$，形成嵌套循环，因此$P(t)^{(i)}_j$实际上共有$n_1+n_2+\cdots+n_s=n$个，即构成了$n$个线性无关的$P(t)$，当取定一种$c_0$时，根据递推可以得到$c_1,\cdots,c_{n_i-1}$，进而可以确定一个解$x(t)$，得到的是一列$e^{\lambda_it}P^{(i)}_j(t)$
    - $c_0$的选取选择只能是$V_i$的一组基，$V_i$的维度是$n_i$，即$c_0$只有$n_i$种选择而得到$n_i$列，这$n_i$列组成$P_ie^{tJ_i}$，而基解矩阵就是$$Pe^{tJ}=[P_1e^{tJ_1},P_2e^{tJ_2},P_3e^{tJ_3},\cdots,P_se^{tJ_s}]$$

- 例题：求$x'=\begin{bmatrix}0&1&-1\\1&1&0\\1&0&1\end{bmatrix}x$的基本解组

  - 求$\begin{bmatrix}0&1&-1\\1&1&0\\1&0&1\end{bmatrix}$的特征值和特征向量可以得到一重特征值$\lambda_1=0$和二重特征值$\lambda_2=1$，即此时$n_1=1,n_2=2$
  - 利用空间分解法：
    - $(A-0\times E)^1v_1=0$，得到$v_1=\begin{bmatrix}-c\\c\\c\end{bmatrix}(c\in R)$，再解$(A-1\times E)^2v_2=0$，得到$v_2=\begin{bmatrix}a-b\\a\\b\end{bmatrix}(a,b\in R)$
    - 令$\eta=\begin{bmatrix}\eta_1\\\eta_2\\\eta_3\end{bmatrix}$，寻找$a,b,c$使得$\eta=v_1+v_2$，则$c=\eta_2-\eta_1-\eta_3,a=\eta_1+\eta_3,b=\eta_1-\eta_2+2\eta_3$
    - 因此$\eta=\begin{bmatrix}\eta_1\\\eta_2\\\eta_3\end{bmatrix}=\begin{bmatrix}\eta_1+\eta_3-\eta_2\\\eta_2-\eta_1-\eta_3\\\eta_2-\eta_1-\eta_3\end{bmatrix}+\begin{bmatrix}\eta_2-\eta_3\\\eta_1+\eta_3\\\eta_1-\eta_2+2\eta_3\end{bmatrix}$
    - $e^{At}\eta=e^{\lambda_1t}[E]v_1+e^{\lambda_2t}[E+\frac{t^1(A-\lambda_2E)}{1!}]v_2=v_1+e^{t}[\begin{bmatrix}1&0&0\\0&1&0\\0&0&1\end{bmatrix}+t\begin{bmatrix}-1&1&-1\\1&0&0\\1&0&0\end{bmatrix}]v_2=v_1+e^{t}\begin{bmatrix}1-t&t&-t\\t&1&0\\t&0&1\end{bmatrix}v_2$
    - 令$\eta=\begin{bmatrix}\eta_1\\\eta_2\\\eta_3\end{bmatrix}=\begin{bmatrix}1\\0\\0\end{bmatrix}$，$e^{At}\eta=\begin{bmatrix}1\\-1\\-1\end{bmatrix}+e^{t}\begin{bmatrix}1-t&t&-t\\t&1&0\\t&0&1\end{bmatrix}\begin{bmatrix}0\\1\\1\end{bmatrix}=\begin{bmatrix}1\\-1\\-1\end{bmatrix}+\begin{bmatrix}0+te^{t}-te^{t}\\0+e^{t}+0\\0+0+e^{t}\end{bmatrix}=\begin{bmatrix}1\\e^t-1\\e^t-1\end{bmatrix}$
    - 同理令$\eta=\begin{bmatrix}\eta_1\\\eta_2\\\eta_3\end{bmatrix}=\begin{bmatrix}0\\1\\0\end{bmatrix}$和$\eta=\begin{bmatrix}\eta_1\\\eta_2\\\eta_3\end{bmatrix}=\begin{bmatrix}0\\0\\1\end{bmatrix}$后可以得到$X(t)=e^{At}=\begin{bmatrix}1&*&*\\e^t-1&*&*\\e^t-1&*&*\end{bmatrix}$（这里偷个懒doge）
  - 利用待定系数法：
    - 它的基解矩阵必然是$\begin{bmatrix}(P_1e^{tJ_1})_{3\times 1}&(P_2e^{tJ_2})_{3\times 2}\end{bmatrix}$的形式，在$P_1e^{tJ_1}$中，解$(A-0\times E)^1c_0=0$，得到$c_{10}=\begin{bmatrix}-1\\1\\1\end{bmatrix}$，得到第一个解是$x(t)=e^{0\times t}c_{10}=\begin{bmatrix}-1\\1\\1\end{bmatrix}$，在$P_2e^{tJ_2}$中，解$(A-1\times E)^2c_0=0$，得到$c_{20}=\begin{bmatrix}1\\1\\0\end{bmatrix}$或$c_{30}=\begin{bmatrix}-1\\0\\1\end{bmatrix}$，根据递推得到$c_{21}=(A-E)c_{20}=\begin{bmatrix}0\\1\\1\end{bmatrix},c_{31}=(A-E)c_{30}=\begin{bmatrix}0\\-1\\-1\end{bmatrix}$，那么得到另两个解$x(t)=e^{t}(\begin{bmatrix}1\\1\\0\end{bmatrix}+t\begin{bmatrix}0\\1\\1\end{bmatrix})=\begin{bmatrix}e^{t}\\e^{t}(1+t)\\e^{t}t\end{bmatrix},x(t)=e^{t}(\begin{bmatrix}-1\\0\\1\end{bmatrix}+t\begin{bmatrix}0\\-1\\-1\end{bmatrix})=\begin{bmatrix}-e^{t}\\-te^{t}\\e^{t}(1-t)\end{bmatrix}$
    - 因此得到基解矩阵是$X(t)=\begin{bmatrix}-1&e^{t}&-e^{t}\\1&(1+t)e^{t}&-te^{t}\\1&te^{t}&e^{t}(1-t)\end{bmatrix}$
# 五、一般性理论

## 1.Picard存在唯一性定理

- [$Lipschitz$条件定义](https://www.doc88.com/p-4661260244432.html?r=1)，直观理解是在某个区域内函数的**某个自变量变化值**的$L$倍永远$\ge$**这个自变量变化引起的函数值的变化**

  - 例：$f(x)=sinx$，利用[拉格朗日中值定理](https://blog.csdn.net/sunbobosun56801/article/details/78105168?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164301175916780274139346%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=164301175916780274139346&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-78105168.first_rank_v2_pc_rank_v29&utm_term=%E6%8B%89%E6%A0%BC%E6%9C%97%E6%97%A5%E4%B8%AD%E5%80%BC%E5%AE%9A%E7%90%86&spm=1018.2226.3001.4187)，$\forall x_1,x_2,|sinx_1-sinx_2|=|cos\xi||x_1-x_2|\le |x_1-x_2|$

  - 满足$Lipschitz$条件是否满足很大程度上取决于函数自变量取值的区间

- [$Picard$存在唯一性定理定义及证明](https://www.doc88.com/p-4661260244432.html?r=1)，$Picard$存在唯一性定理只能保证$f(t,x)$在一个较小的区间内存在唯一解

  - 此处的存在唯一性定理与[线性方程中的存在唯一性定理](https://blog.csdn.net/weixin_52812620/article/details/122613501?spm=1001.2014.3001.5502)的不同之处
    - 定义区间：线性方程的存在唯一性定理中只对$t$有区域限制$t\in[\alpha,\beta]$是条形的；此处的定义区域是矩形的
    - 解的存在区间：线性方程的存在唯一性定理中解存在于方程组的定义区间；此处的解存在区间仅是很小的区域
  - 在证明$Picard$存在唯一性定理时构造的函数序列$\varphi_n(t)=x_0+\int^{t}_{t_0}f(\tau,\varphi_{n-1}(\tau))d\tau$与证明线性方程解存在唯一性有相似之处，但此处要注意$f(t,x)$对$t,x$都有限制，因此构造的序列只有满足$|x-x_0|\le b$即$|\varphi_n(t)-x_0|\le b$时才会有意义，而只有$|t-t_0|\le h$才能保证$|\varphi_n(t)-x_0|\le b$成立
  - $Lipschitz$条件的判断：当$f(t,x)$关于$x$有连续偏导数时，根据拉格朗日中值定理，$\forall (t,x_1),(t,x_2)\in R,|f(t,x_1)-f(t,x_2)|\le |\frac{\partial f}{\partial x}(\xi)||x_1-x_2|$，连续函数在闭区间上必有界$M$，因此必有$\forall (t,x_1),(t,x_2)\in R,|f(t,x_1)-f(t,x_2)|\le M|x_1-x_2|$，此时$M$即为$Lipschitz$常数
  - $Picard$存在唯一性定理中所保证的有解范围并不是最优的
## 2.Peano存在性定理

- 若初值问题中仅要求$f(t,x)$连续而不一定满足$Lipschitz$条件，这时解仍然存在，只是不一定唯一，这就是[$Peano$存在性定理](https://max.book118.com/html/2018/0508/165361835.shtm)
- 证明$Peano$存在性定理可以使用$Euler$折线法
  - 欧拉折线法的思想就是利用折线逼近积分曲线，将解的存在区间$[t_0-h,t_0+h]$等分为$2n$份，每步的步长是$\frac{h}{n}$，依据积分曲线的确定的方向走一步而形成一个折线段，从初始点$(t_0,x_0)$走到$t_0-h$和$t_0+h$
![请添加图片描述](https://i-blog.csdnimg.cn/blog_migrate/f177b9e6d0c7751be44da66ea0087b9f.png)

  - 积分曲线求导确定方向，即$f(t,x)$，第一步折线过$(t_0,x_0)$，斜率是$f(t_0,x_0)$，$\varphi_1(t)=x_0+f(t_0,x_0)(t-t_0),t\in[t_0,t_1]$，同理第二步折线是$\varphi_2(t)=\varphi_1(t_1)+f(t_1,\varphi_1(t_1))(t-t_1),t\in[t_1,t_2]$，则$\varphi_n(t)=\begin{cases} x_0+\sum_{k=0}^{s-1}f(t_k,x_k)(t_{k+1}-t_k)+f(t_s,x_s)(t-t_s),t\in[t_s,t_{s+1})\\x_0+\sum_{k=0}^{-s+1}f(t_k,x_k)(t_{k-1}-t_k)+f(t_{-s},x_{-s})(t-t_{-s}),t\in(t_{-s-1},t_{-s}] \end{cases}$

- 引理：[$Ascoli$引理，一致有界和等度连续](https://zhuanlan.zhihu.com/p/96005329)
  - 一致有界的函数族中每一个函数都是有界函数
  - 等度连续的函数族中每一个函数都是一致连续的，但反之却不一定

- [证明$Peano$存在性定理的大致思路是将$Ascoli$引理应用于$Euler$折线序列，证明其有一个一致收敛的子列，再证明收敛的极限函数就是微分方程的解](https://zhuanlan.zhihu.com/p/96005329)

## 3.解的延拓

- 对于同一个初值问题，当定义区域增大时，根据解的存在唯一性定理求得的解的存在区间可能会缩小，这是荒谬的：当定义区域扩大时，原来有解的区域应该至少保留，而其反而缩小
- 设$x=\varphi(t)$是[开区域](https://blog.csdn.net/AcTarjan/article/details/83831128?ops_request_misc=&request_id=&biz_id=102&utm_term=%E5%BC%80%E5%8C%BA%E5%9F%9F&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-83831128.first_rank_v2_pc_rank_v29&spm=1018.2226.3001.4187)$G$上的初值问题$x'=f(t,x),x(t_0)=x_0$的解，它定义在$[\alpha,\beta]$上，其中$f(t,x)$在$G$上连续，可构造一个以$Q:(\beta,\varphi(\beta))\in G$为中心含于$G$内的矩形区域，则方程$x=f(t,x)$有定义在某区间$[\beta-h_1,\beta+h_1]$上的解$x=\varphi_1(t)$过$Q$

![请添加图片描述](https://i-blog.csdnimg.cn/blog_migrate/aaedcde00dcc29ca1e4c94f489ab8420.png)


- [关于解的延拓的理解](https://www.zhihu.com/question/37714804)，[相关内容](https://www.doc88.com/p-748861847912.html)

## 4.微分不等式与比较定理

- 常微分方程的解很多难以求得，而实际应用中有时需要估计解的大小
- $Gronwall$不等式：设$x(t),f(t)$为区间$[t_0,t_1]$上的非负实连续函数，若有常实数$g\ge0$，使得$x(t)\le g+\int^{t}_{t_0}f(\tau)x(\tau)d\tau,t\in[t_0,t_1]$，则$$x(t)\le ge^{\int^{t}_{t_0}f(\tau)d\tau},t\in[t_0,t_1]$$，[相关证明](https://zhuanlan.zhihu.com/p/46247368)

- 推广的$Gronwall$不等式：设$x(t),g(t)$为区间$[t_0,t_1]$上的非负实连续函数，若有函数$f(t)\ge0$，在区间$[t_0,t_1]$上可积，它们满足$x(t)\le g(t)+\int^{t}_{t_0}f(\tau)x(\tau)d\tau,t\in[t_0,t_1]$，则当$t\in[t_0,t_1]$时，$x(t)\le g(t)+\int^{t}_{t_0}f(\tau)g(\tau)e^{\int^{t}_{\tau}f(s)ds}d\tau,t\in[t_0,t_1]$，[相关证明](https://wenku.baidu.com/view/e413824a64ce0508763231126edb6f1afe007136.html)
- [关于第一比较定理和第二比较定理](https://zhuanlan.zhihu.com/p/96501138)，[推广和应用](https://max.book118.com/html/2017/1219/144922910.shtm)

## 5.解对初值和参数的依赖性

-  [常微分方程对初值和参数的连续依赖性](https://www.docin.com/p-1910784587.html)，[具体例子](https://www.zhihu.com/question/427445015)

## 6.微分方程的数值解

- 初值问题$x'=f(t,x),x(t_0)=x_0$的数值解指解$x(t)$在一些离散节点$t_1<t_2<\cdots<t_n<\cdots$处的近似值$x_1,x_2,\cdots,x_n,\cdots$，两个相邻节点的距离$h_k=x_{k+1}-x_k$称为步长
- 求初值问题的数值解，首先要将其离散化，然后导出计算$x_{m+1}$的递推公式
- 若某种数值方法在计算$x_{m+1}$时只用到了前面相邻一个节点处的近似值$x_m$，称为单步法，如果用到了前面$k$个节点处的近似值，称为$k$步法
- 显式单步法的一般形式是$x_{n+1}=x_n+h\phi(t_n,x_n,h)$，设$x(t)$是初值问题的精确解，则称$T_{n+1}=x(t_{n+1})-x(t_n)-h\phi(t_n,x(t_n),h)$为估计方法的局部截断误差，若存在最大整数$p$使得局部截断误差满足$T_{n+1}=\phi(t_n,x(t_n))h^{p+1}+O(h^{p+2})$则称估计方具有$p$阶精度，$\phi(t_n,x(t_n))h^{p+1}$称为局部截断误差的主项
- 欧拉公式：已知初始值$x_0$，$x_{n+1}=x_{n}+hf(t_{n},x_{n})$
  - 利用欧拉折线逼近，等步长（$h$）单步法，当方程满足$Picard$存在唯一性定理时，欧拉折线法可以收敛到精确解
  - 计算简单，但累积误差严重，$t_n$变大时精度很差，一阶精度
  - 步长缩小时，在一定范围内能够提高精度，但达到一定值时再减小，误差可能反而增大
- 梯形方法：已知初始值$x_0$，$x_{n+1}=x_{n}+\frac{h}{2}[f(t_{n},x_{n})+f(t_{n+1},x_{n+1})],n=0,2,\cdots$
  - 给定的计算方法实际是一个$x_{n+1}$的方程，可以采用[二分法或牛顿迭代法求解](https://blog.csdn.net/hanmingjunv5/article/details/106379274?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164307736016780255228078%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=164307736016780255228078&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-2-106379274.first_rank_v2_pc_rank_v29&utm_term=%E4%BA%8C%E5%88%86%E6%B3%95%E5%92%8C%E7%89%9B%E9%A1%BF%E8%BF%AD%E4%BB%A3%E6%B3%95&spm=1018.2226.3001.4187)，迭代初值可以使用欧拉公式求得
  - 计算较复杂，精度高于欧拉公式，二阶精度
- 改进欧拉公式：它是一个预测-校正系统：$\begin{cases}x_p=x_n+hf(t_n,x_n)\\x_c=x_n+hf(t_{n+1},x_p)\\x_{n+1}=\frac{1}{2}[x_p+x_c]\end{cases}$，$x_p$称为预测值，$x_c$称为校正值
  - 与梯形方法精度类似，计算量比梯形方法小得多，二阶精度

- $q$阶显式$Runge-Kutta$方法：$\begin{cases}x_{n+1}=x_n+h\sum_{i=1}^qw_iK_i\\K_1=f(t_n,x_n)\\K_i=f(t_n+\lambda_ih,x_n+h\sum_{j=1}^{i-1}\mu_jK_j)(i=2,\cdots,q)\end{cases}$，其中$\mu_j$为常数，该方法简称$R-K$方法
  - 当$q=1$时，即为欧拉公式；改进欧拉公式是$q=2$时的一种显式$Runge-Kutta$方法
  - 要求初值问题的解具有较好的光滑性



