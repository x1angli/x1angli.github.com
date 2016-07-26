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
`sudo yum -y update`
>这句就不需要我多说了

`sudo yum install python34 python34-devel python34-setuptools`
`sudo easy_install-3.4 pip`
> 注意，一些教程建议的是`sudo easy_install pip`，这其实是徒劳的，因为这样其实会安装Python 2的pip，而Python 2的pip并不是我们想要的

> 另一些教程会建议从一个网站上下载并执行一个.py脚本：
> `sudo curl https://bootstrap.pypa.io/get-pip.py | python3.4`
> 这么做虽然是对的，但国内的VPS在执行这一条时速度非常慢，因为涉及到从国外网站上下载东西。

`sudo pip3 install --upgrade pip`
`sudo pip3 install --upgrade virtualenv`
> 由于在pip3成功安装后，这两个条命令的pip3如果换成pip最终效果也是一样的，所以你也可以用`pip`命令代替其中的`pip3`。
