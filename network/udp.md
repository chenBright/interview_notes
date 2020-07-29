# UDP

## UDP如何实现可靠传输

[UDP如何实现可靠传输](https://www.jianshu.com/p/6c73a4585eba)提供了一个可靠UDP的简单设计：

1. 添加seq/ack机制，确保数据发送到对端

2. 添加发送和接收缓冲区，主要是用户超时重传

3. 添加超时重传机制

[UDP如何实现可靠传输](https://www.jianshu.com/p/1a7206bbca52)的设计：

1. 超时重传（定时器）

2. 有序接受 （添加包序号）

3. 应答确认 （Seq/Ack应答机制）

4. 滑动窗口、流量控制等机制 （滑动窗口协议）

## UDP包大小

- [UDP中一个包的大小最大能多大?](https://segmentfault.com/a/1190000017959319)
- [告知你不为人知的UDP-疑难杂症和使用](https://zhuanlan.zhihu.com/p/25622691)
- [为什么 TCP/IP 协议会拆分数据](https://draveness.me/whys-the-design-tcp-segment-ip-packet)
- [TCP UDP包大小分析](https://juejin.im/post/5b4aecd7f265da0fa21a74cf)
- [TCP层的分段和IP层的分片之间的关系 & MTU和MSS之间的关系](https://blog.csdn.net/yusiguyuan/article/details/22782943)
- [为什么 TCP/IP 协议会拆分数据](https://draveness.me/whys-the-design-tcp-segment-ip-packet/)