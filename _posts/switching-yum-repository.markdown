---
published: true
title: Switching Centos 7 Yum Repository to Aliyun
layout: post
tags: [DevOps, Centos, RHEL-based Linux, Yum Repository, 阿里云, 系统管理]
categories: [Configuration]
permalink: switching-centos-7-yum-repository-to-aliyun-chs
---
# 如何将Centos 7的yum源更换为阿里云

> 为了提高yum包管理器的下载速度，本文演示了如何将Centos 7主机上将Yum源更换为阿里云，

## 序

由于特殊国情，国内公有云主机在通过Yum访问国外的源时面临速度慢或访问不稳定的痛点。因此，系统管理员们需要将Yum源（Yum Repository）更换为距离主机更近的镜像。

本文将以某Centos的实例为样板，介绍如何在将yum源更换为阿里云
* 由于阿里云自身的“公共镜像”的Centos或Aliyun Linux已经预先将Yum镜像指向阿里云，因此相关的用户一般无须进行本文中的操作。除非之前已经改过。
* 本文所阐述的方法不局限于Centos 7，其同样也适用于版本的Centos以及其他RHEL-Based Linux，如Fedora, RHEL, Aliyun Linux等操作系统。但是相关的网址需要进行微调
* 目标的镜像可以不局限于阿里云，比如也可以指向其他国内的公有云。其原理大同小异。具体的yum源的地址请自行查阅相关说明，本文不再赘述。

## 目标读者
* 需要更改Yum源的公有云用户

## 具体操作

    sudo mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup

备份CentOS-Base.repo。这个源文件一般不大

    sudo wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

将Centos-7.repo下载，并重命名为CentOS-Base.repo。里面包含了base, updates, extra等包组的源

    sudo wget -P /etc/yum.repos.d/ http://mirrors.aliyun.com/repo/epel-7.repo

一般地，除了CentOS-Base.repo以外，我们还需要Centos 7的EPEL，因此下载之
    
    sudo yum clean all
    sudo yum makecache

清空并重建缓存    
    
    sudo yum -y update
    
更新已安装的所有包。
