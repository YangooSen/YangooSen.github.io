---
title: tensor
katex: true
date: 2024-04-03 14:54:50
excerpt: pytorch中的tensor基础操作和概念
tag: pytorch
---

[来自B站视频](https://www.bilibili.com/video/BV1ov411M7xL/?spm_id_from=333.999.0.0&vd_source=6c26f427606a59575440e9bc6cec44af)，[官网教程](https://pytorch.org/tutorials/beginner/basics/tensorqs_tutorial.html)，[API查阅](https://pytorch.org/docs/stable/torch.html)

# 创建 tensor
- **torch.tensor**
- torch.ones_like
- torch.zeros_like
- [tensor.view](https://pytorch.org/docs/stable/tensor_view.html#tensor-views)
> Pytorch 通过 view 机制可以实现 tensor 之间的**内存共享**，避免显式的数据拷贝，因此能实现快速且内存高效的比如切片和 element-wise 等操作，使用view op之后可能产生[不连续内存的tensor](https://zhuanlan.zhihu.com/p/463664495)
- torch.rand_like
- **[torch.allclose](https://pytorch.org/docs/stable/generated/torch.allclose.html#torch-allclose)** 对比两个 tensor 是否足够接近
- torch.rand
> Returns a tensor filled with random numbers from a **uniform distribution** on the interval [0, 1)[0,1)
- [torch.Tensor和torch.tensor有什么区别？](https://blog.csdn.net/qimo601/article/details/109691368)，前者是转化成torch.FloatTensor，后者是根据传入的数据转化成不同类型的dtype
- torch.randn
> 	Returns a tensor filled with random numbers from a **normal distribution** with mean 0 and variance 1 (also called the standard normal distribution).
- **torch.normal(mean,std,size)**
- **torch.zeros**
- **torch.arange**
>torch.range() is deprecated and will be removed in a future release because its behavior is inconsistent with Python’s range builtin. **Instead, use torch.arange(), which produces values in [start, end)**.
- torch.eye
- **torch.full([n,n],x)**
- 移动 tensor
```python
if torch.cuda.is_available():
	tensor=tensor.to('cuda')
```
# tensor 运算
- [pytorch中的矩阵乘法：函数mul,mm,mv以及 @运算 和 *运算](https://blog.csdn.net/beauthy/article/details/121103704)
- torch.is_tensor
- **torch.numel**，返回元素数目
- [torch.bmm](https://pytorch.org/docs/stable/generated/torch.bmm.html#torch.bmm)，批量张量相乘

# tensor 操作
- torch.cat
 > 除了**dim**指定的那个维度，其他维度的shape都应该一样
- torch.reshape(a,shape)
> shape可以用 [] 或 () 指定
- torch.split(a,size)
> 按size大小均分或者可以传入list
- **torch.squeeze**
- [troch.tile(a,dims)](https://pytorch.org/docs/stable/generated/torch.tile.html#torch.tile)
> 扩增a的行列
- torch.transpose(a,dim0,dim1)
- torch.unbind
- **torch.unsqueeze(a,dim)**
- [torch.nn.functional.pad](https://pytorch.org/docs/stable/generated/torch.nn.functional.pad.html?highlight=pad#torch.nn.functional.pad) 
> [torch.nn.functional.pad使用](https://zhuanlan.zhihu.com/p/358599463)
> 对 (batch_size,in_channel,h,w)，pad的顺序是(h1,h2,w1,w2,in_channel1,in_channel2,batch_size1,batch_size2)，因为每一个维度都有两个方向
# tensor 属性
[TENSOR ATTRIBUTES](https://pytorch.org/docs/stable/tensor_attributes.html)

