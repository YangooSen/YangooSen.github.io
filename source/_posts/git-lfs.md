---
title: git_lfs
katex: true
date: 2024-04-27 21:04:35
excerpt: git和git lfs的操作
tag: linux
---

> linux 压缩文件
> ```bash
> zip -r data.zip data/
> ```

起因是需要push比较大的文件，因此找到了[git lfs(Large File Storage)](https://blog.csdn.net/wzk4869/article/details/131661472)这个工具



# lfs
- install
```bash
curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | sudo bash
sudo apt-get install git-lfs

```
- push
```bash
cd foo
git init
git lfs install
git lfs track ".txt" #跟踪大文件
git remote add origin xxx
git add .
git branch -m main
git push origin main:main

```
track大文件之后,之后正常push文件即可

- clone
```bash
#正常clone lfs文件并不是源文件
git lfs clone xxx.git

```
