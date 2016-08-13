
# 如何让uWSGI使用默认的80及443端口

介绍在使用uWSGI的HTTP server时，如何启用默认的80端口（HTTP协议）和443号端口（HTTPS协议）。

uWSGI是一个Python包，主要作为WSGI和uwsgi协议的实现，可以作为socket服务器和HTTP服务器，同时也可用于反向代理、消息队列、cron等任务。同时，也可配合nginx作为http服务器与Python应用程序的接口。

HTTP协议默认为80端口，HTTPS默认为443端口。这两个都是特权端口(priveledged port)，普通模式下uwsgi无法使用，需要做如下特殊处理
    
    
    sudo setcap 'cap_net_bind_service=+ep'  /path/to/your/venv/bin/uwsgi
    
    sudo apt-get install libcap2-bin
    
    which uwsgi
    /path/to/your/venv/bin/uwsgi

这项上述脚本成功执行后，将会同时打开80和443两个端口，极大地方便了系统管理员。

另，有介绍用shared socket实现，比如:

> 80端口的HTTP服务器：
> 
>     uwsgi --shared-socket 0.0.0.0:80 --uid roberto --gid roberto --http =0
> 
> 
> 443端口的HTTPS服务器：
> 
>     uwsgi --shared-socket 0.0.0.0:443 --uid roberto --gid roberto --https =0,foobar.crt,foobar.key
> 

