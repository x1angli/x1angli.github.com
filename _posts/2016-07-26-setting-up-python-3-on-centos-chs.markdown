---
published: true
title: 在Centos主机上安装Python 3
layout: post
tags: [Centos, Python, Aliyun, 阿里云]
categories: [Configuation]
---
> 本文将以阿里云的Centos 7.2 64位ECS实例为样板，介绍如何在Centos主机上安装Python 3.x，及pip/virtualenv等必备组件

阿里云的主机，如果选择了Centos，那么系统会默认安装Python 2，那么我们应该如何快速且稳妥地安装Python 3呢？本文将做一介绍。

## 懒人专用：将下列代码在SSH中执行即可

    sudo yum -y update
    sudo yum install python34 python34-devel python34-setuptools
    sudo easy_install-3.4 pip
    sudo pip3 install --upgrade pip
    sudo pip3 install --upgrade virtualenv

## 详细拆解

`sudo yum -y update`
> 这句话是更新yum包。Yum包是RHEL/Centos系列Linux的默认的包管理器

`sudo yum install python34 python34-devel python34-setuptools`
> 这里是为了尽量从aliyun自带的repository中下载。尽量避免修改aliyun的库源

> 另外，网上的一些些教材会让大家用以下命令
> `# sudo curl https://bootstrap.pypa.io/get-pip.py | python3.4`
> 不建议大家在国内的VPS上使用。原因在于该命令包含从pypa.io上下载的环节，而国内的VPS主机或是个人电脑连国外一般会非常非常慢（甚至慢到1kB/s），所以执行这段命令会花不必要的时间。

`sudo easy_install-3.4 pip`
> 注意，有些地方说的是
> `sudo easy_install pip`
> ——这是错的的！这个将会安装Python 2.x 版的pip! 我们的目标是安装Python 3对应的pip（即pip3），并且将`pip`这个命令指向pip3


`sudo pip3 install --upgrade pip`
`sudo pip3 install --upgrade virtualenv`
> 这里pip也是指向python3，所以pip3等价于pip