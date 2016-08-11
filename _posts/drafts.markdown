
# 如何让uWSGI使用默认的80端口

介绍在使用uWSGI的HTTP server时，如何启用80端口。

uWSGI是一个Python包，主要作为WSGI和uwsgi协议的实现，可以作为socket服务器和HTTP服务器，同时也可用于反向代理、消息队列、cron等任务。同时，也可配合nginx作为http服务器与Python应用程序的接口。

    
    
    sudo setcap 'cap_net_bind_service=+ep'  /path/to/your/venv/bin/uwsgi
    
    sudo apt-get install libcap2-bin
    
    which uwsgi
    /path/to/your/venv/bin/uwsgi
