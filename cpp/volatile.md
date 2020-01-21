# volatile的作用

`volatile`关键字是一种类型修饰符，用它声明的类型变量表示可以被某些编译器未知的因素更改。比如：操作系统、硬件或者其它线程等。

- `volatile`定义变量的值是易变的，每次用到这个变量的值的时候都要去重新读取这个变量的值，而不是读寄存器内的备份。
- 多线程中被几个任务共享的变量需要定义为`volatile`类型，***防止优化编译器把变量从内存装入CPU寄存器中***。

# volatile与多线程

`volatile` 不能解决多线程中的问题。

`volatile` 只在三种场合下是合适的。

- 和信号处理（signal handler）相关的场合；
- 和内存映射硬件（memory mapped hardware）相关的场合；
- 和非本地跳转（`setjmp` 和 `longjmp`）相关的场合。

# 替代volatile

- 原子操作
- 互斥量和条件变量

# 参考

- [谈谈 C/C++ 中的 volatile](https://liam.page/2018/01/18/volatile-in-C-and-Cpp/)
