---
title: how
katex: true
excerpt: 利用hexo的写作流程，情况有变保持更新
date: 2024-04-02 15:30:47
tag: note
---


# 概述
- 依靠页面生成和部署项目[hexo](https://github.com/hexojs/hexo)和hexo主题[Fluid](https://github.com/fluid-dev/hexo-theme-fluid)进行写作
- hexo配置可用看：[使用 Hexo+GitHub 搭建个人免费博客教程（小白向）](https://zhuanlan.zhihu.com/p/60578464)，[一次完整的Hexo写作流程](https://fuguigui.github.io/hexo2/)
- fluid配置可以看：[配置指南](https://hexo.fluid-dev.com/docs/guide/)
# 快捷键和别名
```sh
hexo new xxx
```
> 创建新的名为xxx.md的文章，位于source/_posts目录下，由于配置了_config.yml，会同时创建一个同名的文件夹来放置引用资源，比如图片等

```sh
cbp
```
> bashrc中创建的别名，代表的命令是`cd source/_posts`，即文章创建的地方，之后vim修改文章，编辑摘要excerpt
```sh
cdf2
```
> bashrc中创建的别名，代表的命令是`cd .. && cd ..`，即退回2层父目录。当位于`source/_posts0`下时，调用`cdf2`即可返回初始文件夹进行git操作

```sh
hexocgs
```
> bashrc中创建的别名，代表的命令是`hexo clean && hexo g && hexo s`，即清理原有文件，生成页面，启动服务器预览页面，此时可用预览部署后的结果

```sh
hexo d
```
> 将生成的页面部署到 github，缓存一段时间后即可看到部署结果


# 云存档

由于github存储的是hexo生成的页面结果，而不是原markdown文档，因此难以在原先分支上直接存储写了一部分的文章，因此在[仓库](https://github.com/YangooSen/YangooSen.github.io/tree/writing)中新开了分支 writing，在本地文件夹创建分支writing，跟踪 source 文件夹下的原markdown文章和引用资源，用下面的命令进行拉取和推送
```sh
# ... 修改source文件夹下的内容
git add source
git commit
git push writing writing:writing #将写了一半的文章云存储在writing分支


rm -rf source # 删除本地文章腾出空间
git add source
git commit
git pull -f writing writing:writing #force拉取云存档内容,之后进入source继续写作
```
> 当写了足够多的文章后，确定已经写好的某些文章不会再修改，即可在仓库中新开分支备份，将这些不会再修改的文章存好，writing分支只放还在继续更新的文章

# 关于写作
## 数学公式
- 在文章中展现数学公式依靠[hexo-math](https://github.com/hexojs/hexo-math)，在文章中配置`katex:true`，已经在post.md模板中写好，下面是测试公式
$$
\varphi_i^{(l+1)} = \sigma(\sum_{r\in\mathcal{R}}
\sum_{j\in\mathcal{N}^r(i)}e_{j,i}W_r^{(l)}\varphi_j^{(l)}+W_0^{(l)}\varphi_i^{(l)})
$$

## 引用图片
- 引用格式是 `![图片标题](图片名称)`，注意图片名称不需要加图片路径，即**不需要**`.\xxx\图片名称`，像下面例子这样
![钢铁侠](broken.jpg)

## 引用站内文章
- 引用站内文章和引用站外文章不同，关于引用站内文章的注意点可用看[这篇文章]((https://fuguigui.github.io/hexo2/))，对于本博客而言，引用路径一般是`https://YangooSen.github.io/yyyy/mm/dd/title/`，其中代表指的是时间，title指的是文章标题，因为本博客渲染部署后，文章即位于这个路径



















