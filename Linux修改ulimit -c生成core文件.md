# Linux修改ulimit -c生成core文件 #

每个进程其实都有一组资源限制，而这些资源限制会随着当前进程的fork而继承下来。

shell本身是有一组资源限制的，如果你在某个shell下直接执行一条命令，那么这个新进程一般就继承了shell的资源限制。

今天工作中遇到一个不确定的bug，为了测试那段代码到底会不会搞崩，就运行它到bug处，看看有没有出core文件==》

一直没有出core文件，后来发现是ulimit -c设置的是0，导致没有出core文件。

我们通常可以用ulimit来设置shell的资源限制：

man手册是这么描述的：


	Provides  control  over the resources available to the shell and to processes started by it, on systems that allow such control.

先来看看一段错误的代码，这段代码将导致段错误：

```
#include <stdio.h>
#include <stdlib.h>

int main()
{
    char *str = "hello, world!";
    str[0] = 0;
    return 0;
}
```

编译并运行它：

```
[test1280@localhost 20170623]$ !g
gcc -o main main.c
[test1280@localhost 20170623]$ ./main
段错误
[test1280@localhost 20170623]$ ll
总计 12
-rwxrwxr-x 1 test1280 test1280 6588 06-23 20:43 main
-rw-rw-r-- 1 test1280 test1280  109 06-23 20:32 main.c
```

恩，段错误啦，但是并没有core文件==》

查看现在的资源限制，对core文件大小的设置：

	[test1280@localhost 20170623]$ ulimit -c

-c指的是允许生成的core的大小，单位字节；

输出是0，那就是最多只能生成0字节==》即不允许生成core文件；

How？

修改对core文件大小的限制：

```
[test1280@localhost 20170623]$ ulimit -c unlimited
[test1280@localhost 20170623]$ ulimit -c
unlimited
[test1280@localhost 20170623]$ ./main
段错误 (core dumped)
[test1280@localhost 20170623]$ ll
总计 100
-rw------- 1 test1280 test1280 172032 06-23 20:48 core.25599
-rwxrwxr-x 1 test1280 test1280   6588 06-23 20:43 main
-rw-rw-r-- 1 test1280 test1280    109 06-23 20:32 main.c
```

看到了吗？

关键是：

	ulimit -c unlimited

设置对core文件大小是无限的。

当然，我们可以使用：

	ulimit -c 100

注意这里的100单位是KB。

恩，这下要记住，需要生成core文件，首先看看能生成最大的core文件是多大（Max磁盘占用空间），如果比较小，例如0什么的，可以设置大一些。

PS：

有个小现象：

```
[test1280@localhost ~]$ unlimit -c
-bash: unlimit: command not found
[test1280@localhost ~]$ ulimit -c
0
[test1280@localhost ~]$ ulimit -c unlimited
[test1280@localhost ~]$ ulimit -c
unlimited
[test1280@localhost ~]$ ulimit -c 100
[test1280@localhost ~]$ ulimit -c
100
[test1280@localhost ~]$ ulimit -c unlimited
-bash: ulimit: core file size: cannot modify limit: 不允许的操作
[test1280@localhost ~]$ ulimit -c 50
[test1280@localhost ~]$ ulimit -c
50
```

我发现：

登录shell时一般设置的ulimit -c是0；

在当前shell修改ulimit -c到一个较小值后（假定这个值是100），那么后续ulimit -c指定的值必须要比之前的较小值更小（比100还要小）…不过，重新登录shell就好啦，就可以再次重新设置啦（比如unlimited）。

我们可以将ulimit -c xxx写入登录时的profile中嘛…对吧，嘿嘿。

————————————————

版权声明：本文为CSDN博主「test1280」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。

原文链接：https://blog.csdn.net/test1280/article/details/73655994