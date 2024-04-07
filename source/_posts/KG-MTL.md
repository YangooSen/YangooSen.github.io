---
title: KG-MTL
katex: true
excerpt: KG-MTL代码解析
date: 2024-04-02 17:16:32
tag: code
---
知识图谱和多任务学习预测DTI和CPI，代码：[KG-MTL](https://github.com/xzenglab/KG-MTL) （源代码的基础上下载一下项目中提到的 DRKG 数据集，加几个文件夹，微调一下细节可用跑起来），原文：[KG-MTL: Knowledge Graph Enhanced Multi-Task Learning for Molecular Interaction](https://ieeexplore.ieee.org/document/9815157)，框架如下：
![模型框架](kg-mtl.png)
- dti和cpi的区别是什么? 
运行参数是
python main.py
--loss_mode weighted 
--variant KG-MTL-C 
--gpu 1 
--cpi_dataset human 
--dti_dataset drugcentral

# if __ main__

- 参数中variant是指共享模块中保留哪些部分,可以看原文解释
- 

# main
- drugs 和 drugid 都是 drugid的列表,为什么要设置两个? 
- process_kg做采样子图和转化成torch.tensor，转移到cuda


# load_data
- load_diata返回的是包含数据的类,在init中调用读取函数  
- 读取的是drugcentral_dti_examples.tsv,里面的数据似乎只有label是1的正例
- drug2smiles 是药物id到定长smiles的映射,sample_nodes是药物和靶点的id集合
- smiles_graph[drug_id]中,c_size是分子的原子数量,edge_index是双向边的集合,features是atom_features得到
- dti数据集中得到的sample_nodes最后也添加了cpi数据集的drugid

# model
- MultiTaskLoss只是套了一层计算损失函数的forward,实际调用的模型是MKDTI
- rgcn_layers中的[RelGraphConv](https://blog.csdn.net/weixin_52812620/article/details/137139828?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22137139828%22%2C%22source%22%3A%22weixin_52812620%22%7D)
- [Cross_stitch](https://zhuanlan.zhihu.com/p/37449901)自动决定共享层,其中共享参数都是单个数值?而不是矩阵?
![实例](cross_stitch.webp) 
- self.cpi_hidden_dim 设置的是[78,drug_hidden_dim,drug_hidden_dim]


