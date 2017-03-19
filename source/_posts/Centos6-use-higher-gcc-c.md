---
title: Centos6环境使用高版本gcc和c++
date: 2017-03-19 17:49:39
tags: Centos Linux
toc: true
---

---

## 安装scl repo：
```
yum install centos-release-scl -y
```

### 安装devtoolset-4包：
```
yum install devtoolset-4-gcc-c++ devtoolset-4-gcc -y
# yum install cmake git -y 『可选』
# yum install ncurses-devel openssl-devel bison -y 『可选』
```

---

## 设置devtoolset-4套件中gcc的环境：
```
scl enable devtoolset-4 bash
gcc -v
```

> Using built-in specs.
COLLECT_GCC=gcc
COLLECT_LTO_WRAPPER=/opt/rh/devtoolset-4/root/usr/libexec/gcc/x86_64-redhat-linux/5.3.1/lto-wrapper
Target: x86_64-redhat-linux
Configured with: ../configure --enable-bootstrap --enable-languages=c,c++,fortran,lto --prefix=/opt/rh/devtoolset-4/root/usr --mandir=/opt/rh/devtoolset-4/root/usr/share/man --infodir=/opt/rh/devtoolset-4/root/usr/share/info --with-bugurl=http://bugzilla.redhat.com/bugzilla --enable-shared --enable-threads=posix --enable-checking=release --enable-multilib --with-system-zlib --enable-__cxa_atexit --disable-libunwind-exceptions --enable-gnu-unique-object --enable-linker-build-id --enable-plugin --with-linker-hash-style=gnu --enable-initfini-array --disable-libgcj --with-default-libstdcxx-abi=gcc4-compatible --with-isl=/builddir/build/BUILD/gcc-5.3.1-20160406/obj-x86_64-redhat-linux/isl-install --enable-libmpx --with-mpc=/builddir/build/BUILD/gcc-5.3.1-20160406/obj-x86_64-redhat-linux/mpc-install --with-tune=generic --with-arch_32=i686 --build=x86_64-redhat-linux
Thread model: posix
gcc version 5.3.1 20160406 (Red Hat 5.3.1-6) (GCC)

---

## yum提速
```
sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/CentOS-SCLo-scl-rh.repo
sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/CentOS-SCLo-scl.repo
sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/elrepo.repo
```

### 需要时，使用yum --enablerep参数打开
```
--enablerepo=elrepo-kernel \
elrepo \
centos-sclo-rh \
centos-sclo-sclo
...
```

[部分参考自：山羊博客](http://blog.fungo.me/2016/10/compile-alisql-from-source/)

<!-- ![](http://upload-images.jianshu.io/upload_images/259-90ac0f366310f464.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) -->
