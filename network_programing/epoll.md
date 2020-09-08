# epoll

`epoll`相关知识。

## epoll

- [epoll 深入学习](https://blog.leosocy.top/epoll%E6%B7%B1%E5%85%A5%E5%AD%A6%E4%B9%A0/)
- [深入理解IO复用技术之epoll](https://juejin.im/post/5e92e2b351882573834ed1ae?utm_source=gold_browser_extension#heading-20)总结了很多比较深入的知识。
- [网络基础 — 阅读源码后理解epoll的一个错误纠正](https://blog.csdn.net/Dawn_sf/article/details/79526229)
- [epoll源码解析翻译------说使用了mmap的都是骗子](https://www.cnblogs.com/l2017/p/10830391.html)
- [epoll学习报告](https://blog.ykyi.net/2013/08/epoll%E5%AD%A6%E4%B9%A0%E6%8A%A5%E5%91%8A/)
- [Linux内核：poll机制](https://blog.csdn.net/JansonZhe/article/details/48576025)
- [select、poll、epoll简介](https://xiaoxiami.gitbook.io/linux-server/wu-zhong-i-o-mo-xing/selectpollepolljian-jie)
- [Linux 环境开发（二）：Linux IO 复用之 select/poll/epoll 的差异分析](https://toutiao.io/posts/377995/app_preview)
- [select、poll、epoll之间的区别总结整理](https://www.cnblogs.com/Anker/p/3265058.html)
- [深入理解网络 IO 模型](https://www.cyhone.com/articles/reunderstanding-of-non-blocking-io/)

## POSIX aio

POSIX aio 是 glibc 在用户态用 pthread 实现的，用回调或者 signal 通知（记得是这样，回头去查一下）。
后来的 kernel aio 没有跟进，我想像我这样普通程序员也不太会有机会用到 linux aio。

## IOCP

- [NIO相关基础篇三](https://mp.weixin.qq.com/s/5SKgdkC0kaHN495psLd3Tg?)
- [Netty（二）：Netty为啥去掉支持AIO](https://blog.csdn.net/lirenzuo/article/details/79465975)
- [IOCP的使用和技术内幕](https://xnerv.wang/iocp-usage-and-inside/)
- [完成端口IOCP详解](https://www.cnblogs.com/talenth/p/7068392.html)

## 多个线程如何操作同一个epoll fd

- [多个线程如何操作同一个epoll fd](https://blog.csdn.net/menggucaoyuan/article/details/38959725)
- [epoll的那些坑](http://linbo.github.io/2019/04/14/epoll-pitfall)
- [Epoll listen socket fd accept、读写数据](https://juejin.im/post/6844904118054551559#heading-14)

## EPOLLONESHOT

即使使用 ET 模式，一个 socket 上的某个事件还是可能被触发多次。比如：一个线程在读取完某个 socket 上的数据后开始处理这些数据，而在数据的处理过程中该 socket 上又有新数据可读（`EPOLLIN`再次被触发），此时另外一个线程被唤醒用来读取这些新的数据。于是就出现了两个线程同时操作一个  socket  的局面。而我们希望一个socket 连接在任一时刻都只能被一个线程处理，这就可以通过`EPOLLONESHOT`事件实现。
对于注册了`EPOLLONESHOT`事件的文件描述符，**操作系统最多触发其上注册的一个事件，且只触发一次**，除非我们使用`epoll_ctl`函数重置该文件描述符上注册的`EPOLLONESHOT`事件。这样，在一个线程使用 socket 时，其他线程无法操作 socket。同样，只有在**该socket被处理完后，须立即重置该socket的EPOLLONESHOT事件**，以确保这个socket在下次可读时，其EPOLLIN事件能够被触发，进而让其他线程有机会操作这个socket。

- [I/O复用之 EPOLLONESHOT 事件](http://blog.xiyoulinux.org/detail.jsp?id=5309#)
- [epoll在多线程中的应用-EPOLLEXCLUSIVE和REUSEPORT](https://blog.csdn.net/dream0130__/article/details/104009426)

## 服务器

- [Nginx为什么比Apache Httpd高效：原理篇](http://www.mamicode.com/info-detail-1156329.html)

## C10K、C10M

- [高性能网络编程经典：《The C10K problem》](http://www.52im.net/thread-560-1-1.html)
- [架构师实践日｜从C10K到C10M高性能网络的探索与实践 ](https://blog.qiniu.com/archives/4941)