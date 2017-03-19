---
title: 使用jemalloc优化Nginx&MySQL内存管理
date: 2017-03-19 18:40:19
tags: High performance(高性能)
toc: true
---

---

- 安装jemalloc：
```
yum install jemalloc-devel -y
```

- 一些性能对比

> [jemalloc优化MySQL、Nginx内存管理](https://blog.linuxeye.cn/356.html)

> [MySQL性能测试--jemalloc内存管理](http://www.linuxeye.com/Linux/1914.html)

- 结合MySQL（二进制版）：

```
sed -i 's@executing mysqld_safe@executing mysqld_safe\nexport LD_PRELOAD=/usr/lib64/libjemalloc.so@' /alidata/server/mysql/bin/mysqld_safe
```

- 结合tengine（编译版）：
```
--with-jemalloc
```

- 验证.so已经加载：
```
# lsof -n | grep jemalloc
nginx      6551  root  mem       REG              252,1   210024    1980262 /usr/lib64/libjemalloc.so.1
nginx      6552   www  mem       REG              252,1   210024    1980262 /usr/lib64/libjemalloc.so.1
```
