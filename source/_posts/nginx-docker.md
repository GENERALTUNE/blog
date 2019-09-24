---
title: docker 安装 nginx
date: 2019-09-22 17:28:02
tags:
---
## 在Docker下载Nginx镜像
```
docker search nginx
docker pull nginx
docker images
```
## 创建挂载目录
```
mkdir -p /data/nginx/{conf,conf.d,html,logs}
```

## 编写nginx.conf配置文件
编写nginx,conf配置文件，并放在文件夹中
```
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;

    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  182.254.161.54;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
        proxy_pass http://pic; 
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

    upstream pic{
                server 182.254.161.54:8088 weight=5;
                server 182.254.161.54:8089 weight=5;
    }

}
```
## 启动容器
```
docker run --name mynginx -d -p 82:80  -v /data/nginx/conf/nginx.conf:/etc/nginx/nginx.conf  -v /data/nginx/logs:/var/log/nginx -d docker.io/nginx
```
## 查看启动的容器
```
docker ps 
```

## 进入容器并查看配置文件目录结构
```
docker run -i -t nginx /bin/bash
```
## copy容器里的配置文件到宿主机刚创建的文件夹下面
```

docker cp 1022c6f181b9:/etc/nginx/nginx.conf /data/nginx/conf/nginx.conf
 
docker cp 1022c6f181b9:/etc/nginx/conf.d/default.conf /data/nginx/conf.d/default.conf
```
- 编写default.conf配置文件
- 挂载并启动nginx
```
docker run -p 81:81 --name mynginx --privileged=true -v /data/nginx/html:/usr/share/nginx/html -v /data/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /data/nginx/conf.d/default.conf:/etc/nginx/conf.d/default.conf -v /data/nginx/logs:/var/log/nginx -d nginx

docker run -p 81:81 --name mynginx --privileged=true -v /data/nginx/html:/usr/share/nginx/html -v /data/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /data/nginx/conf.d/default.conf:/etc/nginx/conf.d/default.conf -v /data/nginx/logs:/var/log/nginx -d nginx


docker run -p 80:80 --name nginxforp --privileged=true -v /data/nginx/html:/usr/share/nginx/html -v /data/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /data/nginx/conf.d/default.conf:/etc/nginx/conf.d/default.conf -v /data/nginx/logs:/var/log/nginx -d nginx

```
- 编写好html文件放入  /data/nginx/html 里面，之后在浏览器输入 http://ip:端口号
命令说明：
–privileged=true    配置了nginx.conf的外部挂载 之后可能导致nginx不能启动，使用该命令；
-v /data/conf.d/default.conf:/etc/nginx/conf.d/default.conf    挂载默认配置文件
-v /data/conf/nginx.conf:/etc/nginx/nginx.conf    挂载nginx.conf文件
-v /data/logs:/var/log/nginx   挂载日志目录
-v /data/html:/usr/share/nginx/html   挂载html目录
 
挂载说明:
字符 ：之后的文件目录为nginx容器中的文件目录，需要依据配置文件，或官方说明进行配置，具体可参考https://hub.docker.com/_/nginx官方使用说明；


### win10 mingwin scp 到 centos7
```
pscp D:\cwm2.0\cwm-app.zip root@172.18.190.64:/home/djn/
root@172.18.190.64's password:
scp C:\Users\zhumin\Desktop\cas-6.0.5.tar.gz   root@149.28.16.96
```

### nginx 配置https
1. nginx.conf配置文件
```
server {
        listen       80;
        server_name www.abc.lynch.com abc.lynch.com;
        error_log   /usr/share/nginx/html/test/pay_local.error;
          client_max_body_size 60M;
        client_body_buffer_size 512k;
        location / {   
            root   /usr/share/nginx/html/kshop;
            index  index.html;
            autoindex  on;
        }
       # rewrite ^(.*) https://$server_name$1 permanent; 
}
server {
    listen       443    ssl;
    server_name www.abc.lynch.com abc.lynch.com;
        error_log   /usr/share/nginx/html/test/pay_local.error;
              client_max_body_size 60M;
        client_body_buffer_size 512k;
        location / {
                root   /usr/share/nginx/html/kshop;
                index  index.html;
                autoindex  on;
        }
    ssl_certificate /etc/nginx/conf.d/fullchain.pem; #证书位置
    ssl_certificate_key /etc/nginx/conf.d/privkey.pem; #私钥位置
}
```
2. 启动nginx，映射443端口
```
docker run --restart=always --name nginx_erp_test -d -p 80:80 -p 443:443 -v /www/html/attachment:/www/html/attachment -v /data/nginx/html:/usr/share/nginx/html -v /data/nginx/conf/nginx.conf:/etc/nginx/nginx.conf  -v /data/nginx/logs:/var/log/nginx -v /data/nginx/conf.d:/etc/nginx/conf.d nginx

docker run -p 80:80 -p 443:443 --name nginxforp --privileged=true -v /data/nginx/html:/usr/share/nginx/html -v /data/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /data/nginx/conf.d/default.conf:/etc/nginx/conf.d/default.conf -v /data/nginx/logs:/var/log/nginx -v /data/nginx/cert/2844601_gerante.com.key:/etc/nginx/cert/2844601_gerante.com.key -v /data/nginx/cert/2844601_gerante.com.pem:/etc/nginx/cert/2844601_gerante.com.pem -d nginx


```

目的：https方式访问网站

解决步骤：

1，首先得确保http访问该网站是没问题的。

2，配置nginx.conf监听443端口，443是ssl默认的端口

a，nginx.conf这个文件所在路径：/www/server/nginx/conf/nginx.conf（因为我是用宝塔面板安装的nginxa跟不用宝塔面板安装的路径会有所不同）

b，nginx.conf中http模块里的server模块是用来配置虚拟主机的，我们的ssl配置就要再server模块里完成，因为宝塔面板在创建网站的时候，就将每个虚拟主机的conf单独写出来了，然后在nginx.conf里include这些单独的conf。形如：include /www/server/panel/vhost/nginx/*.conf; 

c，需要修改你想要配置https的虚拟主机的conf文件，形如（需增加的配置）：

``` 
server {

    listen 443 ssl;

    server_name www.xxxxx.cn;这个域名必须是你申请ssl证书的时候绑定的域名

    ssl on;

    ssl_certificate /www/server/panel/vhost/cert/1682997_www.fancy56.cn.pem; #SSL 证书文件路径，由证书签发机构提供

    ssl_certificate_key /www/server/panel/vhost/cert/1682997_www.fancy56.cn.key; #SSL 密钥文件路径，由证书签发机构提供

    ssl_session_timeout 5m;

    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;

    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;

    ssl_prefer_server_ciphers on;

}

```
3，到云服务器的控制台，添加安全组规则，增加443端口

4，云服务器的防火墙开启443端口

firewall-cmd --zone=public --add-port=443/tcp --permanent  增加443端口

firewall-cmd --reload  重启防火墙

以上就是我在配置ssl的时候遇到的一些问题解决方式。