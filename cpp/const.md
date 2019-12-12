# const 的作用

- 定义常量

  常量的值是不能被更新的。

- 类型检查

  `const`常量与`#define`宏定义常量的区别：

  - `const`常量具有类型，编译器可以进行***类型检查***；
  - `#define`宏定义没有数据类型，只是简单的字符串替换，不能进行类型检查。

- 可以节省空间，避免不必要的内存分配。

  `const`定义常量从汇编的角度来看，只是给出了对应的***内存地址***，而不是像`#define`一样给出的是***立即数***，所以，`const`定义的常量在程序运行过程中只有一份拷贝，而`#define`定义的常量在内存中有若干个拷贝。

# const对象默认为文件局部变量

非const变量默认为extern。

要使const变量能够在其他文件中访问，必须在文件中显式地指定它为extern。

# const与指针

原则：从头到左结合，const“修饰”位于它右边的类型或变量。

参考[Github]([https://github.com/Light-City/CPlusPlusThings/tree/master/basic_content/const#5%E6%8C%87%E9%92%88%E4%B8%8Econst](https://github.com/Light-City/CPlusPlusThings/tree/master/basic_content/const#5指针与const))。

# 函数中使用const

# 参考

- [Github](https://github.com/Light-City/CPlusPlusThings/blob/master/basic_content/const/README.md)