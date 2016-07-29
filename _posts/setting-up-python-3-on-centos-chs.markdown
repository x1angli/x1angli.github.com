---
published: true
title: Setting Up Python 3 on Centos
layout: post
tags: [Centos, Python, Aliyun]
categories: [Configuration]
permalink: setting-up-python-3-on-centos-chs
---
# 在Centos主机上安装Python 3

> 本文将以阿里云的Centos 7.2 64位ECS实例为样板，介绍如何在Centos主机上安装Python 3.x，及pip/virtualenv等必备组件

## 懒人版：直接复制粘贴下列代码到SSH终端去

    sudo yum -y update
    sudo yum install python34 python34-devel python34-setuptools
    sudo easy_install-3.4 pip
    sudo pip3 install --upgrade pip
    sudo pip3 install --upgrade virtualenv


## 详细拆解：

#### 更新yum包

    sudo yum -y update

这句就不需要我多说了

#### 安装Python 3.4的核心程序和扩展程序

    sudo yum install python34 python34-devel python34-setuptools

在这里, `python34`和`python34-devel`一般连用。而`python34-devel`一般是安装python时必然附加的选项，否则一些pip包就无法安装。

而`python34-setuptools`则是为了安装pip做准备。

> 注意，一些教程上不是通过yum安装`python34-setuptools`，而是建议用户从一个网站上下载并执行一个.py脚本：`sudo curl https://bootstrap.pypa.io/get-pip.py | python3.4`。
> 这么做虽然也能装上pip，但国内的VPS所在的机房连国外一般非常慢，导致在执行这一条时会花费非常长的时间，因此不建议这么用。

#### 安装Python 3相关联的pip
    sudo easy_install-3.4 pip

> 注意，一些教程建议的是 `sudo easy_install pip` 。
> 其实是徒劳的，因为这样其实会安装Python 2的pip，而Python 2的pip应该已经安装在Centos里面的，并且不是我们想要的。我们需要Python 3的pip


#### 用pip去更新自己，并且安装virtuanenv

    sudo pip3 install --upgrade pip
    sudo pip3 install --upgrade virtualenv

由于在pip3成功安装后，这两个条命令的pip3如果换成pip最终效果也是一样的，所以你也可以用`pip`命令代替其中的`pip3`。
