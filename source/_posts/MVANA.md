---
title: MVANA
katex: true
date: 2024-12-02 15:11:01
excerpt: MVANA 模型代码解析
tag: code
---

# main
```python
dataset = get_dataset(ds_name, config["data_dir"])

acc, std, duration_mean = train_eval.cross_validation(ds_name,"MVANA",dataset,seed,MVANA,device,config,time_str) 


```
