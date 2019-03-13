---
title: 同步 I/O 多路复用之 select
date: 2019-03-12 19:33:32
tags: [linux,C++]
category: linux
---


`select()`  赋予你同时监控多个 sockets 的能力，他会告诉你哪些 sockets 可以读取，哪些可以写入，哪些触发了异常。

<!--more-->

## 使用

```c++
int select(int nfds,fd_set *readfds,fd_set *writeset,fd_set *exceptset,const struct timeval *timeout)

```

### nfds 

nfds 是所有加入集合的句柄值的最大那个值还要加1

比如我们创建了3个句柄：

```c++
int sa, sb, sc;
sa = socket(...); /* 分别创建3个句柄并连接到服务器上 */
connect(sa,...);
sb = socket(...);
connect(sb,...);
sc = socket(...);
connect(sc,...);
FD_SET(sa, &rdfds);/* 分别把3个句柄加入读监视集合里去 */
FD_SET(sb, &rdfds);
FD_SET(sc, &rdfds);
```

在使用 select 函数之前，一定要找到3个句柄中的最大值是哪个，我们一般定义一个变量来保存最大值，取得最大 socket 值如下：

```c++
int nfds = 0;
if(sa > maxfd)
	nfds = sa;
if(sb > maxfd)
	nfds = sb;
if(sc > maxfd)
	nfds = sc;
ret = select(maxfd + 1,...);
```

#### 为什么 +1？

[标准](http://pubs.opengroup.org/onlinepubs/9699919799/functions/pselect.html)上说：

> nfds 参数是指定要测试的描述符范围。第一个描述符每次都会被检查，也就是说，从0到 nfds - 1 的描述符会被检查。

`nfds` 需要的不是文件描述符数量，而是 watch 范围的上限。[3]  [5]

#### 为什么是最大的句柄？

在  ["Advanced Programming in the UNIX Environment"](http://www.apuebook.com/) 中 W. Richard Stevens 说这是为了性能优化。

####  设置 nfds 为0？

[man page](http://linux.die.net/man/2/select) 讲到

> 一些代码中调用 `select()` 三个 `set` 都设置为空，nfds 为 0，和一个非空的 timeout，用来实现一个可移植的亚秒级别的 sleep

在 `nanosleep` 广泛支持前，`select` 是唯一可移植实现亚秒 sleep 的方法。[4]

### readset，writeset，exceptset

这三个参数都是 `fd_set *` 类型。

即我们在程序里要申明几个 `fd_set` 类型的变量，比如 `rdfds`, `wtfds`, `exfds`，然后把这个变量的地址 `&rdfds`, `&wtfds`, `&exfds` 传递给 `select` 函数。

- readset：当句柄的状态变成可读的时系统就会告诉 `select` 函数返回
- writeset：句柄状态变成可写的时系统就会告诉`select` 函数返回
- exceptset：特殊情况，即句柄上有特殊情况发生时系统会告诉 select函数返回。特殊情况比如对方通过一个socket句柄发来了紧急数据。

`fd_set` 可以理解为一个集合，这个集合中放的是文件描述符，可以通过以下的宏进行设置：

```c++
void FD_ZERO(fd_set *fdset);          //清空集合
void FD_SET(int fd, fd_set *fdset);   //将一个给定的文件描述符加入集合之中
void FD_CLR(int fd, fd_set *fdset);   //将一个给定的文件描述符从集合中删除
int FD_ISSET(int fd, fd_set *fdset);  // 检查集合中指定的文件描述符是否可以读写 
```
一个常见的用法：
```c++
FD_ZERO(&readset);
FD_SET(fd,&readset);
select(fd+1,&readset,NULL,NULL,NULL);
if(FD_ISSET(fd,readset){……}
```



### timeval

timeout为结构timeval，用来设置 `select()` 的等待时间，其结构定义如下

```c++
struct timeval {
    long tv_sec;   //seconds
    long tv_usec;  //microseconds
};
```

这个参数有三种可能

1. 永远等待下去：仅在有一个描述字准备好I/O时才返回。为此，把该参数设置为空指针 `NULL`。
2. 等待一段固定时间：在有一个描述字准备好I/O时返回，但是不超过由该参数所指向的 timeval 结构中指定的秒数和微秒数。
3. 根本不等待：检查描述字后立即返回，这称为轮询。为此，该参数必须指向一个 timeval 结构，而且其中的定时器值必须为0。

### 返回值

执行成功则返回文件描述词状态已改变的个数，如果返回0代表在描述词状态改变前已超过 timeout 时间，当有错误发生时则返回-1，错误原因存于 errno，此时参数 `readfds`，`writefds`，`exceptfds` 和 timeout 的值变成不可预测

## 优点

目前几乎在支持所有平台，有良好的跨平台性。

##  缺点

- 每次调用 `select`，都需要把文件描述符集合从用户态拷贝到内核态，这个开销在fd很多时会很大
- 单个进程能监视的描述符存在最大限制，在 Linux 一般为1024，只能通过重新编译内核等复杂方式改变。
- 通过在内核遍历文件描述符来获取已经就绪的 socket。事实上，同时连接的大量客户端在一时刻可能只有很少的处于就绪状态，因此随着监视的描述符数量的增长，其效率也会线性下降。

## 下面是linux环境下select的一个简单用法

```c++
/*
** select.c -- a select() demo
*/
#include <stdio.h>
#include <sys/time.h>

#define STDIN 0 // standard input 的 file descriptor

int main() {
    struct timeval tv;
    fd_set readfds;

    tv.tv_sec = 5;
    tv.tv_usec = 0;

    FD_ZERO(&readfds);
    FD_SET(STDIN, &readfds);

    // 不用管 writefds 與 exceptfds：
    select(STDIN + 1, &readfds, NULL, NULL, &tv);

    if (FD_ISSET(STDIN, &readfds)) {
        printf("A key was pressed!\n");
    } else {
        printf("Time out.\n");
    }

    return 0;
}
```






参考资料：

[1]: http://www.cnblogs.com/Anker/archive/2013/08/14/3258674.html

[2]: http://jackxiang.com/post/3398/

[3]: https://unix.stackexchange.com/questions/7742/whats-the-purpose-of-the-first-argument-to-select-system-call
[4]: https://stackoverflow.com/questions/16767845/what-does-select-do-when-nfds-is-0
[5]: https://stackoverflow.com/questions/12025133/select-from-multiply-sockets-right-nfds-value?rq=1
