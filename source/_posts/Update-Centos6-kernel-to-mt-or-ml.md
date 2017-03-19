---
title: Centos6更新内核到3.x (lt)或4.x (mt)版本
date: 2017-03-19 00:07:13
tags: Centos Linux
toc: true
---


---


## 添加源：

``` bash
rpm -Uvh http://www.elrepo.org/elrepo-release-6-6.el6.elrepo.noarch.rpm
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
```

## 搜索kernel相关包：
``` bash
yum --enablerepo=elrepo-kernel search kernel
```

---

### kernel-lt 3.x:

>kernel-lt.x86_64 : The Linux kernel. (The core of any Linux-based operating system.)
kernel-lt-devel.x86_64 : Development package for building kernel modules to match the kernel.
kernel-lt-doc.noarch : Various bits of documentation found in the kernel sources.
kernel-lt-firmware.noarch : Firmware files used by the Linux kernel
kernel-lt-headers.x86_64 : Header files for the Linux kernel for use by glibc

``` bash
============================================================================================================================================
 Package                       Arch                       Version                                   Repository                         Size
============================================================================================================================================
Installing:
 kernel-lt                     x86_64                     3.10.105-1.el6.elrepo                     elrepo-kernel                      33 M

Transaction Summary
============================================================================================================================================
Install       1 Package(s)

Total download size: 33 M
Installed size: 154 M
```

---

### kernel-ml 4.x:

>kernel-ml.x86_64 : The Linux kernel. (The core of any Linux-based operating system.)
kernel-ml-devel.x86_64 : Development package for building kernel modules to match the kernel.
kernel-ml-doc.noarch : Various bits of documentation found in the kernel sources.
kernel-ml-firmware.noarch : Firmware files used by the Linux kernel.
kernel-ml-headers.x86_64 : Header files for the Linux kernel for use by glibc.


```
============================================================================================================================================
 Package                       Arch                       Version                                   Repository                         Size
============================================================================================================================================
Installing:
 kernel-ml                     x86_64                     4.10.3-1.el6.elrepo                       elrepo-kernel                      41 M

Transaction Summary
============================================================================================================================================
Install       1 Package(s)

Total download size: 41 M
Installed size: 182 M
```

## 安装后修正配置：
```
# vim /etc/grub.conf
default=0    # 0的意思是最新安装的内核，1次之，以此类推
```

## 重启，验证内核版本：
``` bash
[root@x ~]# uname -a
Linux x 4.10.3-1.el6.elrepo.x86_64 #1 SMP Wed Mar 15 15:09:10 EDT 2017 x86_64 x86_64 x86_64 GNU/Linux
```
