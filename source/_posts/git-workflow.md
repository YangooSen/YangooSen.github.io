---
title: git_workflow
katex: true
date: 2024-05-15 22:02:49
excerpt: 详细解释多人git操作流程
tag: linux
---

下面介绍两人(甲和乙)同时git一个仓库的情况。需要的是每个本地有两个分支，远程github有三个分支（多人就有多个分支）。本地的两个分支在远程分别有对应，一个对应本人的分支，一个对应整个项目的主分支main


# 创建仓库

项目初始创建github仓库，其中某一个人叫甲的先push
```bash
git init
git remote add origin xxx.git

git add xx
git commit
```
# 命名分支

创建本地的分支，提交本地结果和主分支结果

```bash
git branch -m jia #似乎只有commit之后才能命名
git push origin jia:main
git push origin jia:jia
```
此时远程github上有两个分支

# clone仓库

另一个人乙也参与进项目开发

```bash
git clone xxx.git
cd xxx
```
此时乙的本地只有main分支

# 创建分支
```bash
git branch yi #创建分支yi
git add xx
git commit #在yi分支提交修改

git push origin yi:yi
```
创建本地的分支，保证这个本地有两个分支，push yi:yi 之后远程github有三个分支,yi分支是yi的东西，jia分支是甲的东西，main分支也是甲的东西，乙还没有把自己的提交到main

# 合并分支

```bash
git checkout main #乙切换到main分支
git merge yi #乙将自己的分支与git clone的甲的分支合并到main上
git push origin main:main
```
合并之后main分支是最新的，提交到github的main分支

```bash
git checkout yi
git pull origin main:yi
git push origin yi:yi
```
由于github上的main已经是最新，保证自己本地的也是最新可用pull一下


# 同步分支
甲如果要继续修改，先pull最新的分支,因为甲本地还只有jia分支

```bash
git pull origin main:main
git checkout main
git merge jia

git push origin jia:jia
```
这样甲也是最新的分支了


# 之后流程
```bash
git checkout jia #先在自己分支上进行操作
git add xx
git commit
git push origin jia:jia #推送到远程自己对应的分支
git checkout main
git pull origin main:main #将远程最新main拉回本地，保证本地的main是最新的
git merge jia #将此次自己的修改同步到main分支上
git push origin main:main #将自己的修改推送到远程主分支

git checkout jia
git pull origin main:jia #保证自己本地分支也是最新的
git push origin jia:jia  #保证远程对应分支也是最新，这个可做可不做，因为下一次流程也是先push自己的分支
```
最重要的是记住
- 每次都是**先修改和push自己对应的分支**
- 把**最新的main同步到本地后再**merge和push










