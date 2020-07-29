# C++对象模型

本文档记录了 C++ 对象模型相关的知识。

## 《深入探索C++对象模型》笔记

- [Github](https://github.com/Making-It/note/blob/d38bfe1ef7266b024b215f7935d643b9d4cc0e0f/C++/C++%E5%AF%B9%E8%B1%A1%E6%A8%A1%E5%9E%8B.md)
- [Adoo's blog](http://www.roading.org/develop/cpp/%E3%80%8A%E6%B7%B1%E5%BA%A6%E6%8E%A2%E7%B4%A2c%E5%AF%B9%E8%B1%A1%E6%A8%A1%E5%9E%8B%E3%80%8B%E7%AC%94%E8%AE%B0%E6%B1%87%E6%80%BB.html)

- [“斑驳”的博客](https://yanchunan.github.io/post/c++/%E6%B7%B1%E5%BA%A6%E6%8E%A2%E7%B4%A2c++%E5%AF%B9%E8%B1%A1%E6%A8%A1%E5%9E%8B/)
- [Hunter Yuan 的博客](https://clodfisher.github.io/2018/08/InsideTheC++ObjectModel/)

## 空类

### 对象大小

C++ 标准规定，不允许一个对象（当然包括类对象）的大小为 0，不同的对象不能具有相同的地址。这是由于：

- `new`需要分配不同的内存地址，不能分配内存大小为 0 的空间；
- 避免除以`sizeof(T)`时得到除以 0 错误。

故使用**一个字节**来区分空类。

### 默认函数

- 默认构造函数
- 默认析构函数
- 默认拷贝构造函数
- 默认赋值运算符
- 取址运算符
- `const`版本的取址运算符

## 只能在堆上（栈上）生成对象的类

### 只能在堆上

方法：将析构函数设置为私有。

原因：C++ 是静态绑定语言，编译器管理栈上对象的生命周期，编译器在为类对象分配栈空间时，会先检查类的析构函数的访问性。若析构函数不可访问，则不能在栈上创建对象。

### 只能在栈上

方法：将`operator new()`和`operator delete()`重载为私有。

原因：在堆上生成对象，使用`new operator`关键词操作，其过程分为两阶段：第一阶段，使用`operator new()`在堆上寻找可用内存，分配给对象；第二阶段，调用`placement new()`构造对象。将`operator new()`操作设置为私有，那么第一阶段就无法完成，就不能够在堆上生成对象。

## 如何得到一个结构体内成员的偏移量？

```c++
#define offset(TYPE, MEMBER) ((size_t)&((TYPE*)0)->MEMBER)
```

## 不能继承的类

- [C++中定义一个不能被继承的类](https://blog.csdn.net/u012501459/article/details/48129327)
- [用C++设计一个不能被继承的类](https://www.jianshu.com/p/b8a5a8c2cec7)
- [C++ 不给力之不可继承](http://zhuoqiang.me/ungelivable-cpp-those-who-cannot-inherit.html)

## lambda表达式原理

- [C++ lambda 内 std::move 失效问题的思考](https://www.cyhone.com/articles/why-move-no-work-in-lambda/)
- [C++ Lambda 编译器实现原理](https://hacpai.com/article/1545790136502)

