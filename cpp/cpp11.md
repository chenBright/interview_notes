# 我平时用到的C++11特性

- auto
- nullptr
- for-each
- 构造函数
  - 委托构造：在同一个类中一个构造函数调用另一个构造函数。
  - 继承构造：如果派生类想要使用基类的构造函数，需要在构造 函数中显式声明。
- 右值、移动语义、移动构造函数 
- std::forward、std::move
- stl容器新增的 emplace_back 方法
- std::array
- 无序容器：std::unordered_map/std::unordered_multimap、std::unordered_set/std::unordered_multiset
- 元组 std::tuple
- Initializer_list（初始化列表）
- std::chrono 库
- 正则表达式
- 智能指针系列（std::shared_ptr/std::unique_ptr/std::weak_ptr）
- 线程库std::thread、线程同步技术库 std::mutex/std::condition_variable/std::lock_guard等
- lamda表达式、std::bind/std::function 库
- 其他的就是一些关键字的用法(override、final、delete)
- using 代替 typedef

- [C++11 中值得关注的几大变化（详解）](https://coolshell.cn/articles/5265.html)