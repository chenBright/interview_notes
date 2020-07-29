# DDos

`DDoS`(Distributed Denial of Service)即分布式拒绝服务攻击，是目前最为强大、最难以防御的攻击方式之一。

## 方式

[维基百科](https://zh.wikipedia.org/wiki/%E9%98%BB%E6%96%B7%E6%9C%8D%E5%8B%99%E6%94%BB%E6%93%8A#%E6%94%BB%E5%87%BB%E6%96%B9%E5%BC%8F)上有更详细的分类介绍。

1. SYN Flood
2. DNS Query Flood
3. ICMP flood
4. CC ( Challenge Collapsar)攻击

## 防御DDos的方法

- 添加防火墙规则
- 加大带宽
- 增加服务器
- 使用CDNA技术
- 高防服务器
- 带流量清洗的ISP
- 流量清洗服务等
- 前端自带的防御功能

## SYN Flood

- [什么是SYN Flood攻击？](https://zhuanlan.zhihu.com/p/29539671)
- [TCP处理新建连接的逻辑](https://github.com/hustcat/hustcat.github.io/blob/master/_posts/2017-03-03-tcp_syn_cookies_and_window_size.md#tcp%E5%A4%84%E7%90%86%E6%96%B0%E5%BB%BA%E8%BF%9E%E6%8E%A5%E7%9A%84%E9%80%BB%E8%BE%91)
- [SYN cookies 机制下连接的建立](https://yq.aliyun.com/articles/42391)
- [快速穷举TCP连接欺骗攻击-利用SYN Cookies](http://www.91ri.org/7075.html)

## 参考

- [常见的DDos攻击手段及解决手段](https://juejin.im/post/5d68aaa76fb9a06ad4515a50)
- [DDos攻击解决办法](https://www.cnblogs.com/diantong/p/11438756.html)
- [浅谈DDos攻击与防御](https://thief.one/2017/05/10/1/)