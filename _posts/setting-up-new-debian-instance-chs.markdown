---
published: true
title: Setting Up New Debian Instance
layout: post
tags: [DevOps, Debian, Debian, Linux, System Administration, 系统管理]
categories: [Configuration]
permalink: setting-up-new-debian-instance-chs
---
# 架设全新的Debian 8主机

> 本文将以阿里云的Debian 8 64位ECS实例为样板，介绍如何在Centos主机上安装Python 3.x，及pip/virtualenv等必备组件

## 目标读者

使用Debian Linux的的程序员、系统管理员。

## 大致：
ssh root@SERVER_IP_ADDRESS
echo -e "PASSWORD\nPASSWORD"|sudo adduser USERNAME --gecos ""
apt-get update -y
apt-get install sudo -y
usermod -a -G sudo USERNAME
su - USERNAME
mkdir .ssh
chmod 700 .ssh
nano .ssh/authorized_keys
# copy.....
chmod 600 .ssh/authorized_keys
exit
nano /etc/ssh/sshd_config
#PermitRootLogin yes
#PermitRootLogin no
systemctl restart ssh


