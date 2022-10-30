---
layout: page
title: 服务器搭建指南
tags: Tutorials
permalink: /Tutorials/ServerBuildManual.html
---

_各种服务器的搭建教程_

---
---

# 准备SteamCMD

[Link](https://developer.valvesoftware.com/wiki/SteamCMD)

---

## 直接从仓库安装SteamCMD

__Ubuntu/Debian:__

```shell
sudo add-apt-repository multiverse
sudo apt install software-properties-common
sudo dpkg --add-architecture i386
sudo apt update
# 64-bit机器需要执行以上代码, 官网这么说的, 我没试过
sudo apt install lib32gcc-s1 steamcmd screen
```

__RedHat/CentOS:__

```shell
sudo yum install steamcmd screen
```

---

## 手动安装

* 腾讯云的CentOS机器默认情况下搜索不到SteamCMD, 很神秘。所以加入手动安装方法: 

__Ubuntu/Debian:__

```shell
sudo apt-get install lib32gcc-s1 screen
mkdir ~/Steam && cd ~/Steam
curl -sqL "https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz" | tar zxvf -
```
__RedHat/CentOS:__

```shell
yum install glibc.i686 libstdc++.i686 screen
mkdir ~/Steam && cd ~/Steam
curl -sqL "https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz" | tar zxvf -
```

---

## 使用

```shell
screen -S SteamCMD
cd ~/Steam
./steamcmd.sh
# 等待进入SteamCMD

force_install_dir <path> # 设置安装路径，一般没什么好改的
login anonymous # 登录, 一般匿名登录就好
app_update <app_id> # app_id就是服务器的id啦
```
---
---

# Don't Starve together
__app_id: 343050__
[Link](https://dontstarve.fandom.com/wiki/Guides/Don%E2%80%99t_Starve_Together_Dedicated_Servers)
* 有个依赖, 似乎要自己构建

```shell
./dontstarve_dedicated_server_nullrenderer_x64: error while loading shared libraries: libcurl-gnutls.so.4: cannot open shared object file: No such file or directory
# 教程就此中断吧，先不写了，摸了
```

* [新建服务器](https://accounts.klei.com/account/game/servers?game=DontStarveTogether)

* 可以直接Configure Server 

```
Server Token = <token> # 不用管
Maximum Players = <int> # 最大人数
Server Playstyle = <string> # 世界类型: Servival(生存), Relaxed(休闲), Endless(无尽), Wilderless(荒野), Lights out(永夜)
Cluster Name = <string> # 服务器名称
Cluster Description = <string> # 服务器简介
Cluster Password = <string> # 密码
```

* 下载得到一个MyDediServer.zip, 直接scp文件到服务器吧
