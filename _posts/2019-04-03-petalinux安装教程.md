---
layout:     post   				    # 使用的布局（不需要改）
title:      PetaLinux安装 				# 标题 
subtitle:                            #副标题
date:       2019-04-03 				# 时间
author:     LC 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - zcu106
---

# 一、安装要求

* 主机环境
  * 8 GB RAM (recommended minimum for Xilinx tools)
  * 2 GHz CPU clock or equivalent (minimum of 8 cores)
  * 100 GB free HDD space
  * Supported OS:
    * Ubuntu Linux 16.04.3, 16.04.4 (64-bit)
    * CentOS 7.2, 7.3, 7.4, 7.5 (64-bit)
  * PetaLinux tools require your host system` /bin/sh ` is bash. If you are using Ubuntu distribution and your` /bin/sh ` is dash, consult your system administrator to change your default host system` /bin/sh ` with the *sudo dpkg-reconfigure dash* command.
在终端输入
```
1、$ sudo dpkg-reconfigure dash 
2、在弹出的界面选择“否”，禁用dash
3、$ sudo ls -al /bin/sh
```
如果看到`lrwxrwxrwx 1 root root 5 Jun 9 14:59 /bin/sh -> /bin/bash`表示修改完成。

# 二、更换更新源
## 第一步

打开终端，输入`sudo gedit /etc/apt/sources.list`,将自己找到的源替换到该文件底部，将其他内容使用`#`注释。可以使用阿里源，清华源等等。
> 我在这使用的是西电源

版本号为：
1. bionic (18.04 LTS)
2. artful (17.10)
3. zesty (17.04)
4. yakkety (16.10)
5. xenial (16.04 LTS)
6. trusty (14.04 LTS)
7. precise (12.04 LTS)

以xenial为例，其它版本替换成相应版本号即可（可以用 lsb_release -a 查看当前版本代号）。 编辑/etc/apt/sources.list， 替换为以下内容（请做好备份） 请根据需要去掉 deb-src 以及 backports 和 proposed-updates 前面的注释。
```
deb http://linux.xidian.edu.cn/mirrors/ubuntu/ xenial main restricted universe multiverse
#deb-src http://linux.xidian.edu.cn/mirrors/ubuntu/ xenial main restricted universe multiverse

deb http://linux.xidian.edu.cn/mirrors/ubuntu/ xenial-security main restricted universe multiverse
#deb-src http://linux.xidian.edu.cn/mirrors/ubuntu/ xenial-security main restricted universe multiverse

deb http://linux.xidian.edu.cn/mirrors/ubuntu/ xenial-updates main restricted universe multiverse
#deb-src http://linux.xidian.edu.cn/mirrors/ubuntu/ xenial-updates main restricted universe multiverse

#deb http://linux.xidian.edu.cn/mirrors/ubuntu/ xenial-backports main restricted universe multiverse
#deb-src http://linux.xidian.edu.cn/mirrors/ubuntu/ xenial-backports main restricted universe multiverse

#deb http://linux.xidian.edu.cn/mirrors/ubuntu/ xenial-proposed main restricted universe multiverse
#deb-src http://linux.xidian.edu.cn/mirrors/ubuntu/ xenial-proposed main restricted universe multiverse
```
## 第二步

更新软件源，在终端输入：
$ `sudo apt-get update`。

# 三、安装依赖库

>需要root权限

## 第一步
快速安装依赖库
```
$ sudo apt-get install tofrodos  iproute2 gawk
$ sudo apt-get install xvfb
$ sudo apt-get install gcc git make 
$ sudo apt-get install net-tools libncurses5-dev 
$ sudo apt-get install zlib1g-dev zlib1g-dev:i386 libssl-dev flex bison libselinux1  
$ sudo apt-get install gnupg wget diffstat chrpath socat xterm
$ sudo apt-get install autoconf libtool tar unzip texinfo zlib1g-dev gcc-multilib
$ sudo apt-get install build-essential libsdl1.2-dev libglib2.0-dev
$ sudo apt-get install screen pax gzip tar
```
##第二步
安装`tpfp`库
```
$ sudo apt-get install tftpd tftp openbsd-inetd
```
```
$ sudo gedit /etc/inetd.conf
% 在文件内复制如下内容
tftp dgram udp wait nobody /usr/sbin/tcpd /usr/sbin/in.tftpd /tftproot
```
```
$ sudo mkdir /tftproot
$ sudo chmod 777 /tftproot
$ sudo /etc/init.d/openbsd-inetd restart
$ sudo netstat -an | more | grep udp
```
如果看到如下显示，表示成功。
```
udp        0      0 0.0.0.0:69              0.0.0.0:*
```
# 四、安装PetaLinux

>安装软件时需要是普通用户，并且Vivado、XSDK和PetaLinux的版本必须一致。

## 第一步
从官网下载对应版本的PetaLinux版本，至于怎么去官网下载，大家应该都清楚吧，此处就不展示了。

## 第二步

```
$ mkdir -p opt/pkg/petalinux/2018.3

$ ./petalinux-v2018.3-final-installer.run /home/pp/opt/pkg/petalinux/2018.3
```
* 如果新建的文件夹权限不够，需要使用`chmod -R 700 opt/`
* 一旦开始安装，不能复制或者移动所安装文件夹的内容
* 安装过程会出现三个license文档，键盘输入`q`键关闭

# 五、设置PetaLinux工作环境变量

## 第一步
```
$ sudo gedit ~/.bashrc

%添加如下内容，并保存
source <path-to-installed-PetaLinux>/settings.sh
```
## 第二步
验证环境变量成功
```
$ echo $PETALINUX
%出现如下信息，即成功
/opt/pkg/petalinux
```


