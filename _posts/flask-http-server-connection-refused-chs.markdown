---
published: true
title: Flask拒绝连接的故障及解决方案
layout: post
tags: [DevOps, Flask, HTTP server, Python, connection refused, troubleshooting]
categories: [Configuration]
permalink: flask-http-server-connection-refused-chs
---
# 一个Python Flask拒绝连接 (connection refused) 的故障及解决方案

## 故障描述

一个基于Flask框架的Python程序，在开发本地机调试时一切正常。然而，在部署到远程服务器时，浏览器或任何Restful客户端在调用Flask自带的http均显示“connection refused / 连接被拒绝”。

## tl;dr / 简版解决方案：

两种解决方案：
1. （推荐方案）使用uWSGI或者其他的HTTP服务器，通过uwsgi协议或WSGI协议与Python通信，从而避免Flask自己的HTTP服务器。
2. （短平快方案）如果为了简便需要使用Flask的服务器，将原有的Flask启动方式为`app.run()`或`app.run(host='127.0.0.1')`，需要将之改为`app.run(host='0.0.0.0)`

## 详细过程：

#### telnet以获取更多信息

之前用浏览器测试；改用`telnet 123.45.678.90 8888` （本文中所出现的IP和端口号只为演示说明用，非真实数据），有如下发现：

1. 开发机在启动本地HTTP服务器后，`telnet 127.0.0.1 8888`正常
2. 在用SSH连接的服务器本机上，`telnet 127.0.0.1 8888`正常
3. 在用SSH连接的服务器本机上，`telnet 123.45.678.90 8888`失败，拒绝连接
4. 在用SSH连接的第三方服务器上，`telnet 123.45.678.90 8888`失败，拒绝连接

#### IP与端口

首先怀疑的是IP与端口是否正确。反复确认之后，IP与port均正确。因此将之排除

#### Flask的SERVER_NAME设置

在[Flask的设置文档](http://flask.pocoo.org/docs/0.11/config/)中，介绍了`SERVER_NAME`。该参数如果被不当设置，也会造成HTTP服务器无法访问。但是此参数主要在同一主机上部署多个不同站点，即通过TCP连接之后输入的host区分，故不会一开始就拒绝TCP连接。

此外，也人工复查SERVER_NAME设置，确认无误。

#### 防火墙

从telnet的结果上看，HTTP机器依然在运转，重点放在防火墙上。通过`sudo iptables -L`发现没有设置任何规则。机器是新配置的服务器，防火墙没有人为地设置过。

同时`netstat -an`发现，`tcp   0   0   127.0.0.1:8888   0.0.0.0:*   LISTEN`，说明Python进程下HTTP server处于正常的listen状态

然而，通过netstat返回的结果，我们似乎发现了问题的真正端倪……

#### 网卡的interfaces

netstat命令可以列出本机的所有正在运行中的网络服务。这些服务的本地IP，有些是显示`0.0.0.0`，有些显示公网IP即`123.45.678.90`，有些显示类似于`10.x.x.x`的内网IP，还有些直接显示`127.0.0.1`这样的本机IP。

这实际上由云端服务器的特性决定，同一个物理网卡可以虚拟出不同的interfaces，像`0.0.0.0`就是一个interfaces，代表不受限地可以和全网通信；而那些挂载在`10.x.x.x`接口上的服务们只能和内网IP通信；另外，还有`127.0.0.1`的只能和本机loopback进行通信。
并且需要注意的是，即使通信的来源也同样是本机，那么发起连接的客户端必须也要用`127.0.0.1`这个IP，而不能用别的IP。不幸的是，我们的Flask这个HTTP服用器恰恰“选择”了最受限制的`127.0.0.1`……

这一点可以从Flask的启动信息中可以看出，当Flask在启动时，会显示：

     * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)

此时，解决方案，就是在`app.run`时指定`host`这个参数，即`app.run(host='0.0.0.0)`。如果不指定，默认就会出现开头的故障。

