# TCP

## 网络模型

- [网络OSI七层及各层作用](https://blog.csdn.net/u011774517/article/details/67631439)
- [OSI 7层模型和TCP/IP 4层模型](https://zhuanlan.zhihu.com/p/32059190)
- [或许这样能帮你了解 OSI 七层模型](https://juejin.im/post/59eb06b1f265da430f313c7f)

## 基本知识

- [两张动图-彻底明白TCP的三次握手与四次挥手](https://blog.csdn.net/qzcsu/article/details/72861891)
- [TCP协议详解（TCP报文、三次握手、四次挥手、TIME_WAIT状态、滑动窗口、拥塞控制、粘包问题、状态转换图）](https://blog.csdn.net/daaikuaichuan/article/details/83475809)
- [TCP模型知识点总结](https://xnerv.wang/tcp-model-knowledge-summary/)
- [TCP/IP报文头部结构整理](https://blog.csdn.net/ythunder/article/details/65664309)
- [Tcp报文简介以及头部选项字段(Tcp Options字段)](https://blog.csdn.net/Hollake/article/details/89327474)
- [常用的TCP Option](https://blog.csdn.net/blakegao/article/details/19419237)
- [关于 TCP/IP，必知必会的十个问题](https://juejin.im/post/598ba1d06fb9a03c4d6464ab#heading-27)
- [计算机网络太难？了解这一篇就够了](https://juejin.im/post/5d896cccf265da03bd055c87)

## 重传机制

- 很好的文章：[TCP重传机制刨根问底，TCP的玄学调参](https://segmentfault.com/a/1190000021064727)

## 流量控制 

[TCP流量控制](https://juejin.im/post/598ba1d06fb9a03c4d6464ab#heading-22)

## 拥塞控制

- [面试之路（29）-TCP流量控制和拥塞控制-滑动窗口协议详解](https://blog.csdn.net/lpjishu/article/details/51366691)
- [浅谈TCP（2）：流量控制与拥塞控制](https://juejin.im/post/5acae2b55188252b0b2013aa#heading-18)
- [TCP流量控制和拥塞控制](https://segmentfault.com/a/1190000011641120)

[知乎提到拥塞控制的缺点：](https://www.zhihu.com/question/47560918/answer/232939764)

> 还有拥塞控制算法也不能很好的适应网络不太稳定的场景（比如无线网络），TCP的拥塞控制认为丢包是因为网络传输饱和，所以一但出现丢包就采取指数级避让，而无线网络因为短暂的信号干扰导致的丢包并不是因为网络传输饱和，此时采取指数级避让是不合适的，会导致无线传输的速度骤降。
>
> 关于这个已经有很多改进的算法了，比如BBR。

[网络拥塞控制(五) 传统TCP存在的缺陷](https://blog.csdn.net/bytxl/article/details/44834057)提到：

> **传统的TCP总是把包的丢失解释为网络发生了拥塞，而假定链路错误造成的分组丢失是忽略不计的**，这种情况是基于当时 V. Jacobson 的观察，认为链路错误的几率太低从而可以忽略，然而在高速网络中，这种假设是不成立的，**当数据传输速率比较高时，链路错误是不能忽略的**。在无线网络中，链路的误码率更高，因此，如果笼统地认为分组丢失就是拥塞所引起的，从而降低一半的速率，这是对网络资源的极大浪费。**拥塞的判断需要两个连续的分组丢失。**

[TCP BBR拥塞控制算法解析](https://blog.csdn.net/ebay/article/details/76252481)提到：

> 2.1 基于丢包的拥塞控制算法的两大缺陷
> （1）不能区分是拥塞导致的丢包还是错误丢包
> 基于丢包的拥塞控制方法把数据包的丢失解释为网络发生了拥塞，而假定链路错误造成的分组丢失是忽略不计的，这种情况是基于当时 V. Jacobson 的观察，认为链路错误的几率太低从而可以忽略，然而在高速网络中，这种假设是不成立的，当数据传输速率比较高时，链路错误是不能忽略的。在无线网络中，链路的误码率更高，因此，如果笼统地认为分组丢失就是拥塞所引起的，从而降低一半的速率，是对网络资源的极大浪费。
> （2）引起缓冲区膨胀
> 我们会在网络中设置一些缓冲区，用于吸收网络中的流量波动，在连接的开始阶段，基于丢包的拥塞控制方法倾向于填满缓冲区。当瓶颈链路的缓冲区很大时，需要很长时间才能将缓冲区中的数据包排空，造成很大的网络延时，这种情况称之为缓冲区膨胀。在一个先进先出队列管理方式的网络中，过大的 buffer 以及过长的等待队列，在某些情况下不仅不能提高系统的吞吐量甚至可能导致系统的吞吐量近乎于零。并且当所有缓冲区都被填满时，会出现丢包。

### 为什么HTTP下载文件，一开始一点一点地加快速度，最后达到峰值？

这个过程跟拥塞控制的过程很像，可以从拥塞控制方面分析。

- [为什么http下载不是直接下载而是一点一点地加快速度？如果直接下载会有什么后果？](https://www.zhihu.com/question/50349911/answer/120569222)

### 为什么多线程下载能加速

- [知乎](https://www.zhihu.com/question/19914902/answer/33647099)
- [QUIC和TCP](http://blog.chinaunix.net/uid-28387257-id-4335291.html)

## TCP建立连接为什么是三次握手，为什么不是两次或四次?

- [为什么 TCP 建立连接需要三次握手](https://draveness.me/whys-the-design-tcp-three-way-handshake)
- [为什么 TCP 建立连接需要三次握手 · Why's THE Design?](https://draveness.me/whys-the-design-tcp-three-way-handshake)
- [TCP建立连接为什么是三次握手，为什么不是两次或四次?](https://blog.csdn.net/to_be_better/article/details/54885684)
- [TCP三次握手原理以及为什么是3次](https://juejin.im/post/5dd680726fb9a05a81711360)
- [知乎: TCP 为什么是三次握手，而不是两次或者四次](https://www.zhihu.com/question/24853633)
- [TCP连接的建立和关闭详解](http://walkerdu.com/2017/04/07/tcp-create-close-note/)
- [Java面试知识点：TCP 三次握手和四次挥手协议](https://zhuanlan.zhihu.com/p/83136330)
- [“三次握手，四次挥手”你真的懂吗？](https://zhuanlan.zhihu.com/p/53374516)

[知乎](https://www.zhihu.com/question/49324380/answer/116066114)介绍了握手失败的情况：

> 服务器收到 SYN 包后发出 SYN+ACK 数据包，服务器进入 SYN_RECV 状态。
>
> 而这个时候客户端发送 ACK 给服务器失败了，服务器没办法进入 ESTABLISH 状态，这个时候肯定不能传输数据的，不论客户端主动发送数据与否，服务器都会有定时器发送第二步 SYN+ACK 数据包，如果客户端再次发送 ACK 成功，建立连接。
>
> 如果一直不成功，服务器肯定会有超时设置，超时之后会给客户端发 RTS 报文，进入 CLOSED 状态，这个时候客户端应该也会关闭连接。

## 为什么 **TCP** 建立连接是三次握手，终止连接却要四次挥手？

因为当服务器收到客户的 SYN 连接请求报文之后，可以直接发送 SYN+ACK 报文。 其中，ACK 报文是用来应答的，SYN 报文是用来同步的。但是关闭连接时，当服 务器收到 FIN 报文时，服务器执行被动关闭，这个 FIN 由 TCP 确认，也作为一个 文件结束符传递给接收端应用进程，放在已排队等候接收端应用进程处理的其它 数据之后，也就是说，服务器在接收到 FIN 之后，可能不会立即关闭 SOCKET， 所以只能先回复一个 ACK 报文，通知客户“你发的 FIN 报文我收到了”。只有等接收端应用进程处理完其它数据之后，服务器才会发送 FIN 报文，因此关闭连接 时服务器发来的 ACK 和 FIN 可能不能一起发送，所以需要四次挥手。

## TCP包的seq和ack号计算方法

- [TCP包的seq和ack号计算方法](https://blog.csdn.net/huaishu/article/details/93739446)

## MTU、MSS

**MTU = MSS + TCP Header + IP Header**

**mtu是网络传输最大报文包。 mss是网络传输数据最大值。**

**MTU**：maximum transmission unit，最大传输单元，由硬件规定，如以太网的 MTU 为 1500 字节。

**MSS**：maximum segment size，最大分节大小，为 TCP 数据包每次传输的最大数据分段大小，一般由发送端向对端 TCP 通知对端在每个分节中能发送的最大 TCP 数据。MSS 值为 MTU 值减去 IPv4 Header（20 Byte）和 TCP header（20 Byte）得到。

**分片**：若一 IP 数据报大小超过相应链路的 MTU 的时候，IPV4 和 IPV6 都执行分片（fragmentation），各片段到达目的地前通常不会被重组(re-assembling)。IPV4主机对其产生的数据报执行分片，IPV4路由器对其转发的数据也执行分片。然而IPV6只在数据产生的主机执行分片；IPV6路由器对其转发的数据不执行分片。

- [TCP/IP协议：最大传输单元MTU 和 最大分段大小MSS （TCP的分段和IP的分片）](https://blog.csdn.net/qq_26093511/article/details/78739198)

## keepalive

TCP 保活的作用：

1. **探测连接的对端是否存活**
在应用交互的过程中，可能存在以下几种情况：
- 客户端或服务器端意外断电、死机、崩溃、重启

- 中间网络已经中断，而客户端与服务器端并不知道

  利用保活探测功能，可以探知这种对端的意外情况，从而保证在意外发生时，可以释放半打开的TCP连接。

2. **防止中间设备因超时删除连接相关的连接表**
中间设备如防火墙等，会为经过它的数据报文建立相关的连接信息表，并为其设置一个超时时间的定时器，如果超出预定时间，某连接无任何报文交互的话，中间设备会将该连接信息从表中删除，在删除后，再有应用报文过来时，中间设备将丢弃该报文，从而导致应用出现异常。

表现：

1. 如果主机可达，对方就会响应ACK 应答，就认为是存活的。
2. 如果可达，但应用程序退出，对方就发 RST 应答，发送 TCP 撤消连接。
3. 如果可达，但应用程序崩溃，对方就发 FIN 消息。
4. 如果对方主机不响应 ACK、RST，继续发送直到超时，才撤消连接。默认二个小时。

- [TCP保活（TCP keepalive）](https://blog.csdn.net/chenlycly/article/details/81030671)
- [TCP keepalive的详解(解惑)](https://blog.csdn.net/lanyang123456/article/details/90578453)
- [TCP 连接断连问题剖析](https://www.ibm.com/developerworks/cn/aix/library/0808_zhengyong_tcp/index.html)
- [TCP Keepalive机制刨根问底](https://segmentfault.com/a/1190000021057175)

## socket

- [socket使用TCP协议时，send、recv函数解析以及TCP连接关闭的问题](https://www.cnblogs.com/lidabo/p/4534755.html)
- [[译]TCP Socket 是如何工作的?](https://colobu.com/2019/07/27/How-TCP-Sockets-Work/)
- [close和shutdown关闭TCP连接](https://segmentfault.com/a/1190000020303015)
- [linux网络编程系列（九）--优雅关闭以及如何检测对端已经关闭](https://juejin.im/post/5d1840335188255ee8176be2)
- [socket 编程 ： shutdown vs close](https://www.cnblogs.com/cnblogs-wangzhipeng/p/10162026.html)
- [linux网络编程系列（六）--setsockopt的常用选项](https://juejin.im/post/5d183f7f5188255d6a32de17)

## tcp同时打开

[TCP连接的建立和关闭详解](http://walkerdu.com/2017/04/07/tcp-create-close-note/)：

> TCP协议也是支持同时打开的，尽管这个概率是非常小的，因为这要求通信双方都知道对方的端口号，这需要双方都事先bind()各自的端口，并同时发出SYN包。TCP针对同时打开的情况，仅建立一条连接而不是两条连接。
> 对于同时打开连接的双方，在发送SYN包后，都进入SYN_SEND状态，在收到对端的SYN包后，进入SYN_RCVD状态，并对此SYN包再次回复SYN包进行确认，当双方都收到对端的SYN包ack后，就会进入ESTABLISHED状态。

## TCP双方同时关闭

[TCP连接的建立和关闭详解](http://walkerdu.com/2017/04/07/tcp-create-close-note/)：

> TCP协议是允许通信的双方同时执行关闭操作，即同时发送 FIN 包。当双方发送FIN包后，都进入 FIN_WAIT_1 状态（双方互不知对方已发送 FIN，所以和正常关闭一样）。当双方都收到 FIN 包后，TCP 协议层就会知道是双方是同时关闭，然后就会进入 CLOSING 状态，并对对方的 SYN 包进行 ack 确认。当双方收到对方的 ack 确认后，就会进入 TIME_WAIT 状态。

## TCP如何保证可靠传输

1. 确认和重传：接收方收到报文就会确认，发送方发送一段时间后没有收到确认就会重传。

2. 数据校验：TCP报文头有校验和，用于校验报文是否损坏

3. 数据合理分片和排序：

   TCP 会按最大传输单元(MTU)合理分片，接收方会缓存未按序到达的数据，重新排序后交给应用层。

   而 UDP：IP数据报大于1500字节，大于 MTU。这个时候发送方的 IP 层就需要分片，把数据报分成若干片，是的每一片都小于 MTU。而接收方 IP 层则需要进行数据报的重组。由于UDP的特性，某一片数据丢失时，接收方便无法重组数据报，导致丢弃整个UDP数据报。

4. 流量控制：当接收方来不及处理发送方的数据，能通过滑动窗口，提示发送方降低发送的速率，防止包丢失。

5. 拥塞控制：当网络拥塞时，通过拥塞窗口，减少数据的发送，防止包丢失。

- [网络学习笔记（二）：TCP可靠传输原理](https://juejin.im/post/5c8f615ff265da612009824a#heading-6)

## TCP连接后拔掉网线

[网络编程释疑之：TCP半开连接的处理](https://blog.51cto.com/yaocoder/1309358)：

> **其实我们可以在应用层模拟SO_KEEPALIVE的方式，用心跳包来模拟保活探测分节。**由于服务器通常要承担成千上万的并发连接，所以肯定是由客户端在应用层进行心跳来模拟保活探测分节，客户端多次收不到服务器的响应时可终止此TCP连接，而服务端可监测客户端的心跳包，若在一定时间间隔内未收到任何来自客户端的心跳包则可以终止此TCP连接，这样就有效避免了TCP半开连接的情况。

[linux C/C++服务器后台开发面试题总结](https://www.cnblogs.com/nancymake/p/6516933.html)：

> socket编程，如果 client 断电了，服务器如何快速知道？
>
> 使用定时器（适合有数据流动的情况）；
>
> 使用socket选项 SO_KEEPALIVE（适合没有数据流动的情况）; 
>
> 1. 自己编写心跳包程序，简单的说就是自己的程序加入一条线程，定时向对端发送数据包,，查看是否有 ACK，根据 ACK 的返回情况来管理连接。此方法比较通用，一般使用业务层心跳处理，灵活可控，但改变了现有的协议；
> 2. 使用 TCP 的 keepalive 机制，UNIX 网络编程不推荐使用 SO_KEEPALIVE 来做心）跳检测。
> keepalive 原理：TCP 内嵌有心跳包。以服务端为例，当 server 检测到超过一定时间（/proc/sys/net/ipv4/tcp_keepalive_time 7200 即 2 小时）没有数据传输，那么会向 client 端发送一个 keepalive packet。

- [网络编程释疑之：TCP半开连接的处理](https://blog.51cto.com/yaocoder/1309358)
- [网络编程释疑之：TCP连接拔掉网线后会发生什么](https://blog.51cto.com/yaocoder/1589919)
- [TCP连接后拔掉网线后socket句柄写入还会成功吗](https://libisky.com/post/5d426661e97ae5201b8d49ba)
- [epoll检测对端关闭](https://www.geek-share.com/detail/2724406084.html)

## 糊涂窗口综合症

- [糊涂窗口综合症](https://zh.wikipedia.org/wiki/%E7%B3%8A%E6%B6%82%E7%AA%97%E5%8F%A3%E7%BB%BC%E5%90%88%E7%97%87)
- [糊涂窗口综合症和Nagle算法](https://www.cnblogs.com/zhaoyl/archive/2012/09/20/2695799.html)
- [TCP-IP详解：糊涂窗口综合症（Silly Window syndrome）](https://blog.csdn.net/wdscq1234/article/details/52463952)
- [速读原著-TCP/IP(糊涂窗口综合症)](https://cloud.tencent.com/developer/article/1598141)

## linux内核收发数据包的处理过程

- [linux网络基础原理](https://juejin.im/post/5dc13de9e51d456eea09dac5#heading-0)
- [Linux内核中网络数据包的接收-第一部分 概念和框架](https://blog.csdn.net/dog250/article/details/50528280)
- [Linux内核中网络数据包的接收-第二部分 select/poll/epoll](https://blog.csdn.net/dog250/article/details/50528373?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)

## fast open

- [面试题-你听过TCP Fast Open (TFO/TCP快速打开)吗？能解释一下吗？](https://www.cnblogs.com/passzhang/p/12249539.html)
- [TCP 的那些事 | TCP Fast Open](https://blog.csdn.net/u014023993/article/details/85928026)

