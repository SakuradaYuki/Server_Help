---
layout: page
title: Steam游戏服务器搭建指南
permalink: /Tutorials/steamGameServerHostManual.html
parent: Tutorials
---

_写给自己看的 Steam 游戏的各种服务器的搭建教程_

_阅读本文章需要并默认你对 __Linux系统__ 与 __常用工具/指令__ 有一定的了解, 包括但不限于: cd rm vim screen 等_

_还要会看错误log和搜索引擎的使用方法_

_还要对各种格式的配置文件有初步了解_

_[Link]为官方/详细的文档_

___不要执行你看不懂的指令___

---
---

# 目录

_点击跳转和复制都会有的, 就看我能鸽多久了, 现阶段直接 <kbd>Ctrl</kbd> + <kbd>F</kbd> 吧_

1. 准备 SteamCMD
2. Don't Starve together

---
---

# 准备SteamCMD

<p style='color:#bb0000;text-align:center'>!!! Warning: Do not run steamcmd while operating as the root user. Doing so is a security risk. !!!</p>
<p style='color:#bb0000;text-align:center'>!!! 警告: 登录为 root 用户身份时不要运行 steamcmd. 这样做会带来安全风险。 !!!</p>

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

<p style='color:#bb0000;text-align:center'>本文所有教程使用默认路径/设置</p>

---
---

# Don't Starve together

[官方文档[Link]](https://forums.kleientertainment.com/forums/topic/64441-dedicated-server-quick-setup-guide-linux/) [Fandom[Link]](https://dontstarve.fandom.com/wiki/Guides/Don%E2%80%99t_Starve_Together_Dedicated_Servers) [Fandom_zh-cn[Link]](https://dontstarve.fandom.com/zh/wiki/%E5%A4%9A%E4%BA%BA%E7%89%88%E9%A5%A5%E8%8D%92%E7%8B%AC%E7%AB%8B%E6%9C%8D%E5%8A%A1%E5%99%A8)

__app_id: 343050__

---

## 准备配置文件

先[新建服务器](https://accounts.klei.com/account/game/servers?game=DontStarveTogether), 在网页这边可以直接 Configure Server. 不用写配置文件还是不错的, 不过只有一些基础设置, 高级设置后续会提到 

配置内容说明: 

```shell
Server Token = <token> # 不用管
Maximum Players = <int> # 最大人数
Server Playstyle = <string> # 世界类型: Servival (生存), Relaxed (休闲), Endless (无尽), Wilderless (荒野), Lights out (永夜)
Cluster Name = <string> # 服务器名称
Cluster Description = <string> # 服务器简介
Cluster Password = <string> # 密码
```

下载得到一个 MyDediServer.zip , 解压后得到以下文件

```shell
.
└── MyDediServer                
    ├── Caves                    # 洞穴相关文件夹
    │   ├── server.ini           # 洞穴服务器配置文件
    │   └── worldgenoverride.lua # 洞穴地图生成配置文件
    ├── Master                   # 主世界相关文件夹
    │   ├── server.ini           # 主世界服务器配置文件
    │   └── worldgenoverride.lua # 主世界地图生成配置文件
    ├── cluster.ini              # 服务端配置文件
    └── cluster_token.txt        # Token
```

这里来解释几个可能需要使用/修改/添加的参数

| 文件名                        |             配置项              | 参数数据类型 |                        说明                        |
| :---------------------------- | :-----------------------------: | :----------: | :------------------------------------------------: |
| MyDediServer<br>└ cluster.ini | [GAMEPLAY] -> pause_when_empty  |     bool     |                  在无人时是否停止                  |
| MyDediServer<br>└ cluster.ini | [GAMEPLAY] -> vote_kick_enabled |     bool     |                    投票踢人功能                    |
| MyDediServer<br>└ cluster.ini |     [NETWORK] -> tick_rate      |     int      | 每秒通信次数，越高游戏体验越好, 服务器负载也会变大 |
| MyDediServer<br>└ cluster.ini |  [NETWORK] -> whitelist_slots   |     int      |        白名单人员保留数目, 小于等于最大人数        |
| MyDediServer<br>└ cluster.ini |     [MISC] -> max_snapshots     |     int      |            最大快照数, 默认一天一个快照            |
| MyDediServer<br>└ cluster.ini |       [SHARD] -> bind_ip        |    string    |                  服务器监听的地址                  |
| Caves<br>└ server.ini         |      [SHARD] -> master_ip       |    string    |                Master 服务器的地址                 |
| Caves<br>└ server.ini         |     [SHARD] -> master_port      |     int      |           监听 Master 服务器的 UDP 端口            |
| Caves<br>└ server.ini         |     [SHARD] -> cluster_key      |    string    |                      连接密码                      |

解释一下 server.ini 中的 master_ip、master_port、cluster_key 的作用: 

饥荒服务器分为主世界与洞穴世界，这两个世界的服务端是独立运行的。洞穴世界的服务器可以架设到另一台服务器上，与主世界服务器依靠 master_ip, master_port, cluster_key 这三个参数来进行通讯, ip 端口和密码，这很好理解。这也使得多世界成为可能(大概吧)。

---

## 启动服务器

将修改配置文件后的 `MyDediServer` 文件夹复制粘贴到服务器的 `~/.klei/DoNotStarveTogether/` 下

服务端启动指令:

`cd <path_to_DSTServer>/bin64 && ./dontstarve_dedicated_server_nullrenderer_x64 [args]`

其中 dontstarve_dedicated_server_nullrenderer_x64 为服务端二进制文件

[args] 可选:

|                       args                       |                                    说明                                     |
| :----------------------------------------------: | :-------------------------------------------------------------------------: |
|              -port [1024 .. 65535]               |                           强制服务器使用特定端口                            |
|                 -tick [15 .. 60]                 |                      强制服务器以特定的 tick 速率运行                       |
|                -players [1 .. 64]                |                    强制服务器中允许的最大玩家数, 最大64                     |
|                     -console                     |                         启用命令行,可执行 Lua 代码                          |
|                       -lan                       |                 局域网模式, 不出现在服务器列表, 无法拆礼物                  |
| -persistent_storage_root \<path_to_root_folder\> | 设置永久存储的根目录, 默认为 $HOME/.klei <br>配合 conf_dir 使用控制存档位置 |
|       -conf_dir \<relative_path_to_save\>        |                   强制服务器从备用目录加载保存和设置数据                    |
|         -cluster \<path_to_save_folder\>         |                              存档文件夹的名字                               |
|              -shard \<world_name\>               |       世界的文件夹名称<br>如洞穴世界默认为Caves<br>主世界默认为Master       |

存档最终将指向 \<path_to_root_folder\>/\<relative_path_to_save\>/\<path_to_save_folder\>

然后再运行服务器即可, 推荐使用 screen 或 tmux, 命令示例: 

`cd ~/Steam/steamapps/common/Don\'t\ Starve\ Together\ Dedicated\ Server/bin64 && ./dontstarve_dedicated_server_nullrenderer_x64 -console -cluster MyDediServer -shard Master`

---

## MOD

### MOD 文件获取

假如你想下载 __简易血条DST__ , 在对应创意工坊网页, 右键复制网页URL, 得到 https://steamcommunity.com/sharedfiles/filedetails/?id=1207269058. 复制id=后的那串数字, 得到 1207269058.

编辑 ~/Steam/steamapps/common/Don\'t\ Starve\ Together\ Dedicated\ Server/mod/dedicated_server_mods_setup.lua 文件

添加 `ServerModSetup("1207269058")` 即可

最终结果如下:

```lua
--There are two functions that will install mods, ServerModSetup and ServerModCollectionSetup. Put the calls to the functions in this file and they will be executed on boot.

--ServerModSetup takes a string of a specific mod's Workshop id. It will download and install the mod to your mod directory on boot.
	--The Workshop id can be found at the end of the url to the mod's Workshop page.
	--Example: http://steamcommunity.com/sharedfiles/filedetails/?id=350811795
	--ServerModSetup("350811795")

--ServerModCollectionSetup takes a string of a specific mod's Workshop id. It will download all the mods in the collection and install them to the mod directory on boot.
	--The Workshop id can be found at the end of the url to the collection's Workshop page.
	--Example: http://steamcommunity.com/sharedfiles/filedetails/?id=379114180
	--ServerModCollectionSetup("379114180")
ServerModSetup("1207269058")

```

`--`开头的为注释, 阅读并掌握以后删掉即可

### MOD 启用

在 \<path_to_save_folder\>/\<world_name\> 文件夹下新建 modoverrides.lua

此种方法可以独立设置每个存档启用的 mod

模板如下

```lua
return {
    ["workshop-MOD_ID_1"]={ configuration_options={  }, enabled=true }, -- 有几个mod就需要几个
    ["workshop-MOD_ID_2"]={ configuration_options={  }, enabled=true }
}
```

结果如下:

```lua
return {
    ["workshop-1207269058"]={ configuration_options={  }, enabled=true }
}
```

你可以导出本地存档中的 modoverrides.lua, 复制本地的 mod 源文件到服务器, 这样其实更方便, 网络上教程很多, 也可以直接在上方提到的 Fandom_zh-cn[Link] 中查看

---

## 防火墙端口相关

默认只需要放行 \<path_to_root_folder\>/\<relative_path_to_save\>/\<path_to_save_folder\>\<world_name\>\server.ini 中 [NETWORK] -> server_port 端口, 默认为11000, 协议为udp

路径按照本文设置来的话就是 $HOME/.klei/DoNotStarveTogether/MyDediServer/Master/server.ini

__到此, 已经完成饥荒服务器的基本设置, 下面会提到一些高级设置__

---

## 高级设置

### 管理员、黑白名单

在运行服务器的时候你可能会注意到这三行日志:

```
[00:00:00]: OnLoadPermissionList: $HOME/.klei//DoNotStarveTogether/MyDediServer/blocklist.txt (Failure)
[00:00:00]: OnLoadPermissionList: $HOME/.klei//DoNotStarveTogether/MyDediServer/adminlist.txt (Failure)
[00:00:00]: OnLoadUserIdList: $HOME/.klei//DoNotStarveTogether/MyDediServer/whitelist.txt (Failure)
```

它们分别是黑名单、管理员名单和白名单

黑名单与白名单不需要解释。可以通过白名单人员保留数目 (MyDediServer cluster.ini 中的 [NETWORK] -> whitelist_slots) 将服务器配置为仅白名单人员可加入, 配合密码双重保险

黑名单在游戏内按 <kbd>Tab</kbd>, 里面就可以直接 ban 人

饥荒服务器的部分指令可以无视权限执行, 比如回档, 为了防止恶意操作可添加自己为管理员

这三个文件的使用方式也很简单, 以添加自己为管理员为例:

当然你得在 $HOME/.klei/DoNotStarveTogether/MyDediServer/ 下新建对应文本文件

打开[网站](https://accounts.klei.com/account/info)并登录账号, 找到 __Klei User ID__ 下面的 ID, 格式为 KU_\*\*\*\*\*\_\*\* , 直接复制粘贴到 adminlist.txt 即可, 一行填入一个ID

服务器内其他人的UserID可以在日志中查询

### 游戏内指令

按 __<kbd>`</kbd>__ (通常叫飘号键) 唤出控制台

|             指令              | 可选参数类型/格式 |                       解释                       |
| :---------------------------: | :---------------: | :----------------------------------------------: |
|           c_save()            |         /         |                   立刻保存世界                   |
|         c_shutdown()          |       bool        | 为真时保存游戏后关闭服务器, 为假时不保存直接关闭 |
|         c_rollback()          |        int        |          回档, int为回到倒数第几个存档           |
|      c_regenerateworld()      |         /         |                     删档重开                     |
|         c_announce()          |     "string"      |           发送公告, 内容需用引号括起来           |
|          c_connect()          | "ip","port","psw" |            链接到指定服务器, 无需搜索            |
| TheNet:Kick()<br>TheNet:Ban() |   Klei User ID    |                  踢人<br>封禁人                  |
