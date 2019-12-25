# 《深入探索C++对象模型》

本文档记录了 C++ 对象模型相关的知识。

## 笔记

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

