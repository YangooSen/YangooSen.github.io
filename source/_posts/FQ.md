---
title: FQ
katex: true
date: 2024-04-26 10:49:27
excerpt: 记录科学上网的方式
tag: note
---

事件的起因是要在服务器跑数据，而下载数据又需要FQ，因为服务器只有命令行，就导致了一些问题，下面是最后成功的步骤

# clash
[clash](https://clash.wiki/)是FQ的代理工具，还需要节点，我用的是师兄推荐的[ikuuu](https://ikuuu.org/user)

## step

- 下载合适的clash
```bash
mkdir ~/clash
cd ~/clash

#需要下载符合系统的clash，这里选的是64位linux
wget https://github.com/DustinWin/clash_singbox-tools/raw/main/ClashPremium-release/clashpremium-linux-amd64

mv clashpremium-linux-amd64 clash
```

- 下载节点服务的配置文件(参考选用的节点)
```bash
wget -O config.yaml "https://v2ois.no-mad-world.club/link/IoxK4lTJ3gDDQvyV?clash=3"
```
这个配置文件可以自己修改，参考[注释](https://clash.wiki/configuration/configuration-reference.html)，端口占用的时候修改`port`和`socks-port`,`external-controller`是外部监听，在另一台机器上监听FQ机器的状况，选择节点，查看流量等。这个我自己配了很久没搞好，实际用处不大感觉，可以看[在 Linux 中使用 Clash](https://zhuanlan.zhihu.com/p/692274448)，下面是服务器端的配置例子
```
port: 15732
#port: 7890
socks-port: 15731
#socks-port: 7891
allow-lan: false
mode: Rule 
log-level: info 
external-controller: 0.0.0.0:9990
#external-controller: 127.0.0.1:9090
external-ui: ./dashboard
#secret: "123456"
...

```
- 启动clash
```bash
chmod +x clash
./clash -d .

```
`- d`是指定配置文件yaml的路径，这里就是当前文件夹

- 配置代理端口
```bash
export http_proxy=http://127.0.0.1:15732
export https_proxy=http://127.0.0.1:15732
```
这里因为变量大小写的问题搞了很久，注意要用小写

至此就完成了命令行的代理，但注意**并不是**系统代理，比如浏览器访问外网网页还是不会走代理

## ALL proxy

为了在浏览器访问也走梯子，需要配置系统代理（或者浏览器代理）。linux具体操作是在设置中修改Network，选择手动后修改地址，可以看[步骤](https://ikuuu.org/user/tutorial?os=linux&client=clash)的第五步，HTTP_PROXY,HTTPS_PROXY,SOCKS_HOST都需要修改

而通过命令行的方式修改系统代理暂时没找到方式，因此服务器只有命令行的情况可能会麻烦一点


## server

服务器端用clash，注意要先等它转到好节点之后才能用，实测wget总是使用挂掉的节点，而不会转到好节点，可以先用
```bash
curl google.com -v
curl www.youtube.com -v
```
返回301或者302证明节点已经选好了，服务器端由于没有什么方便的手动选择节点方式
可以curl几次，如果还没有选好节点，就重新启动一下clash，还不行就重新wget一下配置yaml配置文件，注意wget的地址可能会随着时间改变，可能是官方在处理挂掉的节点

# PigCha

官方有[教程](http://120.241.39.52:8888/misc/linux_tutorial)，这个本地和服务器端比较方便，一步到位但月费也比ikuuu贵

