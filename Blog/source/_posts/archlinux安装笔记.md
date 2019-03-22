---
title: archlinux 安装笔记
date: 2018-10-06 16:07:40
categories:
- Linux学习
tags:
- Linux
---

## 一.准备工作
 1.安装[VMware虚拟机](https://www.baidu.com/link?url=LepTOXQc-bVXySu-vF-QDWSHm4eMiBMRd0qq4ajzmw_H9ap4ILYdC0hEweQ-LM1TgEbK_uxctpoqz6wSnn52LIssOB3v1M2JBWgjAxlXuJK&wd=&eqid=827c25cd0002c393000000035ac9ddba)；
 
2.在[清华大学开源开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/)下载archlinux的镜像；
## 二.开始装机
1.打开VM，新建虚拟机，选择ubantu64位，大小选择25；
<!-- more -->
2.创建好之后，点击开机按钮；

3.大概等待几分钟，基本安装即可完成；
## 三.使用命令配置
### 1.连接到因特网
1. 守护进程 dhcpcd 已被默认启用来探测有线设备, 并会尝试连接，可以使用命令：
```
ping -c 3 archlinux.org
ping -c 3 www.baidu.com
```
2.无线状态展示
```
iw -show
```
### 2. 设置密码;
```
passwd
```
### 3.分区方法：
1.分区方案：
```
sda1—————20—————/boot/EFi

sda2—————200M————/boot

sda3—————100G————/
```

2.磁盘若被系统识别到，就会被分配为一个块设备，查看分区。
```
fdisk -l
```
3.建立GPT分区表
```
fdisk /dev/sda
```
4.建立分区 
```
回车：

提示让输入开始扇区(一个扇区512B，按自己要分区容量大小进行计算) 
输入2048,回车

让输入结束扇区，由于一个扇区512B，要创建200M的分区,应该输入：+200M；

建立第二个分区： 
输入n; 
回车 
输入开始扇区: 回车 （默认开始扇区即可） 
输入结束扇区:+200M

建立第三个分区： 
输入n; 
回车 
输入开始扇区:回车 （默认开始扇区即可） 
输入结束扇区:直接回车(默认大那个数字)

输入:w 保存并退出； 
```
5.格式化分区
```
mkfs.fat -F32 /dev/sda1
mkfs.ext4 /dev/sda2
mkfs.ext4 /dev/sda3
```
6.挂载分区
首先将根分区挂载到 /mnt
```
mount /dev/sda3 /mnt
mkdir /mnt/boot
mount /dev/sda2 /mnt/boot
mkdir /mnt/boot/EFI
mount /dev/sda1 /mnt/boot/EFI
```
7.如果使用多个分区，还需要为其他分区创建目录并挂载它们（/mnt/boot、/mnt/home)
```
mkdir /mnt/boot
mount /dev/sda/boot
```
8.安装基本系统
```
pacstrap /mnt base
pacstrap -i /mnt base base-devel
```
9.Fstab
```
genfstab -U /mnt >> /mnt/etc/fstab
```
### 4.软件管理:
1.使用pacman来管理软件
```
#安装软件/软件组
pacman -S [软件1] [软件2] ...

#卸载软件/软件组
pacman -R [软件1] [软件2] ...

#刷新数据库
pacman -Syy

#升级整个系统
pacman -Syyu
```
2.更改镜像源：
```
sudo gedit /etc/pacman.conf

#在文档结尾处加入下面的文字：
    [archlinuxcn]
    SigLevel = Optional TrustAll
    Server = http://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
#保存退出，刷新pacman数据库
```
### 5.图形界面配置;

此以Gnome为例，其它基本大同小异

1.首先安装xorg-server，这是图形界面的基础。
```
sudo pacman -S xorg-server
```
2.然后安装相关驱动，选择2（virtualbox-guest-utils）， 此为arch在虚拟机安装所需要的软件组。
3.再安装Gnome3桌面了
```
sudo pabman -S gnome
```
4.让开机进入图形界面
```
sudo systemctl enable gdm.service
```
相关systenmctl的使用:
```
#show system status using:
systemctl status
```
```
#List running units:
systemctl list-units
```
```
#List failed units:
systemctl -failed
```
5.安装中文字体
```
sudo pacman -S adode-source-han-sans-cn-fonts
```
6.安装chrome浏览器
```
sudo pacman -S google-chrome
```
### 6.用户管理
1.新安装的arch只有一个root用户，使用root用户来进行日常系统管理是很危险的事情，说不定哪天手抖输了个rm -rf /*然后你就呵呵了。所以我们通常用普通用户来进行日常使用，有需要的时候就用sudo来获取root权限。
2.首先添加一个用户，并把它加到wheel组
```
useradd -m  -G wheel -s  /bin/bash 【用户名】
```
3.设置密码：
```
passwd
```
4.最后设置wheel组的用户能用sudo获取root权限:
```
visudo
    #找到这样的一行,把前面的#去掉:
    #%wheel ALL=(ALL) ALL
按ESC键，输入x!回车就可以保存并退出
```
### 7.总结操作系统的安装流程
安装archlinux最重要的是要去看archwiki上的教程，启动arch后先要进行分区，挂载分区，安装相关驱动，最后配置桌面等等。其实最重要的就是在于好好看wiki，不要去看其他半全不全的教程QAQ。
