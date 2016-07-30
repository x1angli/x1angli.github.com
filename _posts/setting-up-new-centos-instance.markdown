---
published: true
title: Setting Up a new Centos Instance
layout: post
tags: [DevOps, Centos, Linux, Server provisioning, IAAS, 系统管理]
categories: [Configuration]
permalink: setting-up-new-centos-instance-chs
---
# 在公有云上配置新的Centos实例

> 本文将以阿里云的Centos 7.2 64位ECS实例为样板，介绍如何配置一台新的Centos主机

## 序

虽然网上有同类教程作为参考，其目标受从主要还是面向国外的公有云用户。由于语言及特殊国情，国内的开发者及系统管理员需要有适合自己的教程和样例。因此，本人将在

    adduser alex
    passwd alex
    'setting the password
    gpasswd -a alex wheel
    
    # log in as "alex"
    su - alex
    mkdir .ssh
    chmod 700 .ssh
    vi .ssh/authorized_keys
    # Enter insert mode, by pressing i, 
    # then paste the public key into the editor.  注意文件格式，puttygen的UI上的多行文本框的格式，而不是pem文件的格式
    # hit ESC to leave insert mode.
    # Enter :x then ENTER to save and exit the file.
    chmod 600 .ssh/authorized_keys
    exit
    
    # log in as "root"
    vi /etc/ssh/sshd_config
    # "/PermitRoot" to search, "/" is the search hotkey
    # Enter insert mode, by pressing i, 
    #  set the "PermitRootLogin no"
    # hit ESC to leave insert mode.
    # Enter :x then ENTER to save and exit the file.
    systemctl reload sshd
    
    =====================================
    sudo systemctl start firewalld
    # type in password
    sudo firewall-cmd --permanent --add-service=ssh
    sudo firewall-cmd --permanent --add-service=http
    sudo firewall-cmd --permanent --add-service=https
    ' sudo firewall-cmd --permanent --add-port=5000/tcp
    sudo firewall-cmd --permanent --add-port=80/tcp
    sudo firewall-cmd --permanent --list-all
    sudo firewall-cmd --reload
    sudo systemctl enable firewalld
    
    ----------------------------------
    sudo yum install ntp
    sudo systemctl start ntpd
    sudo systemctl enable ntpd
    ----------------------------------
    sudo fallocate -l 4G /swapfile
    sudo chmod 600 /swapfile
    sudo mkswap /swapfile
    sudo swapon /swapfile
    sudo sh -c 'echo "/swapfile none swap sw 0 0" >> /etc/fstab'
    
    =====================================

    sudo yum clean all
    sudo yum makecache
    sudo yum -y update

