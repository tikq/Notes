---
layout: post
title:  "VirtualBox虚拟机安装配置"
date: 2019-01-11
categories: Linux
tags: VirtualBox Ubuntu
---
本文介绍了在VirtualBox中安装Ubuntu Server的主要过程及相关配置

<!--more-->
### 准备工作
#### 下载镜像及VM
* [VirtualBox 5.2.22](https://www.virtualbox.org/)

* [Ubuntu Server 18.04.1 LTS 64-Bit](http://www.ubuntu.com/)

本文所使用的物理机为Windows7 64-Bit家庭版，下载Ubuntu Server镜像及Virtualbox，安装好VirtualBox。

#### VirtualBox配置
虚拟机使用Host-Only+网络共享静态IP方式联网，禁用DHCP，网卡选项手动配置网卡，VirtualBox Host-Only以太网适配器（如果没有请点击创建）配置如下：

![VirtualBox Host-Only](https://tikq.github.io/assets/image/blog/2019/2019-01-11_11-27-34.jpg)

### VM虚拟机创建
#### 新建虚拟电脑
主界面点击新建按钮新建虚拟电脑，输入名称，选择系统类型及版本，这里系统类型选择Linux、Ubuntu（64-bit),如下：

![新建虚拟电脑](https://tikq.github.io/assets/image/blog/2019/2019-01-11_11-14-34.jpg)

后续依次分配内存（1024MB)、创建虚拟硬盘(硬盘类型选择VDI、分配大小固定)，选择硬盘存放位置及大小,等待硬盘创建完成。

#### 虚拟电脑网络配置
选中虚拟电脑点击设置按钮进入网络选项卡，网卡1中连接方式选择“仅主机（Host-Only)网络”，界面名称选择已创建好的“VirtualBox Host-Only Ehternet Adapter”，混杂模式为“全部允许”，勾选“接入网线”，配置完成如下：

![虚拟电脑网络配置](https://tikq.github.io/assets/image/blog/2019/2019-01-11_11-26-58.jpg)


#### 安装镜像
* 主界面选中创建好的虚拟电脑，点击启动按钮并在弹窗中选择启动镜像进入安装流程：

    ![启动虚拟电脑](https://tikq.github.io/assets/image/blog/2019/2019-01-11_11-23-05.jpg)
    ![镜像选择](https://tikq.github.io/assets/image/blog/2019/2019-01-11_11-23-52.jpg)

* 安装过程中网络配置选择手动配置:Edit IPv4,不是用DHCP自动分配虚拟机IP，网络配置如下：

    ![禁用DHCP](https://tikq.github.io/assets/image/blog/2019/2019-01-11_11-30-36.jpg)
    ![禁用DHCP](https://tikq.github.io/assets/image/blog/2019/2019-01-11_11-31-02.jpg)
    ![IPv4配置](https://tikq.github.io/assets/image/blog/2019/2019-01-11_11-32-08.jpg)

* Ubuntu archive镜像配置,这里使用网易镜像地址：http://mirrors.163.com/ubuntu

* 文件系统安装,使用整个磁盘，确认后等待安装完成后重启。

* 在VirtualBox主界面选中虚拟机，启动项可以选择无界面启动，启动后可使用SSH客户端连接，对于虚拟机较多的情况，方便使用。

* 启动后主机和虚拟机互Ping，虚拟机ping任意公网地址，看网络是否通畅。

* 安装过程到此结束，后续可根据需要安装相应软件进行开发工作。

### 可能遇到的问题
* 无法选择64-bit系统版本？
   
   BIOS开启Intel 虚拟技术。

* 网络无法Ping通？
 
   请确认防火墙配置。

* 安装后如何修改网络配置？
   
    执行`sudo nano /etc/netplan/50-cloud-init.yaml`命令编辑网络配置：
    ```
    # This file is generated from information provided by
    # the datasource.  Changes to it will not persist across an instance.
    # To disable cloud-init's network configuration capabilities, write a file
    # /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
    # network: {config: disabled}
    network:
        ethernets:
            enp0s3:
                addresses: [192.168.137.20/24]
                dhcp4: false
                gateway4: 192.168.137.1
                nameservers:
                addresses: [192.168.137.1]
        version: 2
    ```
* 主机与虚拟机文件共享？
  
   这里我试用FileZilla进行文件上传下载，实现文件共享。

* Windwos10下使用无线网络共享给VitrualBox虚拟网卡，重启后虚拟机DNS解析出现问题？

    可能是无线网络共享不稳定，尝试重新配置共享。


参考链接：
* https://netplan.io/
* https://askubuntu.com/questions/1029531/how-to-setup-a-static-ip-on-ubuntu-18-04-server