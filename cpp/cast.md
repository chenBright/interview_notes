# 类型转换

## C数据类型转换

### 隐式类型转换

- 赋值转换
- 运算类型转换

### 强制类型转换

强制类型转换的格式为：

```c++
(type_name) expression
```

## C++数据类型转换

### 隐式类型转换

C++ 包括 C 的隐式类型转换，除此之外，在 C++ 中，将某种类型的对象**拷贝**到另一种不同类型的对象中时就会发生隐式转型。比如返回值（函数声明的返回值与代码块实际返回值不同的情况下），按值传递异型参数等情况均会发生隐式类型转换。
例如：

```
class A {}; 
class B
{
    public: B (A a) {}
    public: B (int c, int d = 0);
    public: operator double() const; 
};

A a;
B b1 = a;
B b2 = 10;
B b3;
double d;
d = 10 + b3;
```

此外 C++ 也不能在转换过程中进行连续的隐式类型转换。

可以通过关键字`explict`可以来规避可被单参调用的构造函数引起的隐式类型转换。

### 显示类型转换

C++语言也支持C语言的类型强制类型转换，但这种转换可以在两种不同类型的对象的指针之间进行，这很可能是相当危险的事情。所以 C++ 提供四种转换操作符来细分显式类型转换。

#### static_cast

- `static_cast` 作用和**C语言风格强制转换**的效果基本一样，由于**没有运行时类型检查来保证转换的安全性**，所以这类型的强制转换和C语言风格的强制转换都有安全隐患；

- 用于**类层次结构**中基类（父类）和派生类（子类）之间指针或引用的转换。注意：进行上行转换（把派生类的指针或引用转换成基类表示）是安全的；进行下行转换（把基类指针或引用转换成派生类表示）时，由于没有动态类型检查，所以是不安全的。

- 用于基本数据类型之间的转换，如把`int`转换成`char`，把`int`转换成`enum`。这种转换的安全性需要开发者来维护。

- `static_cast`不能转换掉原有类型的`const`、`volatile`、或者 `__unaligned`属性。(前两种可以使用const_cast 来去除)

- c++  的任何的隐式转换都是使用 `static_cast` 来实现。

#### reinterpret_cast

`reinterpret_cast`运算符是用来处理无关类型之间的转换；它会产生一个新的值，这个值会有与原始参数（expressoin）有**完全相同的比特位**。

```c++
reinterpret_cast <new_type> (expression)
```

`new_type`必须是一个指针、引用、算术类型、函数指针或者成员指针。

`reinterpret_cast` 用来执行**低级转型**，如将执行一个 int 的指针强转为 int。其转换结果与编译平台息息相关，不具有可移植性，因此在一般的代码中不常见到它。

`reinterpret_cast` 常用的一个用途是转换函数指针类型，即可以将一种类型的函数指针转换为另一种类型的函数指针，但这种转换可能会导致不正确的结果。总之，`reinterpret_cast` 只用于底层代码，一般我们都用不到它，如果你的代码中使用到这种转型，务必明白自己在干什么。

IBM 的 C++ 指南告诉我们`reinterpret_cast`可以用在什么地方作为转换运算符：

- 从**指针类型**到一个**足够大的整数类型**

- 从**整数类型**或者**枚举类型**到**指针类型**

- 从一个**指向函数的指针**到另一个**不同类型的指向函数的指针**

- 从一个**指向对象的指针**到另一个**不同类型的指向对象的指针**

- 从一个**指向类函数成员的指针**到另一个**指向不同类型的函数成员的指针**

- 从一个**指向类数据成员的指针**到另一个**指向不同类型的数据成员的指针**


#### const_cast

- **常量指针**被转化成**非常量的指针**，并且仍然指向原来的对象；

- **常量引用**被转换成**非常量的引用**，并且仍然指向原来的对象；

- `const_cast`一般用于修改指针。如`const char *p`形式;

- `const_cast`也可以去除对象的`volatile`属性。

#### dynamic_cast

`dynamic_cast` 主要用来在继承体系中的**安全向下转型**。它能安全地将指向基类的指针转型为指向子类的指针或引用，并获知转型动作成功是否。

**dynamic_cast 只能用在指针和引用类型的转换中**，它是唯一进行**运行期**（runtime）检查的类型转换符，它的主要目的就是保证转换后的类型是一个完整类型（Complete type）。`dynamic_cast`在转换**指针类型**时，如果结果不是一个 Complete Type，它会返回`NULL`；`dynamic_cast`在转换**引用类型**时，如果结果不是一个 Complete Type，它会抛出`bad_cast`的异常。`dynamic_cast` 会动使用行时信息（RTTI）来进行类型安全检查，确定源指针所指的内容是否真的合适被目标指针接受。如果是不适合，那么`dynamic_cast`则会返回`null`。因此 `dynamic_cast` 存在一定的效率损失。

```c++
class CBase { };
class CDerived: public CBase { };

int main(){
    CBase b;
    CBase* pb;
    CDerived d;
    CDerived* pd;
    pb = dynamic_cast<CBase*>(&d);     // ok: derived-to-base
    pd = dynamic_cast<CDerived*>(&b);  // error: base-to-derived
}
```

上面的代码最后一行会出错（error: 'CBase' is not polymorphic），**因为dynamic_cast 只有在基类带有虚函数的情况下才允许将基类转换为子类**。

```c++
class CBase
{
    virtual void dummy() { }
};

class CDerived1 : public CBase {
    int a;
};

class CDerived2 : public CBase {
    int a;
    virtual void dummy() { }
};

int main()
{
    CBase *pb = new CBase;
    CDerived1* pd1 = dynamic_cast<CDerived1*>(pb); // pd1 为 NULL
    CDerived2* pd2 = dynamic_cast<CDerived2*>(pb); // pd2 不为 NULL
}
```

## 参考

- [类型转换](https://github.com/selfboot/CS_Offer/blob/master/C%2B%2B/Basic.md#%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2)
- [C++之类型转换详解](http://harlon.org/2018/03/24/cpluscplustypeswitch/)