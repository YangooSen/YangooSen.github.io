---
title: Dataset_DataLoader
katex: true
date: 2024-04-03 17:20:32
excerpt: 自定义Dataset和DataLoader的参数和源码
tag: pytorch
---

[来自B站视频](https://www.bilibili.com/video/BV1ov411M7xL/?spm_id_from=333.999.0.0&vd_source=6c26f427606a59575440e9bc6cec44af)，[官网教程](https://pytorch.org/tutorials/beginner/basics/data_tutorial.html)，[API查阅](https://pytorch.org/docs/stable/torch.html)
- A custom Dataset class must implement three functions: \_\_init\__, \_\_len__, and \_\_getitem__.
```python
import os
import pandas as pd
from torchvision.io import read_image

class CustomImageDataset(Dataset):
    def __init__(self, annotations_file, img_dir, transform=None, target_transform=None):
        self.img_labels = pd.read_csv(annotations_file)
        self.img_dir = img_dir
        self.transform = transform
        self.target_transform = target_transform

    def __len__(self):
        return len(self.img_labels)

    def __getitem__(self, idx):
        img_path = os.path.join(self.img_dir, self.img_labels.iloc[idx, 0])
        image = read_image(img_path)
        label = self.img_labels.iloc[idx, 1]
        if self.transform:
            image = self.transform(image)
        if self.target_transform:
            label = self.target_transform(label)
        return image, label
```
- The Dataset retrieves our dataset’s features and labels **one sample at a time**. While training a model, we typically want to **pass samples in “minibatches”**, reshuffle the data at every epoch to reduce model overfitting, and use Python’s multiprocessing to speed up data retrieval.

[DataLoader](https://pytorch.org/docs/stable/data.html#torch.utils.data.DataLoader) 介绍，[源码](https://pytorch.org/docs/stable/_modules/torch/utils/data/dataloader.html#DataLoader):
- collate_fn 是针对 minibatches 的操作，Dataset 的 transform 是针对单个样本的处理
- collate_fn 的参数是 batch，即 batch_size 个  \_\_getitem__ 返回的 item
- 一般的 Dataset 类型都是[map-style datasets](https://pytorch.org/docs/stable/data.html#map-style-datasets)，如果是 iterable-style 的话，迭代完之后就会变成空的
> **sampler** (Sampler or Iterable, optional): defines the strategy to draw
            samples from the dataset. Can be any ``Iterable`` with ``__len__``
            implemented. **If specified, :attr:`shuffle` must not be specified.**

>**batch_sampler** (Sampler or Iterable, optional): like :attr:`sampler`, but
            returns a batch of indices at a time. **Mutually exclusive with
            :attr:`batch_size`, :attr:`shuffle`, :attr:`sampler`,
            and :attr:`drop_last`.**
- 不设置 sampler 参数，会有默认的 sampler 处理
```python
 if sampler is None:  # give default samplers
     if self._dataset_kind == _DatasetKind.Iterable:
         # See NOTE [ Custom Samplers and IterableDataset ]
         sampler = _InfiniteConstantSampler()
     else:  # map-style
         if shuffle:
             sampler = RandomSampler(dataset, generator=generator)  # type: ignore[arg-type]
         else:
             sampler = SequentialSampler(dataset)  # type: ignore[arg-type]
  ```
  - [RandomSampler](https://pytorch.org/docs/stable/_modules/torch/utils/data/sampler.html#RandomSampler) 中 [randperm](https://pytorch.org/docs/stable/generated/torch.randperm.html?highlight=randperm#torch.randperm) 返回随机排序，[yield from](https://blog.csdn.net/weixin_36338224/article/details/109234922?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522168604315416782425149280%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=168604315416782425149280&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-109234922-null-null.142%5Ev88%5Ekoosearch_v1,239%5Ev2%5Einsert_chatgpt&utm_term=python%20yield%20from&spm=1018.2226.3001.4187) 介绍
```python
yield from torch.randperm(n, generator=generator).tolist()[:self.num_samples % n]
```
[SequentialSampler](https://pytorch.org/docs/stable/_modules/torch/utils/data/sampler.html#RandomSampler) 原序返回
```python
 return iter(range(len(self.data_source)))
```
- 不设置 batch_sampler 参数，会有默认的 [batch_sampler](https://pytorch.org/docs/stable/_modules/torch/utils/data/sampler.html#BatchSampler) 处理，它根据 sampler 采样组成一个 batch 后返回
```python
if batch_size is not None and batch_sampler is None:
    # auto_collation without custom batch_sampler
    batch_sampler = BatchSampler(sampler, batch_size, drop_last)
```
- 不设置 collate_fn 参数，一般也没有 batch_sampler 的情况下，调用默认的 [default_collate](https://pytorch.org/docs/stable/data.html?highlight=default_collate#torch.utils.data.default_collate)，以 batch 为参数，基本没有做任何事
```python
@property
def _auto_collation(self):
    return self.batch_sampler is not None

if collate_fn is None:
    if self._auto_collation:
        collate_fn = _utils.collate.default_collate
    else:
        collate_fn = _utils.collate.default_convert
```
- 视频里讲了_index_sampler 和 _get_iterator 的相关内容
