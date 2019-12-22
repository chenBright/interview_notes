# 函数绑定

## bind

- [std::bind 原理简单图解](https://blog.csdn.net/xiexievv/article/details/50517964)
- [std::bind技术内幕](https://www.cnblogs.com/qicosmos/p/3723388.html)
- [std::bind 的实现原理](http://blog.guorongfei.com/2017/01/27/bind-implementation/)

## function

- [STL 源码简析 – std::function (上)](https://xr1s.me/2019/01/15/stl-source-analyze-std-function-1/)
- [STL 源码简析 – std::function (下)](https://xr1s.me/2019/02/25/stl-source-analyze-std-function-2/)

## placeholder

```c++
template <int I>
struct placeholder {
};

// 特化
placeholder<1>  _1; 
placeholder<2>  _2; 
placeholder<3>  _3; 
placeholder<4>  _4; 
placeholder<5>  _5; 
placeholder<6>  _6; 
placeholder<7>  _7;
placeholder<8>  _8; 
placeholder<9>  _9; 
placeholder<10> _10;
...
```

