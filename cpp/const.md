# const 

## 作用

- 定义常量

  常量的值是不能被更新的。

- 类型检查

  `const`常量与`#define`宏定义常量的区别：

  - `const`常量具有类型，编译器可以进行**类型检查**；
  - `#define`宏定义没有数据类型，只是简单的字符串替换，不能进行类型检查。

- 可以节省空间，避免不必要的内存分配。

  `const`定义常量从汇编的角度来看，只是给出了对应的**内存地址**，而不是像`#define`一样给出的是**立即数**，所以，`const`定义的常量在程序运行过程中只有一份拷贝，而`#define`定义的常量在内存中有若干个拷贝。

## const对象默认为文件局部变量

非const变量默认为extern。

要使const变量能够在其他文件中访问，必须在文件中显式地指定它为extern。

## const与指针

原则：从头到左结合，const“修饰”位于它右边的类型或变量。

参考[Github]([https://github.com/Light-City/CPlusPlusThings/tree/master/basic_content/const#5%E6%8C%87%E9%92%88%E4%B8%8Econst](https://github.com/Light-City/CPlusPlusThings/tree/master/basic_content/const#5指针与const))。

## const成员变量

参考[C++ Static关键字的用法说明](http://guozet.me/post/C++-Static-keyword/)。

### 常量

- 只能通过`初始化列表`初始化
- 不能在类内进行初始化
- 不能在构造函数中初始化
- 不能在类外初始化

### 静态整型常量

- （整型）能否在类中初始化，取决于编译器
- 能在在类外初始化，不能带`static`

### 静态非整型常量

- (double型）能否在类中初始化，取决于编译器
- 能在在类外初始化，不能带`static`

##  C++里的静态成员函数不能用const的原因

[C++里的静态成员函数不能用const的原因](https://www.cnblogs.com/guxuanqing/p/4903154.html)：

> C++的规则：const 修饰符用于表示函数不能修改成员变量的值，该函数必须是含有 this 指针的类成员函数。函数调用方式为 thiscall，而类中的 static 函数本质上是全局函数，调用规约是 __cdecl 或 __stdcall，不能用const 来修饰它。一个静态成员函数访问的值是其参数、静态数据成员和全局变量，而这些数据都不是对象状态的一部分。而对成员函数中使用关键字 const 是表明：函数不会修改该函数访问的目标对象的数据成员。既然一个静态成员函数根本不访问非静态数据成员，那么就没必要使用const了。

总结：因为常函数是操作成员变量的，而静态函数没有成员变量可说。

# 参考

- [Github](https://github.com/Light-City/CPlusPlusThings/blob/master/basic_content/const/README.md)
- [C++ Static关键字的用法说明](http://guozet.me/post/C++-Static-keyword/)