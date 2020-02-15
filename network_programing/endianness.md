# 字节序

字节存储顺序主要分为`大端序`（Big-endian）和`小端序`（Little-endian），区别如下：

- 大端模式：是指数据的**高字节**保存在内存的**低地址**中，而数据的**低字节**保存在内存的**高地址**中；
- 小端模式，是指数据的**高字节**保存在内存的**高地址**中，而数据的**低字节**保存在内存的**低地址**中。

假如有一个4字节的数据为` 0x12 34 56 78`（十进制：`305419896`，`0x12`为高字节，`0x78`为低字节），若将其存放于地址` 0x4000 8000`中，则有：

| 内存地址 | 0x4000 8000（低地址） | 0x4000 8001 | 0x4000 8002 | 0x4000 8003（高地址） |
| -------- | --------------------- | ----------- | ----------- | --------------------- |
| 大端模式 | `0x12（高字节）`      | `0x34`      | `0x56`      | `0x78（低字节）`      |
| 小端模式 | `0x78（低字节）`      | `0x56`      | `0x34`      | `0x12（高字节）`      |

## 特点

**大端模式优点**：**符号**位在所表示的数据的内存的第一个字节中，便于快速判断数据的正负和大小。

**小端模式优点**：

1. 内存的低地址处存放低字节，所以在**强制转换数据时不需要调整字节的内容**（注解：比如把int的4字节强制转换成short的2字节时，就直接把int数据存储的前两个字节给short就行，因为其前两个字节刚好就是最低的两个字节，符合转换逻辑）；
2. CPU 做数值运算时从内存中依顺序依次从低位到高位取数据进行运算，直到最后刷新最高位的符号位，这样的**运算方式会更高效**。

其各自的优点就是对方的缺点，正因为两者彼此不分伯仲，再加上一些硬件厂商的坚持（见1.3节），因此在多字节存储顺序上始终没有一个统一的标准。

## 判断

1. 通过将多字节数据强制类型转换成单字节数据，再通过判断起始存储位置是数据高字节还是低字节进行检测：

```c++
// 大端，返回 true; 小端，返回 false。
bool IsBigEndian_1() {
    int nNum = 0x12345678;
    char cLowAddressValue = *(char*)&nNum;

    // 低地址处是高字节，则为大端
    if ( cLowAddressValue == 0x12 ) {
      return true;
    }

    return false; 
}
```

2. 利用联合体union的存放顺序是所有成员都从低地址开始存放这一特性进行检测：

```c++
// 大端，返回 true; 小端，返回 false。
bool isBigEndian_2() {
    union uendian {
       int nNum;
       char cLowAddressValue;
    };

    uendian u;
    u.nNum = 0x12345678;
    
    // 相当于取低地址的一个字节
    if ( u.cLowAddressValue == 0x12 ) {
      return true;
    }

    return false;
}
```

## 转换

### 大小端转换

```c++
// 实现16bit的数据之间的大小端转换
#define BLSWITCH16(A)   (  ( ( (uint16)(A) & 0xff00 ) >> 8  )    | \  
                           ( ( (uint16)(A) & 0x00ff ) << 8  )     )  

// 实现32bit的数据之间的大小端转换
#define BLSWITCH32(A)   (  ( ( (uint32)(A) & 0xff000000) >> 24) |\
         (((uint32)(A) & 0x00ff0000) >> 8) | \
         (((unit32)(A) & 0x0000ff00) << 8) | \
         (((uint32)(A) & 0x000000ff) << 32)  )
```

### 网络字节序与主机字节序的转换

由于**网络字节序**一律为**大端**，而目前个人PC大部分都是X86的小端模式，因此网络编程中不可避免得要进行网络字节序和主机字节序之间的相互转换，下面是 socket 提供的转换函数

```c++
#define ntohs(n)     // 16位数据类型网络字节顺序到主机字节顺序的转换  
#define htons(n)     // 16位数据类型主机字节顺序到网络字节顺序的转换  
#define ntohl(n)     // 32位数据类型网络字节顺序到主机字节顺序的转换  
#define htonl(n)     // 32位数据类型主机字节顺序到网络字节顺序的转换
```

## 参考

- [大小端存储模式精解](https://jocent.me/2017/07/25/big-little-endian.html)
- [如何判断CPU是大端还是小端模式]([https://itimetraveler.github.io/2018/01/18/%E5%A6%82%E4%BD%95%E5%88%A4%E6%96%ADCPU%E6%98%AF%E5%A4%A7%E7%AB%AF%E8%BF%98%E6%98%AF%E5%B0%8F%E7%AB%AF%E6%A8%A1%E5%BC%8F/](https://itimetraveler.github.io/2018/01/18/如何判断CPU是大端还是小端模式/))