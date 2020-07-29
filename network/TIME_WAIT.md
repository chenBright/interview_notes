# TIME_WAIT

`TIME_WAIT`状态的主要目的有两个：

- 优雅的关闭 TCP 连接，也就是尽量保证被动关闭的一端收到它自己发出去的`FIN`报文的`ACK`确认报文；
- 处理延迟的重复报文，这主要是为了避免前后两个使用相同四元组的连接中的前一个连接的报文干扰后一个连接。

## 2MSL

MSL 就是 max segment lifetime，这是一个 IP 数据包能在网络中生存的最长时间，超过这个时间，IP 数据包将在网络中消失。MSL 在[RFC 793](https://tools.ietf.org/html/rfc793)中建议是**2分钟**，而源自 Berkeley 的 TCP 实现传统上使用**30秒**。

[为什么tcp的TIME_WAIT状态要维持2MSL](https://cloud.tencent.com/developer/article/1450264)：

> `TIME_WAIT`至少需要持续 2MSL 时长，这2个 MSL 中的第一个 MSL 是为了等自己发出去的最后一个 ACK 从网络中消失，而第二 MSL  是为了等在对端收到 ACK 之前的一刹那可能重传的 FIN 报文从网络中消失。另外，虽然说维持 TIME_WAIT 状态一段时间有2个目的，但这段时间具体应该多长主要是为了达成上述第二个目的而设计的。

## 快速回收TIME_WAIT

使用下面的命令可以查看系统中`TIME_WAIT`的数量：

```shell
netstat -a | grep TIME_WAIT | wc -l
# 或者
netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}' # 该命令更快
```

### 编辑`/etc/sysctl.conf`文件中的属性

**net.ipv4.tcp_timestamps**

> RFC 1323 在 TCP Reliability 一节里，引入了 timestamp 的 TCP option，两个 4 字节的时间戳字段，其中第一个 4 字节字段用来保存发送该数据包的时间，第二个 4 字节字段用来保存最近一次接收对方发送到数据的时间。有了这两个时间字段，也就有了后续优化的余地。
>
> tcp_tw_reuse 和  tcp_tw_recycle 都依赖这些时间字段。



**net.ipv4.tcp_tw_reuse**

> 字面意思，reuse TIME_WAIT状态的连接。
>
> 时刻记住一条 socket 连接，就是那个五元组，出现 TIME_WAIT 状态的连接，一定出现在主动关闭连接的一方。所以，当主动关闭连接的一方，再次向对方发起连接请求的时候（例如，客户端关闭连接，客户端再次连接服务端，此时可以复用了；负载均衡服务器，主动关闭后端的连接，当有新的HTTP请求，负载均衡服务器再次连接后端服务器，此时也可以复用），可以复用 TIME_WAIT 状态的连接。
>
> 通过字面解释，以及例子说明，你看到了，tcp_tw_reuse 应用的场景：某一方，需要不断的通过“短连接”连接其他服务器，总是自己先关闭连接（TIME_WAIT 在自己这方），关闭后又不断的重新连接对方。
>
> 那么，当连接被复用了之后，延迟或者重发的数据包到达，新的连接怎么判断？到达的数据是属于复用后的连接，还是复用前的连接呢？那就需要依赖前面提到的两个时间字段了。复用连接后，这条连接的时间被更新为当前的时间，当延迟的数据达到，延迟数据的时间是小于新连接的时间，所以，内核可以通过时间判断出，延迟的数据可以安全的丢弃掉了。
>
> 这个配置，依赖于连接双方，同时对 timestamps 的支持。同时，这个配置，仅仅影响 outbound 连接，即做为客户端的角色，连接服务端 [connect(dest_ip, dest_port)] 时复用 TIME_WAIT 的 socket。



**net.ipv4.tcp_tw_recycle**

> 字面意思，销毁掉 TIME_WAIT。
>
> 当开启了这个配置后，内核会快速的回收处于 TIME_WAIT 状态的 socket 连接。多快？不再是 2MSL，而是一个 RTO（retransmission timeout，数据包重传的 timeout 时间）的时间，这个时间根据 RTT 动态计算出来，但是远小于 2MSL。
>
> 有了这个配置，还是需要保障丢失重传或者延迟的数据包，不会被新的连接（注意，这里不再是复用了，而是之前处于 TIME_WAIT 状态的连接已经被 destroy 掉了，新的连接，刚好是和某一个被 destroy 掉的连接使用了相同的五元组而已）所错误的接收。在启用该配置，当一个 socket 连接进入 TIME_WAIT 状态后，内核里会记录包括该 socket 连接对应的五元组中的对方IP等在内的一些统计数据。当然也包括从该对方 IP 所接收到的最近的一次数据包时间。当有新的数据包到达，只要时间晚于内核记录的这个时间，数据包都会被统统的丢掉。
>
> 这个配置，依赖于连接双方对 timestamps 的支持。同时，这个配置，主要影响到了 inbound 的连接（对outbound 的连接也有影响，但是不是复用），即作为服务端角色，客户端连进来，服务端主动关闭了连接，TIME_WAIT 状态的 socket 处于服务端，服务端快速的回收该状态的连接。
>
> 由此，如果客户端处于 NAT 的网络（多个客户端，同一个 IP 出口的网络环境），如果配置了 tw_recycle，就可能在一个 RTO 的时间内，只能有一个客户端和自己连接成功（不同的客户端发包的时间不一致，造成服务端直接把数据包丢弃掉）。

### socket选项`SO_REUSEADDR`

在程序中可以使用`SO_REUSEADDR`快速重启处于`timewait`状态的服务器。

这个 socket 选项通知内核：如果端口忙，但 TCP 状态位于`TIME_WAIT`，可以重用端口。

> 一个 socket 由相关五元组构成，协议、本地地址、本地端口、远程地址、远程端口。SO_REUSEADDR 仅仅表示可以重用本地本地地址、本地端口，整个相关五元组还是唯一确定的。所以，重启后的服务程序有可能收到非期望数据。必须慎重使用 SO_REUSEADDR 选项。

**一般来说，一个端口释放后会等待两分钟之后才能再被使用，SO_REUSEADDR是让端口释放后立即就可以被再次使用。**

`SO_REUSEADDR`用于对 TCP 处于`TIME_WAIT`状态下的 socket，才可以重复绑定使用。server程序总是应该在调用`bind()`之前设置`SO_REUSEADDR`选项。先调用`close()`的一方会进入`TIME_WAIT`状态。

`SO_REUSEADDR`允许启动一个监听服务器并捆绑其众所周知端口，即使以前建立的将此端口用做他们的本地端口的连接仍存在。这通常是重启监听服务器时出现，若不设置此选项，则`bind()`时将出错。

## TMIE_WAIT接收到数据包

参考[TIME_WAIT状态下对接收到的数据包如何处理](https://blog.csdn.net/justlinux2010/article/details/8725479)。

- 如果返回值是`TCP_TW_SYN`，则说明接收到的是一个“合法”的`SYN`包（也就是说这个`SYN`包可以接受），这时会首先查找内核中是否有对应的监听套接字，如果存在相应的监听套接字，则会释放`TIME_WAIT`状态的传输控制结构，跳转到`process`处开始处理，开始建立一个新的连接。如果没有找到监听套接字会执行到`TCP_TW_ACK`分支
- 如果返回值是`TCP_TW_ACK`，则会调用`tcp_v4_timewait_ack`发送`ACK`，然后跳转到`discard_it`标签处，丢掉数据包。
- 如果返回值是`TCP_TW_RST`，则会调用`tcp_v4_send_reset`给对端发送`RST`包，然后丢掉数据包。
- 如果返回值是`TCP_TW_SUCCESS`，则会直接丢掉数据包。

## TMIE_WAIT或者CLOSE_WAIT过多

- [TCP CLOSE_WAIT 详解](https://www.zhuxiaodong.net/2018/tcp-close-wait-instruction/)
- [线上大量CLOSE_WAIT的原因深入分析](https://juejin.im/post/5c0cf1ed6fb9a04a08217fcc)
- [解决 Linux 下 TIME_WAIT 和 CLOSE_WAIT 过多的问题](https://blog.minhow.com/articles/linux/solve-time-and-close-wait/)
- [TIME_WAIT和CLOSE_WAIT过多](https://blog.51cto.com/net881004/2164024)
- [HttpClient连接池抛出大量ConnectionPoolTimeoutException: Timeout waiting for connection异常排查](https://blog.csdn.net/shootyou/article/details/6615051)

## 参考

- [为什么tcp的TIME_WAIT状态要维持2MSL](https://cloud.tencent.com/developer/article/1450264)
- [系统调优你所不知道的TIME_WAIT和CLOSE_WAIT](https://zhuanlan.zhihu.com/p/40013724)
- [time_wait的快速回收和重用](https://www.cnblogs.com/LUO77/p/8555103.html)
- [再谈应用环境下的TIME_WAIT和CLOSE_WAIT](https://blog.csdn.net/shootyou/article/details/6622226)

