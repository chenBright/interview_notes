# NULL、nullptr的区别

在 C 语言中，常常用`NULL`作为指针变量的初始值，而在 C++ 中，则建议使用`nullptr`。

## NULL

在 C 语言中，`NULL`的定义通常如下：

```c++
#define NULL ((void*)0)
```

但 C++ 中的定义却是：

```c++
#define NULL 0
```

在`stddef.h`头文件中的完整定义如下：

```c++
#undef NULL
#if defined(__cplusplus)
#define NULL 0
#else
#define NULL ((void *)0)
#endif
```

对的，只是一个整形数字 0。把它当成空指针只是一个无可奈何的选择罢了。因为C++中不能将`void*`类型的指针**隐式转换**成其他指针类型，所以不能将NULL定义为`(void*)0`。

## nullptr

`nullptr`并非整型类别，甚至也不是指针类型，但是能转换成任意指针类型。`nullptr`的实际类型是[std:nullptr_t](https://zh.cppreference.com/w/cpp/types/nullptr_t)。

## 参考

- [为什么建议你用nullptr而不是NULL](https://zhuanlan.zhihu.com/p/79883965)
- 

