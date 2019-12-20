# ps

Linux 中的`ps`命令是**Process Status**的缩写。`ps`命令用来列出系统中当前运行的那些进程。`ps`命令列出的是当前那些进程的快照，就是执行`ps`命令的那个时刻的那些进程，如果想要动态的显示进程信息，就可以使用`top`命令。

## Linux的进程状态

linux上进程有 5 种状态：

1. 运行（正在运行或在运行队列中等待）；
2. 中断（休眠中, 受阻, 在等待某个条件的形成或接受到信号）；
3. 不可中断（收到信号不唤醒和不可运行, 进程必须等待直到有中断发生）；
4. 僵死（进程已终止, 但进程描述符存在, 直到父进程调用wait4()系统调用后释放）；
5. 停止（进程收到SIGSTOP, SIGTSTP, SIGTTIN, SIGTTOU信号后停止运行运行）。

`ps`标识进程的5种状态码：

- **D** 不可中断 uninterruptible sleep (usually IO)；
- **R** 运行 runnable (on run queue)；
- **S** 中断 sleeping；
- **T** 停止 traced or stopped；
- **Z** 僵死 a defunct (”zombie”) process。

## 命令参数

> - -A 列出所有的行程
> - -w 显示加宽可以显示较多的资讯
> - -au 显示较详细的资讯
> - -aux 显示所有包含其他使用者的行程
> - au(x) 输出格式 :
> - USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
> - USER: 行程拥有者
> - PID: pid
> - %CPU: 占用的 CPU 使用率
> - %MEM: 占用的记忆体使用率
> - VSZ: 占用的虚拟记忆体大小
> - RSS: 占用的记忆体大小
> - TTY: 终端的次要装置号码 (minor device number of tty)
> - STAT: 该行程的状态:
> - D: 无法中断的休眠状态 (通常 IO 的进程)
> - R: 正在执行中
> - S: 静止状态
> - T: 暂停执行
> - Z: 不存在但暂时无法消除
> - W: 没有足够的记忆体分页可分配
> - <: 高优先序的行程
> - N: 低优先序的行程
> - L: 有记忆体分页分配并锁在记忆体内 (实时系统或捱A I/O)
> - START: 行程开始时间
> - TIME: 执行的时间
> - COMMAND:所执行的指令输出列的含义
> - F 代表这个程序的旗标 (flag)， 4 代表使用者为 super user
> - S 代表这个程序的状态 (STAT)，关于各 STAT 的意义将在内文介绍
> - UID 程序被该 UID 所拥有
> - PID 进程的ID
> - PPID 则是其上级父程序的ID
> - C CPU 使用的资源百分比
> - PRI 这个是 Priority (优先执行序) 的缩写，详细后面介绍
> - NI 这个是 Nice 值，在下一小节我们会持续介绍
> - ADDR 这个是 kernel function，指出该程序在内存的那个部分。如果是个 running的程序，一般就是 “-“
> - SZ 使用掉的内存大小
> - WCHAN 目前这个程序是否正在运作当中，若为 - 表示正在运作
> - TTY 登入者的终端机位置
> - TIME 使用掉的 CPU 时间。
> - CMD 所下达的指令为何

## 例子

> 1、ps -A 显示所有进程信息 
>
> 2、ps -u root 显示指定用户信息
>
> 3、ps-ef 显示所有的进程和命令行，常用组合是 ps -ef|grep tail
>
> 4、ps aux 列出目前所有的正在内存当中的程序
>
> 5、ps -axjf 列出类似程序树的程序显示
>
> 6、ps aux | egrep '(cron|syslog)' 找出与 cron 与 syslog 这两个服务有关的 PID 号码

更多例子见[10个重要的Linux ps命令实战](https://linux.cn/article-4743-1.html)。

## 参考

- [ps 进程查看器](https://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/ps.html)
- [linux的ps命令详解](https://www.jianshu.com/p/e75dc4b6ae8d)
- [Linux ps命令](https://www.runoob.com/linux/linux-comm-ps.html)
- [10个重要的Linux ps命令实战](https://linux.cn/article-4743-1.html)