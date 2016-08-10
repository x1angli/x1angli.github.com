---
published: true
title: Setting Up Python 3 on Centos
layout: post
tags: [DevOps, Centos, Linux, Python, Python 3, 阿里云, 系统管理]
categories: [Configuration]
permalink: setting-up-python-3-on-centos-chs
---
# 在Centos主机上安装Python 3

> 本文将以阿里云的Centos 7.2 64位ECS实例为样板，介绍如何在Centos主机上安装Python 3.x，及pip/virtualenv等必备组件

## 序

虽然网上有一些类似的教程作为参考，主要还是面向国外的公有云用户。由于特殊国情（国际带宽相对受限以及……），国内公有云在访问国外镜像时面临速度慢或访问不稳定等问题。因此，国内用户无法直接照搬这些教程，而需要一份专门针对国情出发的实用系统管理手册。

本文将以阿里云的Centos 7.2 64位ECS实例为样板，介绍如何在Centos主机上安装Python 3.x，及pip/virtualenv等必备组件。
* 本文所阐述的方法不局限于Centos，其同样也适用于其他RHEL-Based Linux，如Fedora, RHEL等操作系统。
* 本文所阐述的方法不局限于阿里云，其同样也适用于其他国内的公有云，尤其是已经优化过系统的yum镜像库（Yum Repositories）的公有云。

## 已知问题

由于通过此种方式安装的virtualenv有可能会被sudo命令失效，即，用户先通过virtualenv命令建立一个虚拟环境，然后再激活此虚拟环境。这时候，任何sudo后的pip命令都将指向全局python环境，而不是指向沙盒中的虚拟环境；只有普通的pip或pip3命令会指向虚拟环境。然而不幸的是，很多包在通过pip安装时都需要sudo权限。一种解决方式是弃用虚拟环境，直接使用全局python…… 


## 目标读者

使用RHEL-Based Linux的国内公有云的程序员、系统管理员。

## 主要思路：
1. 由于阿里云的操作系统在生成实例时已经预先将epel-release等repository指向阿里云自己的源，显然ECS主机访问这些镜像是最快的，因此我们尽量使用这些源，并避免从访问速度相对慢的境外网站下载内容。
2. 相对于已编译的包，自行编译既费时又容易出错，因此尽量使用已编译的包
3. 推荐使用新版本的Python 3。

## 懒人简明版：

在SSH上先输入`sudo`，及管理员密码，再将下列代码复制粘贴并执行：

    sudo yum makecache
    sudo yum -y update
    sudo yum -y install python34 python34-devel python34-setuptools
    sudo easy_install-3.4 pip
    sudo pip3 install --upgrade pip
    sudo pip3 install --upgrade virtualenv


## 详细拆解：

#### 更新yum缓存及已经安装的包

    sudo yum makecache
    sudo yum -y update

更新yum包管理器上的所有已经安装的包。`-y`参数表示所有的提示询问都会被默认yes而继续，这样整个过程就不会被打断。

#### 安装Python 3.4的核心程序和扩展程序

    sudo yum -y install python34 python34-devel 

在这里, `python34`和`python34-devel`一般连用。而`python34-devel`一般是安装python时必然附加的选项，否则一些pip包就无法安装。


#### 安装Python 3相关联的pip

    sudo yum -y install python34-setuptools
    sudo easy_install-3.4 pip

这里从yum镜像中下载`python34-setuptools`包，并且在本地安装Python 3的pip    
    
> 注1：也许你会听说这样一种做法：`sudo curl https://bootstrap.pypa.io/get-pip.py | python3.4`。<br />
> 由于阿里云自己的ECS主机已经事先将PyPI的源指向本地镜像，即http://mirrors.aliyun.com/pypi/，节省了该脚本在下载pip和wheel的.whl文件的时间。
> 然而，如果你的ECS主机没有改过PyPI源指向，考虑到国内的VPS所在的机房连国外一般非常慢，导致在执行这一条时会花费非常长的时间，因此不建议在PyPI在指向国外镜像时如此使用。

> 注2：相对于`sudo easy_install-3.4 pip`，一些教程建议的是 `sudo easy_install pip`，漏掉了版本号`3.4`。<br />
> 然而，这样只会安装Python 2的pip。而Python 2的pip应该已经在Centos上预先安装，且pip for python 2不是我们想要的。因此，我们需要调用于`sudo easy_install-3.4 pip`来安装Python 3的pip


#### 用pip去更新自己，并且安装virtuanenv

    sudo pip3 install --upgrade pip
    sudo pip3 install --upgrade virtualenv

这样，你就能在命令行下使用`pip`和`virtualenv`了

> 注：由于在pip3成功安装后，这两个条命令的pip3如果换成pip最终效果也是一样的，所以你也可以用`pip`命令代替其中的`pip3`。
