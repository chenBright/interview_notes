# tcpdump

- **-i** 指定要捕获的目标网卡名，网卡名可以使用前面章节中介绍的 **ifconfig** 命令获得；如果要抓所有网卡的上的包，可以使用 **any** 关键字。
- **-X** 以 ASCII 和十六进制的形式输出捕获的数据包内容，减去链路层的包头信息；**-XX** 以 ASCII 和十六进制的形式输出捕获的数据包内容，包括链路层的包头信息。
- **-n** 不要将 ip 地址显示成别名的形式；**-nn** 不要将 ip 地址和端口以别名的形式显示。
- **-S** 以绝对值显示包的 ISN 号（包序列号），默认以上一包的偏移量显示。
- **-vv** 抓包的信息详细地显示；**-vvv** 抓包的信息更详细地显示。
- **-w** 将抓取的包的原始信息（不解析，也不输出）写入文件中，后跟文件名。
- **-r** 从利用 **-w** 选项保存的包文件中读取数据包信息。

除了可以使用选项以外，**tcpdump** 还支持各种数据包过滤的表达式，常见的形式如下：

```
## 仅显示经过端口 8888 上的数据包（包括tcp:8888和udp:8888）
tcpdump -i any 'port 8888'

## 仅显示经过端口是 tcp:8888 上的数据包
tcpdump -i any 'tcp port 8888'

## 仅显示从源端口是 tcp:8888 的数据包
tcpdump -i any 'tcp src port 8888'

## 仅显示源端口是 tcp:8888 或目标端口是 udp:9999 的包 
tcpdump -i any 'tcp src port 8888 or udp dst port 9999'

## 仅显示地址是127.0.0.1 且源端口是 tcp:9999 的包 ，以 ASCII 和十六进制显示详细输出，
## 不显示 ip 地址和端口号的别名
tcpdump -i any 'src host 127.0.0.1 and tcp src port 9999' -XX -nn -vv
```

## 参考

- [Linux tcpdump 使用介绍](https://mp.weixin.qq.com/s/0eKYNcm5YS8hgQlpSTsjhA)