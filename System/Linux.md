<!-- TOC -->

- [command](#command)
- [目录](#目录)
- [发行版](#发行版)
- [设置](#设置)
- [资源](#资源)

<!-- /TOC -->

# command

[97 条前端可能用到的 Linux 常用命令总结](https://mp.weixin.qq.com/s/DuMVH1-kxkIpUzxDs7p4og)<br>
[Linux命令大全搜索工具](https://wangchujiang.com/linux-command/)<br>

# 目录

```shell
/
+- bin (binaries)存放二进制可执行文件
+- sbin (super user binaries)存放二进制可执行文件，只有root才能访问
+- etc (etcetera)存放系统配置文件
+- usr (unix shared resources)用于存放共享的系统资源
+- home 存放用户文件的根目录
+- root 超级用户目录
+- dev (devices)用于存放设备文件
+- lib (library)存放跟文件系统中的程序运行所需要的共享库及内核模块
+- mnt (mount)系统管理员安装临时文件系统的安装点
+- boot 存放用于系统引导时使用的各种文件
+- tmp (temporary)用于存放各种临时文件
+- var (variable)用于存放运行时需要改变数据的文件
```

# 发行版


![../.resource/System-JVM-memory_heap.png](../.resource/System-Linux-distribute.jpg)

# 设置

- 终端 Tab 忽略大小写

```bash
# home
touch .inputrc
# set completion-ignore-case on
vim .inputrc
```

# 资源

[GitHub://linux](https://github.com/torvalds/linux)<br>
[Linux X86 程序启动 – main函数是如何被执行的？](https://luomuxiaoxiao.com/?p=516)<br>
[Linux 文件系统详解](https://mp.weixin.qq.com/s/yuyRNlNQQQs6BHJKtQJOQg)<br>
[the-art-of-command-line](https://github.com/jlevy/the-art-of-command-line)<br>
[服务器性能指标（一）——负载（Load）分析及问题排查](https://mp.weixin.qq.com/s/s4MkM6UDo5TOLhfnZadGsQ)<br>
[服务器性能指标（二）——CPU利用率分析及问题排查](https://mp.weixin.qq.com/s/iXVi-5ksjSlU7t0H9Dhjwg)<br>
[如何读懂火焰图？](https://mp.weixin.qq.com/s/ujYSGr_UphO4IkNt12BbXg)<br>
