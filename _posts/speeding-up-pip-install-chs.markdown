---
published: true
title: Speeding up your pip install command
layout: post
tags: [DevOps, Python, Aliyun, PyPI, pip, System Administration, 系统管理]
categories: [Configuration]
permalink: speeding_up_pip_install
---

# 用国内的PyPI镜像加速pip命令

## 序

本文以阿里云的镜像为例，展示如何修改默认的PyPI源，以加快开发和系统搭建的效率。

## 问题提出
无论是家里的开发环境，还是公司访问某些国外网站，用Python的pip/easy_install在更新时，只要是使用默认的pypi.python.org，速度永远非常慢，而且还时常断线。所以，需要修改PyPI的源，将其指向国内的镜像。

## 解决问题

### 具体方案

#### 1. 确定pip配置文件所在的目录和路径

本人倾向于使用全局配置，这样万一更换用户名后（比如从root换成普通用户），设置依然生效。你可从别的网站（https://pip.pypa.io/en/stable/user_guide/#configuration）上得到用户级的设置方式。

1. Linux下的路径： `/etc/pip.conf`
2. Mac OS下的路径：`/Library/Application Support/pip/pip.conf`
3. Windows （Win7或更高）下的路径：`C:\ProgramData\pip\pip.ini`

#### 2. 修改pip配置文件

用任意一种文本编辑工具将与OS对应的配置文件打开，并且加入如下内容

    [global]
    timeout = 60
    index-url = http://mirrors.aliyun.com/pypi/simple/
    trusted-host = mirrors.aliyun.com

### 其他方案

1. 除了aliyun以外，douban的云的使用可能更广泛。考虑到阿里云的镜像距离自己的公有主机更近，因此本文使用阿里云作为案例。读者有兴趣可以在网上搜索。

2. 有一个比较不同镜像更新效率的网站：https://www.pypi-mirrors.org 可以有兴趣看一下。似乎目前国内在积级更新的也只有阿里云和豆瓣了。豆瓣的更新速度一般在10分钟之内，非常快……

### 扩展讨论

1. 关于搜索

    > 虽然修改了软件源，但是`pip search`命令还是不能使用的，因为搜索软件使用的协议与安装软件不同。`pip search`基于xmlrpclib实现，`pip install`基于urllib2实现。同样地，对`pip search`设置代理，也是不起作用的。

    来源：http://www.jianshu.com/p/785bb1f4700d

2. 关于显示默认的源

    直到现在，pip都没有给出列出已经配置的源主机的命令。与之相对应的是，yum几乎每次执行时都会在屏幕上回显repository对应的url……呼吁pip的开发者加上此功能。
