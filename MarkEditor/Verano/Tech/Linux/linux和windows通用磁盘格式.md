---
date: 2023-09-09 00:29:49
layout: 'post'
status: 'public'
---

linux和windows通用的硬盘格式
Linux 和 Windows 通用的硬盘格式是FAT32。

FAT32 文件系统是一种旧的文件系统，可以被 Windows 和 Linux 操作系统所支持。它适用于较小的磁盘容量，最大文件大小限制为4GB。因此，如果您的磁盘容量较小，且您需要在 Linux 和 Windows 之间共享数据，FAT32 是一个不错的选择。

但是需要注意的是，FAT32 不支持文件权限和加密，因此不适用于存储敏感数据或需要高度保护的文件。同时，如果您的磁盘容量超过32GB，您需要将其分区为多个FAT32分区，否则无法格式化整个磁盘。

如果您需要在 Linux 和 Windows 之间共享数据，同时需要更好的性能和安全性，那么您可以考虑使用 exFAT 或 NTFS 文件系统。exFAT 支持更大的文件大小和更高的性能，而 NTFS 支持文件权限和加密，更适合存储敏感数据。不过，需要注意的是，exFAT 和 NTFS 文件系统不是所有 Linux 发行版默认支持的，可能需要进行额外的配置或安装软件才能够使用。

硬盘分区分两种情况：当硬盘磁盘空间< 2T时，使用此种方式没有问题，

当硬盘磁盘空间> 2T 时 使用此种方式可能会出问题，这种情况下使用Parted磁盘分区及挂载

第一步：添加硬盘/新建分区（fdisk）（此步骤非必须）
第二步：格式化分区（mkfs.ext4）
第三步：加载分区（mount）
1、第一步：添加硬盘/新建分区（fdisk）
a、查看当前系统所有硬盘及分区情况：fdisk -l
b、在指定的硬盘（例：/dev/sda）上创建分区：fdisk /dev/sdb ， 根据提示进行下一步操作，如：查看帮助（h），新建分区（n），删除分区（d），查看分区情况（p）
c、分区成功后，写分区表并退出（w）
注：fdisk 支持硬盘最大尺寸为 2TB，更详细说明请参看 Linux 在线手册（man fdisk）或百度一下。
2、第二步：格式化分区（mkfs.ext4）
对新建分区（例：/dev/sda1）进行格式化：mkfs.ext4 /dev/sdb1 。
3、第三步：加载分区
a、创建分区挂接目录，例：mkdir /disk-cache-1 和 mkdir /disk-cache-2
b、编辑 /etc/fstab 配置文件，将分区信息写进去。
c、加载新建分区：mount -a
====================================================


##硬盘挂载、分区、格式化为ext4格式
首先用fdisk -l 发现待分区的磁盘 /dev/sdb
fdisk /dev/sdb 对该磁盘进行分区，输入m并回车
输入n并回车，n是“new”新建分区的意思
输入p并回车
输入数字1并回车
输入w  "write"并回车，意思是对刚才的结果进行保存
再次使用fdisk -l查看分区的结果
如图分的新区为/dev/sdb1,，创建的新区格式化后就可以挂载使用了
格式化分区：mkfs.ext4 /dev/sdd1（这个过程需要的的时间会比较比较长）
其他分区格式化：
mkfs.ext4 /dev/sdc1
mkfs.ext4 /dev/sdd1
mkfs.ext4 /dev/sde1
第三步：加载分区
1.创建分区挂接目录
mkdir -p /dfs/dn1
mkdir -p /dfs/dn2
mkdir -p /dfs/dn3
mkdir -p /dfs/dn4
mount /dev/sdb1 /dfs/dn1
mount /dev/sdc1 /dfs/dn2
mount /dev/sdd1 /dfs/dn3
mount /dev/sde1 /dfs/dn4

2.编辑 /etc/fstab 配置文件
需要修改的部分内容
/dev/mapper/VolGroup-lv_home /home ext4 defaults 1 2
/dev/sdb1 /dfs/dn1 ext4 defaults 0 0
/dev/sdc1 /dfs/dn1 ext4 defaults 0 0
/dev/sdd1 /dfs/dn1 ext4 defaults 0 0
/dev/sde1 /dfs/dn1 ext4 defaults 0 0
====================================================

##Parted磁盘分区及挂载
  一、parted的用途及说明
概括使用说明：
parted用于对磁盘（或RAID磁盘）进行分区及管理，与fdisk分区工具相比，支持2TB以上的磁盘分区，并且允许调整分区的大小。
GNU手册说明：
parted是一个用于硬盘分区或调整分区大小的工具。使用它你可以创建、清除、调整、移动和复制ext2、ext3、linux-swap、FAT、FAT32和reiserfs分区；也能创建、调整和移动苹果系统的HFS分区；还能检测jfs、ntfs、ufs和xfs分区。该工具常用于为新安装的操作系统创建空间，重新分配硬盘使用情况，在将数据拷贝到新硬盘的时候也常常使用。

二、         parted的使用方法及步骤
1、对磁盘进行分区
（1）命令行方式
parted /dev/sdb mklabel gpt mkpart 1 ext3 1 5T
（2）交互式命令方式
命令
解释
parted /dev/sdb
对/dev/sdb进行分区或管理操作
GNU   Parted 1.8.1
使用 /dev/sdb
Welcome   to GNU Parted! Type 'help' to view a list of commands.
系统返回值
(parted)    mklabel   gpt
定义分区表格式
（常用的有msdos和gpt分区表格式，msdos不支持2TB以上容量的磁盘，所以大于2TB的磁盘选gpt分区表格式）
(parted)    mkpart   p1
创建第一个分区，名称为p1
（p1只是第一个分区的名称，用别的名称也可以，如part1）
File system type？  [ext2]?  ext3
定义分区格式
（不支持ext4，想分ext4格式的分区，可以通过mkfs.ext4格式化成ext4格式）
Start？  1
定义分区的起始位置
（单位支持K,M,G,T）
End？   5T
定义分区的结束位置
（单位支持K,M,G,T）
(parted)    print
查看当前分区情况
Model:   ATA VBOX HARDDISK (scsi)
Disk   /dev/sda: 21.5GB
Sector   size (logical/physical): 512B/512B
Partition   Table: msdos
Number  Start     End   Size  File system  Name  Flags

1、 32.3kB  5TB   5TB      ext3       p1      
系统返回值
2、删除分区
命令
解释
parted /dev/sdb
对/dev/sdb进行分区或管理操作
(parted)    rm
rm删除命令
（删除之前必须确保分区没有被挂载）
Partition number？ 1
删除第一个分区
(parted)    print
查看当前分区情况
Model:   ATA VBOX HARDDISK (scsi)
Disk   /dev/sda: 21.5GB
Sector   size (logical/physical): 512B/512B
Partition   Table: msdos
Number  Start     End   Size  File system  Name  Flags
系统返回值

3、格式化几个TB的磁盘的说明
在格式化几个TB的磁盘的时候，时间会非常的长，格式化6T的磁盘时间大概在一个半小时左右。（据硬盘实际情况而定）

以下以一个创建分区为例对此进行说明，下面是操作步骤、步骤说明和相应的截图：
1:选择要分区的盘 
parted /dev/sdb

2:格式化分区 
mklabel gpt

3:分区 
mkpart primary 0% 100%

4:退出 
q

5：格式化 
mkfs.xfs /dev/sdb1
