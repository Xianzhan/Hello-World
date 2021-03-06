<!-- TOC -->

- [应用层](#应用层)
    - [WebSocket](#websocket)
- [传输层](#传输层)
    - [TCP](#tcp)
        - [三次握手](#三次握手)
        - [四次挥手](#四次挥手)
        - [粘包拆包](#粘包拆包)

<!-- /TOC -->

# 应用层

## WebSocket

[全双工通信的 WebSocket](https://mp.weixin.qq.com/s/-h9UM800bODnEMbfHvKtDw)<br>

# 传输层

## TCP

### 三次握手

```shell
            C   S
   SYN_SEND |   |
            | ↘ | 
            |   | SYN_RCV
            | ↙ | 
Established |   |
            | ↘ |
            |   | Established
       data | ↔ | data
```

1. 第一次握手(SYN=1, seq=x)，发送完毕后，客户端进入 SYN_SEND 状态
2. 第二次握手(SYN=1, ACK=1, seq=y, ACKnum=x+1)， 发送完毕后，服务器端进入 SYN_RCVD 状态。
3. 第三次握手(ACK=1，ACKnum=y+1)，发送完毕后，客户端进入 ESTABLISHED 状态，当服务器端接收到这个包时，也进入 ESTABLISHED 状态，TCP 握手，即可以开始数据传输。

### 四次挥手

```shell
           C   S
      data | ↔ | data
           |   |
FIN_WAIT_1 | ↘ | 
           | ↙ | CLOSE_WAIT
FIN_WAIT_2 | ↙ | 
 TIME_WAIT | ↘ | LAST_ACK
    CLOSED |   | CLOSE
```

1. 第一次挥手(FIN=1，seq=a)，发送完毕后，客户端进入 FIN_WAIT_1 状态
2. 第二次挥手(ACK=1，ACKnum=a+1)，发送完毕后，服务器端进入 CLOSE_WAIT 状态，客户端接收到这个确认包之后，进入 FIN_WAIT_2 状态
3. 第三次挥手(FIN=1，seq=b)，发送完毕后，服务器端进入 LAST_ACK 状态，等待来自客户端的最后一个ACK。
4. 第四次挥手(ACK=1，ACKnum=b+1)，客户端接收到来自服务器端的关闭请求，发送一个确认包，并进入 TIME_WAIT状态，等待了某个固定时间（两个最大段生命周期，2MSL，2 Maximum Segment Lifetime）之后，没有收到服务器端的 ACK ，认为服务器端已经正常关闭连接，于是自己也关闭连接，进入 CLOSED 状态。服务器端接收到这个确认包之后，关闭连接，进入 CLOSED 状态。

### 粘包拆包

1. **粘包**

在用户数据量非常小的情况下，极端情况下，一个字节，该 TCP 数据包的有效载荷非常低，传递 100 字节的数据，需要 100 次 TCP 传送，100 次 ACK，在应用及时性要求不高的情况下，将这 100 个有效数据拼接成一个数据包，那会缩短到一个 TCP 数据包，以及一个 ack，有效载荷提高了，带宽也节省了

非极端情况，有可能两个数据包拼接成一个数据包，也有可能一个半的数据包拼接成一个数据包，也有可能两个半的数据包拼接成一个数据包

2. **拆包**

拆包和粘包是相对的，一端粘了包，另外一端就需要将粘过的包拆开，举个栗子，发送端将三个数据包粘成两个 TCP 数据包发送到接收端，接收端就需要根据应用协议将三个数据包拆分成两个数据包

还有一种情况就是用户数据包超过了 mss(最大报文长度)，那么这个数据包在发送的时候必须拆分成几个数据包，接收端收到之后需要将这些数据包粘合起来之后，再拆开

**拆包原理**

用户如果自己需要拆包，基本原理就是不断从 TCP 缓冲区中读取数据，每次读取完都需要判断是否是一个完整的数据包

1.如果当前读取的数据不足以拼接成一个完整的业务数据包，那就保留该数据，继续从tcp缓冲区中读取，直到得到一个完整的数据包<br>
2.如果当前读到的数据加上已经读取的数据足够拼接成一个数据包，那就将已经读取的数据拼接上本次读取的数据，够成一个完整的业务数据包传递到业务逻辑，多余的数据仍然保留，以便和下次读到的数据尝试拼接<br>

[【网络编程】高性能网络编程之TCP消息的发送](https://mp.weixin.qq.com/s/6vtF0eyi5Da98yAoEsVOVg)<br>
[【协议森林】图说TCP之滑动窗口和拥塞窗口](https://mp.weixin.qq.com/s/UtyLffohQ2yOJXvpVALE2Q)<br>
[【协议森林】技术面试，“三次握手，四次挥手”背后那些事](https://mp.weixin.qq.com/s/rSfR0zCRmYXZIiLU-XgzSA)<br>
[【协议森林】详解TCP之滑动窗口](https://mp.weixin.qq.com/s/3VqdjEK4QkER4Q05JgfjhQ)<br>
[跟着动画学习TCP三次握手和四次挥手](https://mp.weixin.qq.com/s/vwZycVjAgodMJe9c4xTliQ)<br>
