---
title: server_methods
katex: true
date: 2024-05-15 15:37:24
excerpt: 与服务器交互的一些命令
tag: linux
---

# sftp

不要用scp，似乎有很多bug且麻烦，[Linux 命令详解：SFTP](https://zhuanlan.zhihu.com/p/51749905)
```bash
#连接服务器
sftp -P remote_port remote_user@remote_host

#下载服务器文件
get /path/remote_file

#上传服务器文件
put local_file

#查看服务器目录内容
ls

#查看本地目录内容
lls

```
