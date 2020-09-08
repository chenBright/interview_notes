# inode

Inode 记录的信息：

- 文件的字节数
- 设备 ID（储存文件的设备ID）
- 文件拥有者的User ID
- 文件的 Group ID
- 文件的读、写、执行权限
- 额外的系统和用户标志位：限制文件的使用和修改来保护文件
- 文件的时间戳，共有三个：ctime 指 inode 上一次变动的时间，mtime 指文件内容上一次变动的时间，atime 指文件上一次打开的时间.
- 链接数：有多少文件名指向这个 inode（硬连接）
- 文件块指针：指向文件数据 block 的位置

参考：

- [关于 inode](https://www.ibm.com/developerworks/cn/aix/library/au-speakingunix14/index.html)

## 文件

- [Linux 为什么多进程能够读写正在删除的文件](https://www.jianshu.com/p/fda6526aad1b)