---
title: paperGraph
katex: true
date: 2024-06-07 15:17:25
excerpt: 科研文章作图
tag: paper
---

# color

[Nature系列配色](https://mp.weixin.qq.com/s/xIdTicrP7AfO2QbdkwGOrQ)

```python
import matplotlib.pylab as plt


def plotColor(colors):
    size=len(colors)
    fig = plt.figure(figsize=(3, 3*size))

    for i in range(1,size+1):
        ax=fig.add_subplot(1,size,i)
        ax.bar(x=1,height=1,color=colors[i-1])


    plt.show()



plotColor([
    "#699ECA",
    "#FF8C00",
    "#F898CB",
    "#4DAF4A"
    ])


```

# graph

## heatmap

```python

from matplotlib import pyplot as plt
import seaborn as sns
import numpy as np
import pandas as pd
from matplotlib.colors import LinearSegmentedColormap

#从白到绿的渐变色
my_colormap = LinearSegmentedColormap.from_list("", ["white", "green"])

#每个方格的 label 设置 index 和 columns
data = pd.DataFrame(np.arange(25).reshape(5, 5), index=['A', 'B', 'C','D','E'], columns=['1', '2', '3','4','5'])

#绘制热图
cmap = sns.heatmap(data, linewidths=0.8, annot=True, fmt="d", cmap=my_colormap)

plt.xlabel("model", size=15)
plt.ylabel("descripotr", size=15)
plt.title("R of regression", size=15)
plt.tight_layout()
plt.show()


```
