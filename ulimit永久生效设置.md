# ulimit永久生效设置 #

1.查看

```
ulimit -a
 
ulimit -u    # max user processes
 
ulimit -n    # open files
```

2.  编辑/etc/security/limits.conf

```
* soft nofile 65536      # open files  (-n)
* hard nofile 65536
 
* soft nproc 65565
* hard nproc 65565       # max user processes   (-u)
```
 
 
需要重新登录，或者重新打开ssh客户端连接，永久生效

 
————————————————

版权声明：本文为CSDN博主「kong-kong」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。

原文链接：https://blog.csdn.net/kq1983/article/details/83443907