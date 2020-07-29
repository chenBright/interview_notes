# union

C++11 标准规定，**任何非引用类型**都可以成为联合体的数据成员，这种联合体也被称为非受限联合体。

C++11中会自动删除非POD成员类型中拥有的非平凡的构造函数，包括默认构造、拷贝构造、赋值构造、析构函数都会被删除。

- [C++11非受限联合体（union](http://c.biancheng.net/view/7165.html)
- [《深入理解C++11》笔记–非受限联合体](https://blog.csdn.net/WizardtoH/article/details/80792915)