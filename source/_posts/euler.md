---
title: euler
katex: true
date: 2024-05-11 10:37:44
excerpt: 对欧拉公式的理解
tag: math
---

二维平面上，对于任何向量$\begin{bmatrix}x\\y\end{bmatrix}$，令$x=rcos\theta,y=rsin\theta$。将任意向量旋转$\alpha$的线性变换是
$$
f:\begin{bmatrix}rcos\theta\\rsin\theta\end{bmatrix}\rightarrow\begin{bmatrix}rcos(\theta+\alpha)\\rsin(\theta+\alpha)\end{bmatrix}\
$$
利用三角公式展开即为
$$
\begin{bmatrix}rcos(\theta+\alpha)\\rsin(\theta+\alpha)\end{bmatrix}=

\begin{bmatrix}rcos\theta cos\alpha-rsin\theta sin\alpha\\rcos\theta sin\alpha+rcos\alpha sin\theta\end{bmatrix}\
$$

此时变换回$x,y$
$$
f:\begin{bmatrix}x\\y\end{bmatrix}\rightarrow\begin{bmatrix}x cos\alpha-y sin\alpha\\x sin\alpha+ycos\alpha\end{bmatrix}\
$$
不难看出将向量旋转$\alpha$的线性变换对应的矩阵是
$$
f:\begin{bmatrix}cos\alpha &-sin\alpha\\sin\alpha & cos\alpha\end{bmatrix}\begin{bmatrix}x\\y\end{bmatrix}=\begin{bmatrix}x cos\alpha-y sin\alpha\\x sin\alpha+ycos\alpha\end{bmatrix}\
$$

考虑二维平面为复平面，任意复数$x+yi$可以表示为向量$\begin{bmatrix}x\\y\end{bmatrix}$，[也可以视为矩阵](https://zhuanlan.zhihu.com/p/85321120)$\begin{bmatrix}x&-y\\y&x\end{bmatrix}$，当复数模为1时即可可以进一步视为旋转矩阵
