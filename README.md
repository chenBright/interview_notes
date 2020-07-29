# interview_notes
CS 面试笔记

## Algorithm

- [位运算](./algorithm/位运算.md)
- [跳表](./algorithm/skiplist.md)
- [时间轮](./algorithm/timerwheel.md)

## OS

- [面试题](./os/面试题.md)
- [死锁](./os/死锁.md)
- [同步与异步、阻塞与非阻塞](./os/同步与异步、阻塞与非阻塞.md)
- [存储管理](./os/存储管理.md)
- [线程](./os/线程.md)
- [CSAPP](./os/csapp/md)

## Network

- [粘包](./network/粘包.md)
- [IP地址分类](./network/IP地址分类/IP地址分类.md)
- [DDos攻击](./network/DDos.md)
- [TIME_WAIT](./network/TIME_WAIT.md)
- [从 URL 输入到页面展现到底发生什么？](./network/Web页面请求过程.md)

## C++

- [C++ 11](./cpp/cpp11.md)
- [const](./cpp/const.md)
- [static](./cpp/static.md)
- [volatile](./cpp/volatile.md)
- [类型转换](./cpp/cast.md)
- [内存相关](./cpp/memory/memory.md)
- [NULL、nullptr的区别](./cpp/nullptr.md)
- [虚函数](./cpp/virtual.md)
- [bind、function、placeholder](./cpp/memory/bind_function_placeholder.md)
- [为什么vector扩容为2倍](./cpp/vector/vector_memory.md))
- [C++对象模型](./cpp/memory/object_model.md)
- [引用的实质](./cpp/reference.md)

## 网络编程

- [Reactor、Proactor、Actor](./network_progaming/model/IO_model.md)
- [字节序](./network_progaming/endianness.md)
- [惊群问题](./network_progaming/shock_group.md)
- [backlog](./network_programing/backlog/listen_backlog.md)

## 系统编程

- [共享内存的位置](./system_programing/shared_memory/shared_memory.md)

## Linux

- [top](./linux/top.README)
- [scp](./linux/scp.README)
- [ps](./linux/ps.README)
- [ulimit](./linux/ulimit.md)

## Git

- [一些常用Git操作](./git/operations.md)

## 面经

- [深信服](./Interview_problems/深信服.md)
- [CVTE](./Interview_problems/cvte.md)
- [阿里](./Interview_problems/阿里.md)
- [腾讯](./Interview_problems/腾讯.md)

## 资料

- [C/C++ 面试基础知识总结](https://www.yuque.com/huihut/interview/readme)
- [面试题干货在此](https://www.nowcoder.com/discuss/57978)
- [面试资料-C++](https://blog.nowcoder.net/n/597b7119c7ff40308fc6f9b59fdb041d)
- [面试资料-操作系统](https://blog.nowcoder.net/n/cf431b32dc1244868e34c9afac189832)
- [面试资料-网络](https://blog.nowcoder.net/n/3e8a113ce1a24a29b6f737522f49fa2f)
- [面试资料-数据库](https://blog.nowcoder.net/n/67050afd11bf4d71bb8df3f84b04aa70)
- [五万字长文 C C++ 面试知识总结（上）](https://juejin.im/post/5cbd7603e51d456e2446fcaf)
- [MySQL 面试题](http://www.jishuchi.com/books/mysql-interview)
- [后端架构师技术图谱](https://github.com/xingshaocheng/architect-awesome/blob/master/README.md)
- [JavaGuide](https://github.com/Snailclimb/JavaGuide)
- [MySQL 面试题](http://www.jishuchi.com/books/mysql-interview)
- [高性能mysql](https://www.kancloud.cn/jdxia/booknote/541867)
- [[灵魂拷问]MySQL面试高频一百问(工程师方向)（文末附MySQL经典面试题34道）](https://zhuanlan.zhihu.com/p/74932727)

## TODO

- 项目
  - 库构建（静态动态链接）、项目部署eventfd
  - gdb 多线程/多进程/core dump
  - Valgrind
  - gcc 编译优
  - ~~负载均衡~~
  - [说说压力测试工具](https://blog.huoding.com/2017/05/31/620)
  - 日志来不及写，怎么处理（tinyWeb中有讨论）、[线程池的陷阱](https://www.cnblogs.com/qicosmos/p/3225099.html)
  - 日志的另一种实现：[分享一个支持IPC的C++多生产者单消费者消息队列](https://zhuanlan.zhihu.com/p/42865831?utm_source=wechat_session&utm_medium=social&utm_oi=52921169346560)
  - [epoll LT、ET收发数据，回答面试官为什么不用ET](https://www.zhihu.com/question/263931629/answer/1012125255)
  - 项目难点：线程安全函数
  - 不足：fd处理没优先级、线程->协程、进程版本不适合作为库（nginx）、周边基础组件少（连接池、rpc）
  - 跟redis比较
    - 相同：
      - Reactor
      - 
    - 不同：
      - 单线程：不需要考虑线程安全问题
      - 定时任务：使用链表管理，简单但是低效
- 基础
  - KMP
- linux
  - awk、sed
  - netstat:检查网络状态，tcpdump:截获数据包，ipcs:检查共享内存，ipcrm:解除共享内存
- 文档
  - Eventfd vs pipe
    - [让事件飞 ——Linux eventfd 原理与实践](https://zhuanlan.zhihu.com/p/40572954)
    - [linux网络编程--eventfd](https://blog.csdn.net/majianfei1023/article/details/51199702)

## 总结

本文从两方面提高遥感图像的检索性能：引入自注意力模块和使用多重相似性损失函数。第一，引入自注意力模块解决了卷积局部运算不能处理上下文信息的缺点，只需要较小的计算量，就能获取数据或特征的长距离、多层的依赖关系，最终提取遥感图像的关键特征。另一方面，使用多重相似性损失函数，综合考虑自相似性、负相对相似性和正相对相似性，使得提取的特征更具区分度——类间分散，类内紧凑。本文分别针对这两方面在4个典型的数据集上做了实验，实验表明，这两方面对遥感图像检索性能的提升有显著的作用。同时，本文也通过实验对比了自注意力模块结合多重相似性损失函数的方法和其他优良的遥感图像检索方面的检索性能差异，我们的方法明显优于其他遥感图像检索方面。

尽管我们提出的方法有更好的性能，但是仍然存在一些不能忽略的缺点。例如，我们的自注意力模块只能连接到CNN的卷积层。但是，全连接层和卷积层也可以用作特征表示。在某些条件下，某些CNN的全连接层比卷积层可以获得更好的检索性能。因此，如何克服使用自注意力模块的局限性是我们未来的重点之一。此外，我们的自注意力模块值考虑了空间信息，没有考虑通道上信息。如何结合空间和通道上的信息是一个未来的研究方面。



