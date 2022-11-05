---
layout: page
title: SteamCMD 部署
permalink: /Tutorials/steamGameServerHostManual/Steamcmd/
parent: ServerBuildManual
grand_parent: 
---

# 准备SteamCMD

{: .warning }
> Warning: Do not run steamcmd while operating as the root user. Doing so is a security risk.
> 
> 警告: 登录为 root 用户身份时不要运行 steamcmd. 这样做会带来安全风险。

[官方文档[Link]](https://developer.valvesoftware.com/wiki/SteamCMD)

---

## 直接从仓库安装SteamCMD

__Ubuntu/Debian:__

```shell
sudo apt install software-properties-common
# 安装得到 add-apt-repository 这个指令
sudo add-apt-repository multiverse
sudo apt install software-properties-common
sudo dpkg --add-architecture i386
sudo apt update
# 64-bit 机器需要执行以上代码
sudo apt install lib32gcc-s1 steamcmd
```

__RedHat/CentOS:__

```shell
sudo yum install steamcmd
```

---

## 手动安装

腾讯云的 CentOS 机器默认情况下搜索不到 SteamCMD , 很神秘。所以加入手动安装方法: 

__Ubuntu/Debian:__

```shell
sudo apt-get install lib32gcc-s1
mkdir ~/Steam && cd ~/Steam
curl -sqL "https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz" | tar zxvf -
```
__RedHat/CentOS:__

```shell
sudo yum install glibc.i686 libstdc++.i686
mkdir ~/Steam && cd ~/Steam
curl -sqL "https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz" | tar zxvf -
```

---

## 使用

包管理器安装 SteamCMD 后的使用方法: 

```shell
cd ~
steamcmd # 默认会在当前路径生成 Steam 文件夹, 下载的各种东西就在里面
```

手动安装 SteamCMD 后的使用方法: 

```shell
screen -S SteamCMD
cd ~/Steam
./steamcmd.sh
# 等待进入 SteamCMD
```

进入 SteamCMD 后的操作: 

```shell
force_install_dir <path> # 设置安装路径，一般没什么好改的, 看自身需求执行
login anonymous # 登录, 一般匿名登录就好
app_update <app_id> # app_id 就是服务端的 id 啦
...... # 你执行的各种指令
quit # 使用 quit 退出 SteamCMD
```

{: .important }
> 本文所有教程使用默认路径/设置

---