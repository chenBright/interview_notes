# 惊群问题

多个线程（或者进程）同时等待一个事件的到来并准备处理事件，当事件到达时，把所有等待该事件的线程（或进程）均唤醒，但是只能有一个线程最终可以获得事件的处理权，其他所有线程又重新陷入睡眠等待下次事件到来。这种线程被频繁唤醒却又没有真正处理事件导致 CPU 无谓浪费称为计算机中的“**惊群问题**”。

参见[维基百科对惊群的定义](http://en.wikipedia.org/wiki/Thundering_herd_problem)。

## 惊群问题出现场景

### Linux2.6内核版本之前系统API中的accept调用

在 Linux 2.6 内核版本之前，当多个线程中的`accept`函数同时监听同一个`listenfd`的时候，如果此`listenfd`变成可读，则系统会唤醒所有使用`accept`函数等待`listenfd`的所有线程（或进程），但是最终只有一个线程可以`accept`调用返回成功，其他线程的`accept`函数调用返回`EAGAIN`错误，线程回到等待状态，这就是`accept`函数产生的惊群问题。

参见[Linux 惊群效应之 Nginx 解决方案](https://zhuanlan.zhihu.com/p/51251700)。在 Linux 2.6 版本之后，内核通过引入一个标记位 `WQ_FLAG_EXCLUSIVE`解决了`accept`函数的惊群问题，当内核收到一个连接之后，只会唤醒等待队列上的第一个线程（或进程），从而避免了惊群问题。Linux 2.6 版本之后，，解决掉了 accept 惊群效应。

### epoll函数中的惊群问题

如果我们使用多线程`epoll`对同一个`fd`进行监控，当fd事件到来时，内核会把所有`epoll`线程唤醒，因此产生惊群问题。无论`epoll_create`在`fork`之前调用还是之后调用，都会产生惊群问题。

在 2016 年一月，Linux 之父 Linus 向内核提交了一个补丁。参见 [epoll: add EPOLLEXCLUSIVE flag](https://github.com/torvalds/linux/commit/df0108c5da561c66c333bb46bfe3c1fc65905898)。

其中的关键代码是：

```c
if (epi->event.events & EPOLLEXCLUSIVE)
	add_wait_queue_exclusive(whead, &pwq->wait);
else
	add_wait_queue(whead, &pwq->wait);
```

简而言之，通过增加一个 `EPOLLEXCLUSIVE` 标志位作为辅助。如果用户开启了 `EPOLLEXCLUSIVE` ，那么在加入内核等待队列时，使用 `add_wait_queue_exclusive` 否则使用 `add_wait_queue`。

至于这两个函数的用法，可以参考这篇文章[Handing wait queues](https://www.halolinux.us/kernel-reference/handling-wait-queues.html)。

其中有这样一段描述

> The add_wait_queue( ) function inserts a nonexclusive process in the first position of a wait queue list. The add_wait_queue_exclusive( ) function inserts an exclusive process in the last position of a wait queue list.

但是[聊聊网络事件中的惊群效应](https://manjusaka.itscoder.com/posts/2019/03/28/somthing-about-thundering-herd/)中的 demo 中显示`EPOLLEXCLUSIVE` 标志位也不能彻底解决惊群问题。因为在于 `EPOLLEXCLUSIVE` **只保证唤醒的进程数小于等于我们开启的进程数**，而不是直接唤醒所有进程，也不是只保证唤醒一个进程

[man 描述](http://man7.org/linux/man-pages/man2/epoll_ctl.2.html)：

> Sets an exclusive wakeup mode for the epoll file descriptor that is being attached to the target file descriptor, fd. When a wakeup event occurs and multiple epoll file descriptors are attached to the same target file using EPOLLEXCLUSIVE, one or more of the epoll file descriptors will receive an event
> with epoll_wait(2). The default in this scenario (when EPOLLEXCLUSIVE is not set) is for all epoll file descriptors to receive an event. EPOLLEXCLUSIVE is thus useful for avoiding thundering herd problems in certain scenarios.

简单来讲就是，就目前而言，系统并不能严格保证惊群问题的解决。很多时候我们还是要依靠应用层自身的设计来解决。

为何内核不能像解决`accept`问题那样解决`epoll`的惊群问题呢？内核可以解决`accept`调用中的惊群问题，是因为内核清楚的知道`accept`调用只可能一个线程调用成功，其他线程必然失败。而对于`epoll`调用而言，**内核不清楚到底有几个线程需要对该事件进行处理**，所以只能将所有线程全部唤醒。

### 线程池中的惊群问题

在实际应用程序开发中，为了避免线程的频繁创建销毁，我们一般建立**线程池**去并发处理，而线程池最经典的模型就是**生产者-消费者模型**，包含一个任务队列，当队列不为空的时候，线程池中的线程从任务队列中取出任务进行处理。一般使用条件变量进行处理，当我们往任务队列中放入任务时，需要唤醒等待的线程来处理任务，如果我们使用`C++`标准库中的函数`notify_all`来唤醒线程，则会将所有的线程都唤醒，然后最终只有一个线程可以获得任务的处理权，其他线程在此陷入睡眠，因此产生惊群问题。

## 惊群效应消耗了什么

- Linux 内核对用户进程（线程）频繁地做**无效的调度**、**上下文切换**等使系统性能大打折扣。上下文切换（context switch）过高会导致 CPU 像个搬运工，频繁地在寄存器和运行队列之间奔波，更多的时间花在了进程（线程）切换，而不是在真正工作的进程（线程）上面。直接的消耗包括 **CPU 寄存器要保存和加载**（例如程序计数器）、**系统调度器的代码需要执行**。间接的消耗在于**多核 cache 之间的共享数据**。
- 为了确保只有一个进程（线程）得到资源，需要对资源操作进行加锁保护，加大了系统的开销。目前一些常见的服务器软件有的是通过锁机制解决的，比如 Nginx（它的锁机制是默认开启的，可以关闭）；还有些认为惊群对系统性能影响不大，没有去处理，比如 Lighttpd。

## 惊群问题解决办法

### 对于`epoll`函数调用的惊群问题解决办法

1. 参考`Nginx`的解决办法：多个进程将`listenfd`加入到`epoll`之前，首先尝试获取一个全局的`accept_mutex`互斥锁，只有获得该锁的进程才可以把`listenfd`加入到`epoll`中。当网络连接事件到来时，只有`epoll`中含有`listenfd`的线程才会被唤醒并处理网络连接事件，从而解决了`epoll`调用中的惊群问题。[accept与epoll惊群](https://pureage.info/2015/12/22/thundering-herd.html)中有详细的处理过程。
2. `EPOLLEXCLUSIVE`是 4.5 内核新添加的一个 epoll 的标识，Ngnix 在 1.11.3 之后添加了`NGX_EXCLUSIVE_EVENT`。`EPOLLEXCLUSIVE`标识会保证一个事件发生时候只有一个线程会被唤醒，以避免多侦听下的“惊群”问题。不过任一时候只能有一个工作线程调用 accept，限制了真正并行的吞吐量。
3. kernel 3.9 增加了 `SO_REUSEPORT` socket option，该选项允许服务端 socket 复用端口，通过`hash`机制将连接分配客户端到具体的进程，而这一切都是由内核处理，在内核层面实现负载均衡注：可以使用`SO_REUSEADDR`快速重启处于`timewait`状态的服务器。

### 对于线程池中的惊群问题

我们需要分情况看待，有时候业务需求就是需要唤醒所有线程，那么这时候使用`notify_all`唤醒所有线程就不能称为”惊群问题“，因为CPU并没有无谓消耗。而对于只需要唤醒一个线程的情况，我们需要使用`notify_one`函数代替`notify_all`只唤醒一个线程，从而避免惊群问题。

## SO_REUSEPORT 带来的小问题

SO_REUSEPORT打开后，再有请求进来不再是各个进程一起去抢，而是内核通过五元组Hash来分配，所以不再会惊群了。但是可能会导致撑死或者饿死的问题，比如一个cpu一直在做一件耗时的任务（比如压缩），但是内核通过hash分配过来的时候是不知道的（抢锁就不会发生这种情况，你没空就不会去抢），以Nginx为例：

因为Nginx是ET模式，epoll要一直将事件处理完毕才能进入epoll_wait（才能响应新的请求）。带来了新的问题：如果有一个慢请求（比如gzip压缩文件需要2分钟），那么处理这个慢请求的进程在reuseport模式下还是会被内核分派到，但是这个时候他如同hang死了，新分配进来的请求无法处理。如果不是reuseport模式，他在处理慢请求就根本腾不出来时间去在惊群中抢到锁。

如果不开启SO_REUSEPORT模式，那么即使有一个进程在处理慢请求，那么他就不会去抢accept锁，也就没有accept新连接，这样就不应影响新连接的处理。

但是开启了SO_REUSEPORT后，内核没法感知你的worker是不是特别忙，只是按Hash逻辑派发连接。也就是SO_REUSEPORT会导致rt偏差更大（抖动明显一些）。[这跟MySQL Thread Pool导致的卡顿原理类似，多个Pool类似这里的SO_REUSEPORT。](https://plantegg.github.io/2020/06/05/MySQL线程池导致的延时卡顿排查/)



在OS层面一个连接hash到了某个socket fd，但是正好这个 listen socket fd 被关了，已经被分到这个 listen socket fd 的 accept 队列上的请求会被丢掉，具体可以[参考](https://engineeringblog.yelp.com/2015/04/true-zero-downtime-haproxy-reloads.html) 和 LWN 上的 [comment](https://lwn.net/Articles/542866/)

从 Linux 4.5 开始引入了 SO_ATTACH_REUSEPORT_CBPF 和 SO_ATTACH_REUSEPORT_EBPF 这两个 BPF 相关的 socket option。通过巧妙的设计，应该可以避免掉建连请求被丢掉的情况。



- [SO_REUSEPORT学习笔记](http://www.blogjava.net/yongboy/archive/2015/02/12/422893.html)

## 参考

- [C++性能榨汁机之惊群问题](http://irootlee.com/juicer_thundering_herd/)
- [聊聊网络事件中的惊群效应](https://manjusaka.itscoder.com/posts/2019/03/28/somthing-about-thundering-herd/)
- [Linux 惊群效应之 Nginx 解决方案](https://zhuanlan.zhihu.com/p/51251700)
- [accept与epoll惊群](https://pureage.info/2015/12/22/thundering-herd.html)
- [惊群探究](https://vcpu.me/%E6%83%8A%E7%BE%A4/)
- [Linux 最新SO_REUSEPORT特性](https://www.cnblogs.com/Anker/p/7076537.html)
- [Ngnix 是如何解决 epoll 惊群的](https://simpleyyt.com/2017/06/25/how-ngnix-solve-thundering-herd/)
- [epoll在多线程中的应用-EPOLLEXCLUSIVE和REUSEPORT](https://blog.csdn.net/dream0130__/article/details/104009426)
- [记一次多进程epoll程序的“惊群”效应](https://sq.163yun.com/blog/article/183642424546795520)
- [epoll和惊群](https://plantegg.github.io/2019/10/31/epoll%E5%92%8C%E6%83%8A%E7%BE%A4/)
- [nginx源码阅读(十四).惊群问题的解决](https://blog.csdn.net/Move_now/article/details/78509211)
- [Linux 惊群效应之 Nginx 解决方案](https://www.infoq.cn/article/kKyLZqAzDomqDkwXgXGv)
- [再谈Linux epoll惊群问题的原因和解决方案](https://blog.csdn.net/dog250/article/details/80837278)
- [Linux 最新SO_REUSEPORT特性](https://www.cnblogs.com/alantu2018/p/8469546.html)
- [Linux惊群效应详解（最详细的了吧）](https://blog.csdn.net/lyztyycode/article/details/78648798?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-4.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-4.channel_param)

