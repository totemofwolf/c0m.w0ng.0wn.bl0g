---
title: 使用rpmbuild工具打包tengine-2.1.2
date: 2017-03-20 00:16:12
tags: Centos Linux
toc: true
---

---

## 新建编译用户：
```
useradd rpmbuild -s /bin/bash
su - rpmbuild
```

---

## 创建目录，准备材料：
```
mkdir -p ~/rpmbuild/{BUILD,RPMS,SOURCES,SPECS,SRPMS}
cd rpmbuild/
```

## 在家目录编写.rpmmacros(自定义制作路径)
```
#.rpmmacros
%_topdir /home/rpmbuild/rpmbuild
%_tmppath /home/rpmbuild/rpmbuild/tmp
%buildroot /home/rpmbuild/rpmbuild/BUILDROOT
%_prefix   /
```

### 准备SPECS文件：
```
cat > SPECS/tengine.spec

Name:       tengine
Version:    2.1.2
Release:    1
Vendor:     Taobao
Summary:    GUN Tengine X86_64
License:    GPL
Source:     tengine-2.1.2.tar.gz
Group:      System Enviroment/Daemons
URL:        http://tengine.taobao.org/
Packager:   gk.wl@qq.com
%description
Taobao tengine package


%prep
%setup -q
./configure --prefix=/usr/local/nginx --with-cc-opt='-O2 -fstack-protector --param=ssp-buffer-size=4 -Wformat -Werror=format-security -D_FORTIFY_SOURCE=2' --with-ld-opt='-Wl,-Bsymbolic-functions -Wl,-z,relro' --user=www --group=www --with-jemalloc --with-http_stub_status_module --with-openssl=/usr/local/src/sh-1.5.5/openssl-1.0.2j --with-http_gzip_static_module --with-http_concat_module --with-http_realip_module --with-http_v2_module --with-http_sysguard_module --with-syslog --with-http_secure_link_module --without-http-cache --without-poll_module --without-select_module --without-mail_pop3_module --without-mail_imap_module --without-mail_smtp_module

make
%install
make DESTDIR=$RPM_BUILD_ROOT install
%clean
[ "$RPM_BUILD_ROOT" != "/" ] && rm -rf "$RPM_BUILD_ROOT"
make clean

%files
%defattr (-,root,root)
/usr/local/nginx/
```

---

### 下载源码包，放入SOURCES目录：
```
wget -c http://tengine.taobao.org/download/tengine-2.1.2.tar.gz SOURCES/
```

### 目录结构：
```
$ tree .
.
├── BUILD
├── RPMS
├── SOURCES
│   └── tengine-2.1.2.tar.gz
├── SPECS
│   └── tengine.spec
└── SRPMS

5 directories, 2 files
```

---

## 开始编译：
```
cd
rpmbuild -ba ./rpmbuild/SPECS/tengine.spec
```

> + /usr/lib/rpm/check-buildroot
+ /usr/lib/rpm/redhat/brp-compress
+ /usr/lib/rpm/redhat/brp-strip /usr/bin/strip
+ /usr/lib/rpm/redhat/brp-strip-static-archive /usr/bin/strip
+ /usr/lib/rpm/redhat/brp-strip-comment-note /usr/bin/strip /usr/bin/objdump
+ /usr/lib/rpm/brp-python-bytecompile /usr/bin/python
+ /usr/lib/rpm/redhat/brp-python-hardlink
+ /usr/lib/rpm/redhat/brp-java-repack-jars
Processing files: tengine-2.1.2-1.x86_64
Requires(rpmlib): rpmlib(CompressedFileNames) <= 3.0.4-1 rpmlib(FileDigests) <= 4.6.0-1 rpmlib(PayloadFilesHavePrefix) <= 4.0-1
Requires: /bin/sh libc.so.6()(64bit) libc.so.6(GLIBC_2.10)(64bit) libc.so.6(GLIBC_2.2.5)(64bit) libc.so.6(GLIBC_2.3)(64bit) libc.so.6(GLIBC_2.3.2)(64bit) libc.so.6(GLIBC_2.3.4)(64bit) libc.so.6(GLIBC_2.4)(64bit) libc.so.6(GLIBC_2.7)(64bit) libcrypt.so.1()(64bit) libcrypt.so.1(GLIBC_2.2.5)(64bit) libdl.so.2()(64bit) libdl.so.2(GLIBC_2.2.5)(64bit) libjemalloc.so.1()(64bit) libpcre.so.0()(64bit) libpthread.so.0()(64bit) libpthread.so.0(GLIBC_2.2.5)(64bit) libz.so.1()(64bit) rtld(GNU_HASH)
Checking for unpackaged file(s): /usr/lib/rpm/check-files /home/rpmbuild/rpmbuild/BUILDROOT
Wrote: /home/rpmbuild/rpmbuild/SRPMS/tengine-2.1.2-1.src.rpm
Wrote: /home/rpmbuild/rpmbuild/RPMS/x86_64/tengine-2.1.2-1.x86_64.rpm
Executing(%clean): /bin/sh -e /home/rpmbuild/rpmbuild/tmp/rpm-tmp.5FyPzw
+ umask 022
+ cd /home/rpmbuild/rpmbuild/BUILD
+ cd tengine-2.1.2
+ '[' /home/rpmbuild/rpmbuild/BUILDROOT '!=' / ']'
+ rm -rf /home/rpmbuild/rpmbuild/BUILDROOT
+ make clean
rm -rf Makefile objs
+ exit 0

### 编译完成的目录结构：
```
$ tree .
.
├── BUILD
├── RPMS
│   └── x86_64
│       └── tengine-2.1.2-1.x86_64.rpm
├── SOURCES
│   └── tengine-2.1.2.tar.gz
├── SPECS
│   └── tengine.spec
└── SRPMS
    └── tengine-2.1.2-1.src.rpm

6 directories, 4 files
```

---

## 安装（root用户）
```
# rpm -ivh /home/rpmbuild/rpmbuild/RPMS/x86_64/tengine-2.1.2-1.x86_64.rpm
Preparing...                ########################################### [100%]
   1:tengine                ########################################### [100%]
```
