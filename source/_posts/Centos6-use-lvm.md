---
title: Centos6 LVM使用教程
date: 2017-03-19 12:47:59
tags: Centos Linux
toc: true
---


---


## 安装lvm：
```
yum install lvm2 -y
```

## 查看新加入磁盘：
```
fdisk -l
```

> Disk /dev/vdb: 21.5 GB, 21474836480 bytes
16 heads, 63 sectors/track, 41610 cylinders
Units = cylinders of 1008 * 512 = 516096 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00000000

## 创建lvm

### 创建pv
```
pvcreate /dev/vdb
```

### 以pv块创建vg组
```
vgcreate vg0 /dev/vdb
```

### 以vg创建lv逻辑卷
```
lvcreate -L 20470M -n lv0 vg0
```

> -L 20470M 这里的数字不能大于fdisk -l看到的数，且必须整除4

## 准备文件系统：
```
yum install xfsprogs -y
mkdir -pv /production
mkfs.xfs /dev/vg0/lv0
umount /production
mount -t xfs -o rw,noatime,nobarrier /dev/vg0/lv0 /production
```

> SGI XFS with ACLs, security attributes, no debug enabled
XFS (dm-0): Mounting V4 Filesystem
XFS (dm-0): Ending clean mount
XFS (dm-0): Unmounting Filesystem
XFS (dm-0): nobarrier option is deprecated, ignoring.
XFS (dm-0): Mounting V4 Filesystem
XFS (dm-0): Ending clean mount

**mount 完发现dmesg出现warning，所以需要修正参数：**

```
umount /production
mount -t xfs -o rw,noatime /dev/vg0/lv0 /production
mount -o remount /production
```
