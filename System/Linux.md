[CMD](#cmd)
  
  
[文件系统](#文件系统)
  
[性能](#性能)
 - [netstat](#netstat)
 - [perf](#perf)
 - [top](#top)
 - [uptime](#uptime)

# CMD

[97 条前端可能用到的 Linux 常用命令总结](https://mp.weixin.qq.com/s/DuMVH1-kxkIpUzxDs7p4og)<br>

# 文件系统

[Linux 文件系统详解](https://mp.weixin.qq.com/s/yuyRNlNQQQs6BHJKtQJOQg)<br>

# 性能

[服务器性能指标（一）——负载（Load）分析及问题排查](https://mp.weixin.qq.com/s/s4MkM6UDo5TOLhfnZadGsQ)<br>
[服务器性能指标（二）——CPU利用率分析及问题排查](https://mp.weixin.qq.com/s/iXVi-5ksjSlU7t0H9Dhjwg)<br>

## netstat

`netstat -tanp`

查看 TCP 网络连接状况

## perf

[如何读懂火焰图？](https://mp.weixin.qq.com/s/ujYSGr_UphO4IkNt12BbXg)<br>

## top

包含了系统全局的很多指标信息, 包括系统负载情况、系统内存使用情况、系统 CPU 使用情况等等

## uptime

系统的运行时、平均负载, 包括 1 分钟、5 分钟、15 分钟内可以运行的任务平均数量, 包括正在运行的任务, 虽然可以运行但正在等待某个处理器空闲以及阻塞在不可中断休眠状态的进程(等待 IO, 状态为 D)的任务