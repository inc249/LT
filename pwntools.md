# pwntools

## 连接

本地process()、远程remote()。对于remote函数可以接url并且指定端口。

## IO模块

下面给出了PwnTools中的主要IO函数。这个比较容易跟zio搞混，记住zio是read、write，pwn是recv、send就可以了。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
send(data) : 发送数据
sendline(data) : 发送一行数据，相当于在末尾加\n

recv(numb=4096, timeout=default) : 给出接收字节数,timeout指定超时
recvuntil(delims, drop=False) : 接收到delims的pattern
（以下可以看作until的特例）
recvline(keepends=True) : 接收到\n，keepends指定保留\n
recvall() : 接收到EOF
recvrepeat(timeout=default) : 接收到EOF或timeout

interactive() : 与shell交互
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

## ELF模块

ELF模块用于获取ELF文件的信息，首先使用ELF()获取这个文件的句柄，然后使用这个句柄调用函数，和IO模块很相似。

下面演示了：获取基地址、获取函数地址（基于符号）、获取函数got地址、获取函数plt地址

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
>>> e = ELF('/bin/cat')
>>> print hex(e.address)  # 文件装载的基地址
0x400000
>>> print hex(e.symbols['write']) # 函数地址
0x401680
>>> print hex(e.got['write']) # GOT表的地址
0x60b070
>>> print hex(e.plt['write']) # PLT的地址
0x401680
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

## 数据处理

主要是对整数进行打包，就是转换成二进制的形式，比如转换成地址。p32、p64是打包，u32、u64是解包。