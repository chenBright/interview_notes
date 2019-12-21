# 虚函数

本文档记录了虚函数相关的知识。

## 静态绑定与动态绑定

**绑定**：函数调用与函数本身的关联，以及成员访问与变量内存地址间的关系。 

- **静态绑定**：指在程序编译过程中，把函数调用与响应调用所需的代码结合的过程，称为静态绑定。发生在编译期。

- **动态绑定**：指在执行期间判断所引用对象的实际类型，根据实际的类型调用其相应的方法。程序运行过程中，把函数调用与响应调用所需的代码相结合的过程称为动态绑定。发生于运行期。

## 通过虚函数表调用虚函数

在 C++ 中**动态绑定是通过虚函数实现的，是多态实现的具体形式**。而虚函数是通过虚函数表实现的。这个表中记录了虚函数的地址，解决继承、覆盖的问题，保证动态绑定时能够根据对象的实际类型调用正确的函数。

C++标准规格说明书中说到，**编译器必须要保证虚函数表的指针存在于对象实例中最前面的位置（这是为了保证正确取到虚函数的偏移量）**。也就是说，我们可以通过对象实例的地址得到这张虚函数表，然后可以遍历其中的函数指针，并调用相应的函数。

虚函数表不一定是存在最开头，但是目前各个编译器大多是这样设置的。

[C++中虚函数、虚继承内存模型](https://zhuanlan.zhihu.com/p/41309205)详细介绍了 C++ 虚函数、虚继承内存模型。

代码（来自[通过虚函数表访问私有函数](https://liam.page/2018/01/23/crack-private-member-function-by-vtable/)）如下：

```c++
#include <stddef.h>
#include <iostream>

class Base {
 public:
  virtual void f() {
    std::cout << "Your are calling Base::f (public)." << std::endl;
  }

 private:
  virtual void g() {
    std::cout << "Your are calling Base::g (private)." << std::endl;
  }
};

class Derived : public Base{};

using funcptr_t = void(*)(void);
using ptr_t     = uint64_t*;

funcptr_t fuckcxx(Base* const ptr, const ptrdiff_t offset) {
  ptr_t pvtbl = reinterpret_cast<ptr_t>(ptr);               // 1.
  ptr_t pfunc = reinterpret_cast<ptr_t>(*pvtbl);
  return reinterpret_cast<funcptr_t>(*(pfunc + offset));
}

int main() {
    Derived d;
    auto f = fuckcxx(&d, 0);
    auto g = fuckcxx(&d, 1);                                // 2.
    f(); g();
    return 0;
}
```

运行结果：

```shell
Your are calling Base::f (public).
Your are calling Base::g (private).
```

[C++虚函数表原理浅析](https://www.cnblogs.com/zhxmdefj/p/11594459.html)这篇文章详细解析了通过虚函数表访问虚函数的方法。

## 虚函数、构造函数、析构函数

### 构造函数不能是虚函数

- **存储空间角度**：如果构造函数是虚函数，就需要通过虚函数表来调用。可是对象就是通过构造函数来实例化的，实例化之前尚没有内存空间，也没有虚函数表。
- **使用角度**：虚函数主要用于在信息不全的情况下，能使重载的函数得到对应的调用，特别允许调用一个只知道接口而不知道其准确对象类型的函数。而构造函数本身就是要初始化对象，势必要知道对象的准确类型。
- **作用**：虚函数的作用在于通过基类的指针或引用来调用它的时候能够变成调用派生类的那个成员函数，而构造函数是在创建对象时自动调用的，不可能通过基类的指针或者引用去调用。

### 析构函数常常是虚函数

基类的指针或引用通常会指向基类或派生类对象，如果基类的析构函数不是虚函数，在通过删除指针或引用来释放对象时，**只会调用基类的析构函数，而不会调用派生类的析构函数，从而导致内存泄漏**。

## 构造函数和析构函数中的虚函数调用

一个类的虚函数在它自己的构造函数和析构函数中被调用的时候，它们就变成普通函数了，不“虚”了。也就是说不能在构造函数和析构函数中让自己“多态”。

当构造函数内部有虚函数时，只调用自己类中的虚函数，原因是调用时还没有派生类版本的信息。

当析构函数内部有虚函数时，与构造函数相同，只有“局部”的版本被调用，原因是因为派生类版本的信息已经不可靠了。由于析构函数的调用顺序与构造函数相反，是从派生类的析构函数到基类的析构函数。当某个类的析构函数被调用时，派生自该类的类的析构函数已经被调用了，相应的数据也已丢失，如果再调用虚函数的最后一级的版本，就相当于对一些不可靠的数据进行操作，这是非常危险的。因此，在析构函数中，虚函数机制也是不起作用的。

详细可参考《Effective C++》item 9，简洁介绍见[Effective C++ 9：在析构/构造时不要调用虚函数](https://harttle.land/2015/07/27/effective-cpp-9.html)。

## 虚函数与访问限定符

多态的实现与访问限定符没有任何关系，访问限定符只是控制类的成员对外部的可见性，但不限制多态。**通过基类指针或引用调用成员函数时，如果该函数时非虚的，那么将采用静态绑定，即编译时绑定；如果该函数是虚拟的，则采用动态绑定，即运行时绑定。**

## 参考

- [通过虚函数表访问私有函数](https://liam.page/2018/01/23/crack-private-member-function-by-vtable/)
- [C++虚函数表原理浅析](https://www.cnblogs.com/zhxmdefj/p/11594459.html)
- [C++ 虚函数表解析](https://coolshell.cn/articles/12165.html)
- [C++中虚函数、虚继承内存模型](https://zhuanlan.zhihu.com/p/41309205)
- [[转载]虚函数与构造函数、析构函数](https://www.jianshu.com/p/c26f1dc83b28)
- [Effective C++ 9：在析构/构造时不要调用虚函数](https://harttle.land/2015/07/27/effective-cpp-9.html)
- [纯虚函数能为private吗？](http://www.cppblog.com/zhuweisky/archive/2005/09/14/269.html)