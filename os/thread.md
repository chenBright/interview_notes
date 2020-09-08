# 线程

本篇介绍了**线程**相关的知识。

##线程共享哪些资源

### 线程共享资源

- 堆：由于堆是在进程空间中开辟出来的，所以它是理所当然地被共享的；因此`new`出来的都是共享的（16位平台上分全局堆和局部堆，局部堆是独享的）。
-  全局变量：它是与具体某一函数无关的，所以也与特定线程无关；因此也是共享的
-  静态变量：虽然对于局部变量来说，它在代码中是“放”在某一函数中的，但是其存放位置和全局变量一样，存于堆中开辟的.bss和.data段，是共享的。
- 文件等公用资源：这个是共享的，使用这些公共资源的线程必须同步。
- 地址空间
- 子进程、闹铃、信号及信号服务程序、记账信息
- 进程代码段

### 线程独享资源

- 栈
- 寄存器
- 程序计数器
- 状态字
- 线程优先级
- 错误返回码

## 线程

- [Linux 线程实现机制分析](https://www.ibm.com/developerworks/cn/linux/kernel/l-thread/index.html)
- [关于“内核线程”、“用户线程”概念的理解](https://blog.csdn.net/u012927281/article/details/51602898?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)
- [普通线程和内核线程](https://www.cnblogs.com/alantu2018/p/8526916.html)
- [Linux虚拟地址空间布局以及进程栈和线程栈总结](https://www.cnblogs.com/xzzzh/p/6596982.html)
- [Linux 上下文切换 寄存器 内核线程 用户线程](https://tuweiguang.cn/2019/07/21/Linux%20%E4%B8%8A%E4%B8%8B%E6%96%87%E5%88%87%E6%8D%A2%20%E5%AF%84%E5%AD%98%E5%99%A8%20%E5%86%85%E6%A0%B8%E7%BA%BF%E7%A8%8B%20%E7%94%A8%E6%88%B7%E7%BA%BF%E7%A8%8B/)
- [Linux下的进程类别（内核线程、轻量级进程和用户进程）--Linux进程的管理与调度（四）](https://www.cnblogs.com/linhaostudy/p/9585506.html)
- [进程线程常见基础问题](http://whatbeg.com/2019/06/05/processthread.html)
- [面试官问：高并发下，你都怎么选择最优的线程数？](https://juejin.im/post/5ec62f26e51d4578853d1918?utm_source=gold_browser_extension)
- [Java线程池实现原理及其在美团业务中的实践](https://tech.meituan.com/2020/04/02/java-pooling-pratice-in-meituan.html)
- [高并发下，你都怎么选择最优的线程数？](https://juejin.im/post/5ec62f26e51d4578853d1918?utm_source=gold_browser_extension)

## 同步

- [pthread的各种同步机制](https://casatwy.com/pthreadde-ge-chong-tong-bu-ji-zhi.html)
- [关于同步的一点思考-上](https://github.com/farmerjohngit/myblog/issues/6#)
- [Linux进程同步机制-Futex](https://cloud.tencent.com/developer/article/1176832)
- [linux内核级同步机制--futex](https://juejin.im/post/5bbca253e51d451e132521e8)
- [linux futex浅析](https://yq.aliyun.com/articles/6043)
- [聊聊并发（五）——原子操作的实现原理](https://www.infoq.cn/article/atomic-operation)

## 线程调度为什么比进程调度更少开销？

在 Unix 中：

- 同一个进程中的线程间进行切换，差不多仅需要切换线程的上下文(如寄存器状态等)；
- 切换到已经映射好的进程，同样差不多仅需要切换进程的**上下文**和**页表**切换；
- 切换到尚未建立映射的进程，除了需要切换上下文、页表切换，更重要的是需要**建立映射**，冲刷 TLB；
- 不同进程的线程之间切换，开销同上一条；

在上面所说的开销中，切换进程上下文开销很小几乎可忽略，而**建立映射**的开销是很大的。所以我认为线程与进程调度的开销差距主要在于**是否需要建立映射**。

## **确保线程安全的几种方式？**

1. 原子操作
2. 同步与锁
3. 可重入
4. `volatile`：阻止过度优化

## 调度

- [linux进程/线程调度策略(SCHED_OTHER,SCHED_FIFO,SCHED_RR)](https://blog.csdn.net/u012007928/article/details/40144089)
- [Linux-C++ pthread线程的调度策略测试](https://blog.sunzhihao.com/pthread%E7%BA%BF%E7%A8%8B%E7%9A%84%E8%B0%83%E5%BA%A6%E7%AD%96%E7%95%A5%E6%B5%8B%E8%AF%95/)
- [进程管理笔记三、完全公平调度算法CFS](https://blog.csdn.net/XD_hebuters/article/details/79623130)
- [linux内核分析——CFS（完全公平调度算法）](https://www.cnblogs.com/tianguiyu/articles/6091378.html)
- [CFS 完全公平调度算法](https://www.jianshu.com/p/e7417f10b6c4)
- [Linux系统调度原理浅析（二） - 敬维](https://jingwei.link/2019/02/13/linux-process-thread-schedule-2.html)

## mutex

- [互斥锁（mutex）的底层原理是什么？ 操作系统具体是怎么实现的？？](https://www.zhihu.com/question/332113890/answer/1052024052)

## spin lock

- [面试必备之深入理解自旋锁](https://zhuanlan.zhihu.com/p/40729293)
- [【原创+整理】线程同步之详解自旋锁](https://www.cnblogs.com/cposture/p/SpinLock.html)

## pthread_cond_wait 为什么需要传递 mutex 参数？

1. 要理解条件变量的作用是在等待某个条件达成时自身要进行睡眠或阻塞，避免忙等待带来的不必要消耗，所以条件变量的作用在于同步。条件变量这个变量其实本身不包含条件信息，条件的判断不在`pthread_cond_wait`函数功能中，而需要外面进行条件判断。这个条件通常是多个线程或进程的共享变量，这样就很清楚了，对于共享变量很可能产生竞争条件尤其还对共享变量加了条件限制，所以从这个角度看，必须对共享变量加上互斥锁。

2. `pthread_cond_wait`函数用于等待目标条件变量。`mutex`参数用于保护条件变量的互斥锁，以确保`pthread_cond_wait`操作的原子性，在调用`pthread_cond_wait`前，必须确保互斥锁`mutex`已经加锁，否则将导致不可预期的结果。`pthread_cond_wait`函数执行时，首先把调用线程放入条件变量的等待队列中，然后将互斥锁`mutex`解锁。可见，从`ptread_cond_wait`开始执行到其调用线程被放入条件变量的等待队列之间的这段时间内，`ptread_cond_signal`和`pthread_cond_broadcast`函数不会修改条件变量。换言之，`pthread_cond_wait`函数不会错过目标条件变量的任何变化。当`pthread_cond_wait `函数成功返回时，互斥锁`mutex`将被再次锁上。

- [pthread_cond_wait()函数为什么需要传递一个互斥量参数](http://progrom.cn/2017/07/22/condvar-with-mutex/)
- [pthread_cond_wait 为什么需要传递 mutex 参数？](https://www.zhihu.com/question/24116967/answer/507224988)
- [为什么 pthread 条件变量(condition variable)函数要用到 mutex ？](https://feng-qi.github.io/2017/05/08/Why-do-pthreads-condition-variable-functions-require-a-mutex/)
- [关于条件变量和互斥锁为何配合使用的思考](https://blog.csdn.net/zrf2112/article/details/52287915)

## 读写锁

- [OS: 读者写者问题(写者优先+LINUX+多线程+互斥量+代码)](https://love.junzimu.com/archives/2800)
- [Linux下写者优先的读写锁的设计](https://www.ibm.com/developerworks/cn/linux/l-rwlock_writing/)

##  ThreadLocal

- [面试官：听说你精通并发编程，来说说你对ThreadLocal的理解](https://juejin.im/post/5ecfb4baf265da7700281163#heading-1)

## 协程

- [云风 coroutine 协程库源码分析](https://www.cyhone.com/articles/analysis-of-cloudwu-coroutine/)
- [微信 libco 协程库源码分析](https://www.cyhone.com/articles/analysis-of-libco/)
- [libco 分析（上）：协程的实现](http://kaiyuan.me/2017/07/10/libco/)
- [libco 分析(下)：协程的管理](http://kaiyuan.me/2017/10/20/libco2/)
- [libco的阅读](https://reachfish.github.io/2017/08/09/libco_read/)
- [s_task - 跨平台的C语言协程多任务库]()
- [动态链接黑魔法: Hook 系统函数](hhttps://github.com/xhawk18/s_task/blob/master/readme_cn.md#%e5%85%b6%e4%bb%96%e5%8d%8f%e7%a8%8b%e5%ba%93%e5%af%b9%e6%af%94ttp://kaiyuan.me/2017/05/03/function_wrapper/)
- [Go 协程调度——基本原理与初始化](https://github.com/LeoYang90/Golang-Internal-Notes/blob/master/Go%20%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6.md)
- [Golang 协程调度原理](https://feiybox.com/2020/03/14/Golang-%E5%8D%8F%E7%A8%8B%E8%B0%83%E5%BA%A6%E5%8E%9F%E7%90%86/)
- [Head First of Golang Scheduler](https://zhuanlan.zhihu.com/p/42057783)
- [30+张图讲解：Golang调度器GMP原理与调度全分析](https://mp.weixin.qq.com/s/SEPP56sr16bep4C_S0TLgA)
- [大神是如何学习 Go 语言之调度器与 Goroutine](https://mp.weixin.qq.com/s?__biz=MzAxMTA4Njc0OQ==&mid=2651438277&idx=4&sn=6afb0ebd60a5a4db5c77e075b3914095&chksm=80bb6237b7cceb217150de4d41a9e98a4e3285c7c885728f92f644d8b0161338319e890783ae&scene=21#wechat_redirect)