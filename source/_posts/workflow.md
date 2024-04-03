---
title: workflow
katex: true
date: 2024-04-03 17:23:17
excerpt: 构建模型的流程，nn.Module，state_dict，parameters，modules
tag: pytorch
---

[来自B站视频](https://www.bilibili.com/video/BV1ov411M7xL/?spm_id_from=333.999.0.0&vd_source=6c26f427606a59575440e9bc6cec44af)，[官网教程](https://pytorch.org/tutorials/beginner/basics/buildmodel_tutorial.html)，[API查阅](https://pytorch.org/docs/stable/torch.html)，[详细信息](https://pytorch.org/tutorials/recipes/recipes_index.html)
- Dataset 的\__init__中定义 transform 一般通过 \_\_getitem__来调用
- [BUILD THE NEURAL NETWORK](https://pytorch.org/tutorials/beginner/basics/buildmodel_tutorial.html) 中是 pytorch 构建模型的简单流程，[PYTORCH RECIPES](https://pytorch.org/tutorials/recipes/recipes_index.html) 是更相关详细内容，训练是要保存 checkpoint，包括 model_state_dict 和 optimizer_state_dict 等
- 类似 tf 中 summary 模型（查看参数数目和分布）的方法在官方 pytorch 中没有，可以通过[torchsummaryX](https://github.com/nmhkahn/torchsummaryX) 来实现
- 简单看模型的参数数目可以
```python
sum(p.numel() for p in model.parameters())
```
- [module](https://pytorch.org/docs/stable/generated/torch.nn.Module.html#torch.nn.Module) 和 API 介绍
- pytorch 的 buffers 和 tf 中的（槽变量？）类似
- 关于 buffer
```python
[docs]    def register_buffer(self, name: str, tensor: Optional[Tensor], persistent: bool = True) -> None:
        r"""Adds a buffer to the module.

        This is typically used to register a buffer that should not to be
        considered a model parameter. For example, BatchNorm's ``running_mean``
        is not a parameter, but is part of the module's state. Buffers, by
        default, are persistent and will be saved alongside parameters. This
        behavior can be changed by setting :attr:`persistent` to ``False``. The
        only difference between a persistent buffer and a non-persistent buffer
        is that the latter will not be a part of this module's
        :attr:`state_dict`.

        Buffers can be accessed as attributes using given names.
```
- [module源码](https://pytorch.org/docs/stable/_modules/torch/nn/modules/module.html#Module) 介绍
- [parameter](https://pytorch.org/docs/stable/generated/torch.nn.parameter.Parameter.html?highlight=parameters)是tensor的一个子类，作为模型参数必须设为 parameter 类型，通过register_parameter 注册
- 改变数据类型 model.to(torch.float32)
- 类的基本属性有 model._module，model._parameters，model._buffers，**返回当前 module 的内容，不会返回子module的内容**，不是为了公有访问设计的
- 类的基本方法有 model.parameters() 或 model.named_parameters()，返回的是迭代器，既包含当前 module 的内容，也包括子 module 的内容
- model.train()和 model.eval() 改变 self.training，在 dropout 和 batchnorm 中会不同
- 保存model
```python
# Specify a path
PATH = "state_dict_model.pt"

# Save
torch.save(net.state_dict(), PATH)

# Load
model = Net()
model.load_state_dict(torch.load(PATH))
model.eval()
```
- checkpoint 是训练中断后继续训练使用的，state_dict 是模型的参数，但是不包括架构，如果要保存架构，可以使用下面的示例。一般推荐按照
```python
# Specify a path
PATH = "entire_model.pt"

# Save
torch.save(net, PATH)

# Load
model = torch.load(PATH)
model.eval()
```
- 尽量使用 docker 创建训练环境，环境如下图所示
![环境](environment.png)
