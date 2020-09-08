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



