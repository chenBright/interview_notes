# top

`top`命令是 Linux下 常用的性能分析工具，能够实时显示系统中各个进程的资源占用状况。

```
$top
    top - 09:14:56 up 264 days, 20:56,  1 user,  load average: 0.02, 0.04, 0.00
    Tasks:  87 total,   1 running,  86 sleeping,   0 stopped,   0 zombie
    Cpu(s):  0.0%us,  0.2%sy,  0.0%ni, 99.7%id,  0.0%wa,  0.0%hi,  0.0%si,  0.2%st
    Mem:    377672k total,   322332k used,    55340k free,    32592k buffers
    Swap:   397308k total,    67192k used,   330116k free,    71900k cached
    PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
    1 root      20   0  2856  656  388 S  0.0  0.2   0:49.40 init
    2 root      20   0     0    0    0 S  0.0  0.0   0:00.00 kthreadd
    3 root      20   0     0    0    0 S  0.0  0.0   7:15.20 ksoftirqd/0
    4 root      RT   0     0    0    0 S  0.0  0.0   0:00.00 migration/0
```

- 第一行
  - 09:14:56 ： 系统当前时间
  - 264 days, 20:56 ： 系统开机到现在经过了多少时间
  - 1 users ： 当前2用户在线
  - load average: 0.02, 0.04, 0.00： 系统1分钟、5分钟、15分钟的CPU负载信息
- 第二行
  - Tasks：任务;
  - 87 total：很好理解，就是当前有87个任务，也就是87个进程。
  - 1 running：1个进程正在运行
  - 86 sleeping：86个进程睡眠
  - 0 stopped：停止的进程数
  - 0 zombie：僵死的进程数
- 第三行
  - Cpu(s)：表示这一行显示CPU总体信息
  - 0.0%us：用户态进程占用CPU时间百分比，不包含renice值为负的任务占用的CPU的时间。
  - 0.7%sy：内核占用CPU时间百分比
  - 0.0%ni：改变过优先级的进程占用CPU的百分比
  - 99.3%id：空闲CPU时间百分比
  - 0.0%wa：等待I/O的CPU时间百分比
  - 0.0%hi：CPU硬中断时间百分比
  - 0.0%si：CPU软中断时间百分比
  - 注：这里显示数据是所有cpu的平均值，如果想看每一个cpu的处理情况，按1即可；折叠，再次按1；
- 第四行
  - Men：内存的意思
  - 8175320kk total：物理内存总量
  - 8058868k used：使用的物理内存量
  - 116452k free：空闲的物理内存量
  - 283084k buffers：用作内核缓存的物理内存量
- 第五行
  - Swap：交换空间
  - 6881272k total：交换区总量
  - 4010444k used：使用的交换区量
  - 2870828k free：空闲的交换区量
  - 4336992k cached：缓冲交换区总量
- 进程信息
  - PID：进程的 ID
  - USER：进程所有者
  - PR：进程的优先级别，越小越优先被执行
  - NInice：值
  - VIRT：进程占用的虚拟内存
  - RES：进程占用的物理内存
  - SHR：进程使用的共享内存
  - S：进程的状态。S 表示休眠，R 表示正在运行，Z 表示僵死状态，N 表示该进程优先值为负数
  - %CPU：进程占用CPU的使用率
  - %MEM：进程使用的物理内存和总内存的百分比
  - TIME+：该进程启动后占用的总的 CPU 时间，即占用 CPU 使用时间的累加值。
  - COMMAND：进程启动命令名称

## 交互指令

```shell
- q：退出top命令
- <Space>：立即刷新
- s：设置刷新时间间隔
- c：显示命令完全模式
- t:：显示或隐藏进程和 CPU 状态信息
- m：显示或隐藏内存状态信息
- l：显示或隐藏 cuptime 信息
- f：增加或减少进程显示标志
- S：累计模式，会把已完成或退出的子进程占用的 CPU 时间累计到父进程的 MITE+
- P：按 %CPU 使用率排行
- T：按 MITE+ 排行
- M：按 %MEM 排行
- u：指定显示用户进程
- r：修改进程 renice 值
- k：kill 进程
- i：只显示正在运行的进程
- W：保存对 top 的设置到文件 ^/.toprc，下次启动将自动调用 toprc 文件的设置。
- h：帮助命令
- q：退出
```

