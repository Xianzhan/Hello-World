<!-- TOC -->

- [操作系统](#操作系统)
    - [基本功能](#基本功能)
    - [相关概念](#相关概念)
- [command](#command)
- [发行版](#发行版)
- [设置](#设置)
- [编码](#编码)
- [WSL](#wsl)
- [资源](#资源)

<!-- /TOC -->

# 操作系统

## 基本功能

- 操作系统是管理计算机硬件和软件资源的计算机程序
    - 处理器资源
    - IO 设备资源
    - 存储器资源
    - 文件资源
- 管理配置内存、决定资源供需顺序、控制输入输出设备等
- 提供用户和系统交互的操作界面

## 相关概念

- 并发性
    - 并行: 指两个或多个事件可以在同一时刻发生
    - 并发: 指两个或多个事件可以在同一时间间隔发生
- 共享性
    - 表现为操作系统中的资源可供多个并发的程序共同使用
    - 这种共同使用的形式称之为资源共享
- 虚拟性
    - 虚拟性表现为把一个物体实体转变为若干个逻辑实体
    - 物理实体是真实存在的, 逻辑实体是虚拟的
    - 虚拟的技术主要由时分复用技术和空分复用技术
    - 资源在时间上进行复用, 不同程序并发使用
    - 多道程序分时使用计算机的硬件资源
- 异步性
    - 在多道程序环境下, 允许多个进程并发执行
    - 进程在使用资源时可能需要等待或放弃

[操作系统面试题](https://mp.weixin.qq.com/s/sTGsnLf0UhzeMoKHSEJI9g)<br>
[操作系统兴衰史](https://mp.weixin.qq.com/s/xmbv7tFWLillGBtVmHLu5g)<br>

# command

[97 条前端可能用到的 Linux 常用命令总结](https://mp.weixin.qq.com/s/DuMVH1-kxkIpUzxDs7p4og)<br>
[Linux命令大全搜索工具](https://wangchujiang.com/linux-command/)<br>

# 发行版

```shell
Arch

Debian
 +- Knoppix
 |   +- Morphix
 |       +- Hiwix Hiweed
 |           +- Deepin
 +- Ubuntu

Red Hat
 +- Red Hat Enterprise
 |   +- CentOS
 |   +- Oracle Enterprise
 +- Fedora Core
     +- Fedora
         +- Moblin 2
         |   +- MeeGo
         +- Fusion

SLS
 +- Slackware
     +- S.u.S.E
         +- SuSE
```

# 设置

- 终端 Tab 忽略大小写

```bash
# home
touch .inputrc
# set completion-ignore-case on
vim .inputrc
```

# 编码

Windows 10 设置默认编码
1. Windows 设置
2. 时间和语言
3. 语言
4. 管理语言设置
5. 更改系统区域设置...
6. 使用 Unicode UTF-8 提供全球语言支持(U)

# WSL

> Windows Subsystem for Linux

[Install](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

# 资源

[Github://os-tutorial](https://github.com/cfenollosa/os-tutorial)<br>
[GitHub://linux](https://github.com/torvalds/linux)<br>
[Linux X86 程序启动 – main函数是如何被执行的？](https://luomuxiaoxiao.com/?p=516)<br>
[Linux 文件系统详解](https://mp.weixin.qq.com/s/yuyRNlNQQQs6BHJKtQJOQg)<br>
[the-art-of-command-line](https://github.com/jlevy/the-art-of-command-line)<br>
[服务器性能指标（一）——负载（Load）分析及问题排查](https://mp.weixin.qq.com/s/s4MkM6UDo5TOLhfnZadGsQ)<br>
[服务器性能指标（二）——CPU利用率分析及问题排查](https://mp.weixin.qq.com/s/iXVi-5ksjSlU7t0H9Dhjwg)<br>
[如何读懂火焰图？](https://mp.weixin.qq.com/s/ujYSGr_UphO4IkNt12BbXg)<br>
