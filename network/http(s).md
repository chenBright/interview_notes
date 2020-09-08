# HTTP(S)

HTTP(S)相关知识。

## 状态码

### 1XX

表示临时响应并需要请求者继续执行操作的状态代码。
指定客户端应相应的某些动作，代表请求已被接受，需要继续处理。由于 HTTP/1.0 协议中没有定义任何 1xx 状态码，所以除非在某些试验条件下，服务器禁止向此类客户端发送 1xx 响应。

### 2XX

表示成功处理了请求的状态代码。
代表请求已成功被服务器接收、理解、并接受。这系列中最常见的有200、201状态码。

### 3XX

表示要完成请求，需要进一步操作。 通常，这些状态代码用来重定向。
代表需要客户端采取进一步的操作才能完成请求，这些状态码用来重定向，后续的请求地址（重定向目标）在本次响应的 Location 域中指明。这系列中最常见的有301、302状态码。

### 4XX

这些状态代码表示请求可能出错，妨碍了服务器的处理。
表示请求错误。代表了**客户端**看起来可能发生了错误，妨碍了服务器的处理。常见有：401、404状态码。

### 5XX

这些状态代码表示服务器在尝试处理请求时发生内部错误。 这些错误可能是服务器本身的错误，而不是请求出错。
代表了**服务器**在处理请求的过程中有错误或者异常状态发生，也有可能是服务器意识到以当前的软硬件资源无法完成对请求的处理。常见有500、503状态码。  

- [HTTP状态码](https://www.nowcoder.com/discuss/385142?type=post&order=time&pos=&page=0)

## 缓存

`Cache-Control`中的`max-age`指令用于指定**缓存过期的相对时间**。资源达到指定时间后过期。该功能与`Expires`类似。但其**优先级高于Expires**，如果同时设置`max-age`和`Expires`，`max-age`生效，忽略`Expires`。

- [一文了解HTTP中的缓存](https://juejin.im/post/5dbf8438f265da4d4f65ba99#heading-12)
- [浏览器缓存详解:expires,cache-control,last-modified,etag详细说明](https://blog.csdn.net/eroswang/article/details/8302191)

## Transfer-Encoding

- [HTTP 协议中的 Transfer-Encoding](https://imququ.com/post/transfer-encoding-header-in-http.html)

## HTTPS

- [HTTPS的建立流程](https://segmentfault.com/a/1190000000476876)
- [https建立连接的过程](https://juejin.im/post/5dcca1ab518825598c0f253d)
- [深入理解HTTPS工作原理](https://github.com/ljianshu/Blog/issues/50#)
- [图解SSL/TLS协议](https://www.ruanyifeng.com/blog/2014/09/illustration-ssl.html)
- [HTTPS篇之SSL握手过程详解](https://razeencheng.com/post/ssl-handshake-detail)
- [为什么 HTTPS 需要 7 次握手以及 9 倍时延](https://draveness.me/whys-the-design-https-latency)
- [SSL/TLS协议运行机制的概述](https://www.ruanyifeng.com/blog/2014/02/ssl_tls.html)
- [HTTPS劫持](https://bloodzer0.github.io/ossa/red_vs_blue/hijack_https/)
- [HTTPS中间人攻击及防御](https://elliotsomething.github.io/2016/12/22/HTTPS%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E5%87%BB%E5%8F%8A%E9%98%B2%E5%BE%A1/)
- [数字签名是什么？](http://www.ruanyifeng.com/blog/2011/08/what_is_a_digital_signature.html)

## HTTP2.0

- [HTTP,HTTP2.0,SPDY,HTTPS 你应该知道的一些事](http://www.alloyteam.com/2016/07/httphttp2-0spdyhttps-reading-this-is-enough/)
- [HTTP2和HTTPS来不来了解一下？](https://www.cnblogs.com/Java3y/p/9392349.html)
- [HTTP2 Server Push 的研究](http://www.alloyteam.com/2017/01/http2-server-push-research/)
- [当我们在谈论HTTP队头阻塞时，我们在谈论什么？](https://liudanking.com/arch/what-is-head-of-line-blocking-http2-quic/)
- [解密 HTTP/2 与 HTTP/3 的新特性](https://www.infoq.cn/article/kU4OkqR8vH123a8dLCCJ)

## 请求方法

### options

- [什么时候会发送options请求](https://juejin.im/post/5cb3eedcf265da038f7734c4)

### head

**HTTP `HEAD` 方法** 请求资源的头部信息, 并且这些头部与 HTTP [`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 方法请求时返回的一致. 该请求方法的一个使用场景是在下载一个大文件前先获取其大小再决定是否要下载, 以此可以节约带宽资源.

`HEAD` 方法的响应不应包含响应正文. 即使包含了正文也必须忽略掉. 虽然描述正文信息的 [entity headers](https://developer.mozilla.org/zh-CN/docs/Glossary/Entity_header), 例如 [`Content-Length`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Length) 可能会包含在响应中, 但它们并不是用来描述 `HEAD` 响应本身的, 而是用来描述同样情况下的 [`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 请求应该返回的响应.

如果 `HEAD` 请求的结果显示在上一次 [`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 请求后缓存的资源已经过期了, 那么该缓存会失效, 即使 `GET` 请求已经完成.

## 幂等性

- [微服务架构之幂等性问题及设计思想，你不得不知的一些幂等方案](https://juejin.im/post/6844903880615002125)

- [如何保证业务的幂等性](https://gongfukangee.github.io/2019/03/25/Idempotence/)
- [分布式系统互斥性与幂等性问题的分析与解决](https://tech.meituan.com/2016/09/29/distributed-system-mutually-exclusive-idempotence-cerberus-gtis.html)

