---
title: shell 命令记录
date: 2017-03-13 10:28:22
tags:
---

resin 的启动
service resin start
service resin status 


CENTOS  开放端口号
firewall-cmd --permanent --add-port=6600/tcp

firewall-cmd --permanent --query-port=6800/tcp

firewall-cmd --list-ports

firewall-cmd --reload

Mongodb  安装启动 配置
设置mongodb的yum安装源
Create a /etc/yum.repos.d/mongodb-org-3.2.repo file so that you can install MongoDB directly, using yum.

[mongodb-org-3.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.2.asc

sudo yum install -y mongodb-org  yum 方法安装
/etc/mongod.conf   配置文件地址  
默认端口：27017 by default.
sudo service mongod start
sudo service mongod restart
日志
/var/log/mongodb/mongod.log
数据
/var/lib/mongo

sudo chkconfig mongod on 设置开机启动

sudo yum erase $(rpm -qa | grep mongodb-org)

sudo rm -r /var/log/mongodb
sudo rm -r /var/lib/mongo


Mongodb  数据库操作

导入数据
移除数据先
mongoimport --db test --collection restaurants --drop --file primer-dataset.json

设置登录口令后登录命令
mongo -port 27017 -u "myadmin" -p "zhumin"   --authenticationDatabase "admin"


最小化安装完centos 7 之后 需要做的事情
1. 配置静态 ip  /etc/sysconfig/network-scripts/ 目录下 ipcfg-enXX 文件 查看ip 命令  ip addr show

  一个静态ip 配置案例
   TYPE="Ethernet"
   BOOTPROTO="static"
   IPADDR=10.0.0.10
   NETWORKING=yes
  GATEWAY=192.168.1.1
 NETMASK=255.255.255.0
 NM_CONTROLLED=noa
 DEFROUTE="yes"
 PEERDNS="yes"
 PEERROUTES="yes"
 IPV4_FAILURE_FATAL="no"
 IPV6INIT="yes"
 IPV6_AUTOCONF="yes"
 IPV6_DEFROUTE="yes"
 IPV6_PEERDNS="yes"
  IPV6_PEERROUTES="yes"
 IPV6_FAILURE_FATAL="no"
 NAME="enp0s3"
 UUID="17eeb7fe-f11c-4b8b-83be-a9dd2281dda2"
 DEVICE="enp0s3"
 ONBOOT="yes"

2. 安装软件
      1). 安装 links  浏览工具  yum install links
      2). 安装 Apache HTTP 服务器 yum istall httpd 如果你想更改 Apache HTTP 服务器的默认端口号(80)为其它端口，你需要编辑配置文件 ‘/etc/httpd/conf/httpd.conf’ 并查找以下面开始的行：
            1.LISTEN 80
     把端口号 ‘80’ 改为其它任何端口(例如 3221)，保存并退出。
     增加刚才分配给 Apache 的端口通过防火墙，然后重新加载防火墙。

    允许 http 服务通过防火墙(永久)。
     1.# firewall-cmd –add-service=http

		     允许 3221 号端口通过防火墙(永久)。
    1.# firewall-cmd –permanent –add-port=3221/tcp

     重新加载防火墙。
	     1.# firewall-cmd –reload
	     完成上面的所有事情之后，是时候重启 Apache HTTP 服务器了，然后新的端口号才能生效。
     1.# systemctl restart httpd.service
        3). yum install lrzsz  
   4). export JAVA_HOME=/usr/local/java/jdk1.8.0_11 
          export JRE_HOME=/usr/local/java/jdk1.8.0_11/jre 
                export PATH=$PATH:/usr/local/java/jdk1.8.0_11/bin 
            export CLASSPATH=./:/usr/local/java/jdk1.8.0_11/lib:/usr/local/java/jdk1.8.0_11/jre/lib
       5). 安装编译环境  $ yum install ncurses-devel  $ yum install gcc

      3.新增用户  useradd  xxxxx  设置密码  passwd xxxxx
      4. 防火墙   开启80 端口 firewall-cmd --zone=public --add-port=80/tcp --permanent
         重启防火墙生效：firewall-cmd --reload
  5. 更新或升级最小化安装的 CentOS  yum update && yum upgrade

   jdk1.8.0_111


     修改系统环境变量文件
     vi + /etc/profile


     向文件里面追加以下内容：
     JAVA_HOME=/usr/java/jdk1.8.0_25
      JRE_HOME=/usr/java/jdk1.8.0_25/jre
      PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
 CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
  export JAVA_HOME JRE_HOME PATH CLASSPATH





 192.168.1.129
  安装nginx

  首先由于nginx的一些模块依赖一些lib库，所以在安装nginx之前，必须先安装这些lib库，这些依赖库主要有g++、gcc、openssl-devel、pcre-devel和zlib-devel 所以执行如下命令安装

  yum install gcc-c++  
 yum install pcre pcre-devel  
 yum install zlib zlib-devel  
  yum install openssl openssl--devel

		  二、安装Nginx

  安装之前，最好检查一下是否已经安装有 nginx
  find -name nginx

	  如果系统已经安装了nginx，那么就先卸载
	  yum remove nginx

  首先进入 /usr/local 目录
 cd /usr/local
  从官网下载最新版的nginx
 wget http://nginx.org/download/nginx-1.9.6.tar.gz
  tar -zxvf nginx-1.9.6.tar.gz
 cd nginx-1.9.6

 接下来安装，使用--prefix参数指定nginx安装的目录,make、make install安装

 ./configure  $默认安装在/usr/local/nginx   
 make  
  make install

  如果没有报错，顺利完成后，最好看一下 nginx 的安装目录
  whereis nginx

 安装完毕后，进入安装后目录（/usr/local/nginx）便可以启动或停止它了

 使用命令编译nginx

 ./configure --sbin-path=/usr/local/nginx/nginx --conf-path=/usr/local/nginx/nginx.conf --pid-path=/usr/local/nginx/nginx.pid --with-http_ssl_module --with-pcre=/usr/local/src/pcre-8.38 --with-zlib=/usr/local/src/zlib-1.2.8 --with-openssl=/usr/local/src/openssl-1.0.1q
  make 
 make install

  configure命令是用来检测你的安装平台的目标特征的。它定义了系统的各个方面，包括nginx的被允许使用的连接处理的方法，比如它会检测你是不是有CC或GCC，并不是需要CC或GCC，它是个shell脚本，执行结束时，它会创建一个Makefile文件。nginx的configure命令支持以下参数：
  –prefix=path    定义一个目录，存放服务器上的文件 ，也就是nginx的安装目录。默认使用 /usr/local/nginx。
 –sbin-path=path 设置nginx的可执行文件的路径，默认为  prefix/sbin/nginx.
  –conf-path=path  设置在nginx.conf配置文件的路径。nginx允许使用不同的配置文件启动，通过命令行中的-c选项。默认为prefix/conf/nginx.conf.
  –pid-path=path  设置nginx.pid文件，将存储的主进程的进程号。安装完成后，可以随时改变的文件名 ， 在nginx.conf配置文件中使用 PID指令。默认情况下，文件名 为prefix/logs/nginx.pid.
  –error-log-path=path 设置主错误，警告，和诊断文件的名称。安装完成后，可以随时改变的文件名 ，在nginx.conf配置文件中 使用 的error_log指令。默认情况下，文件名 为prefix/logs/error.log.
  –http-log-path=path  设置主请求的HTTP服务器的日志文件的名称。安装完成后，可以随时改变的文件名 ，在nginx.conf配置文件中 使用 的access_log指令。默认情况下，文件名 为prefix/logs/access.log.
 –user=name  设置nginx工作进程的用户。安装完成后，可以随时更改的名称在nginx.conf配置文件中 使用的 user指令。默认的用户名是nobody。
 –group=name  设置nginx工作进程的用户组。安装完成后，可以随时更改的名称在nginx.conf配置文件中 使用的 user指令。默认的为非特权用户。
  –with-select_module –without-select_module 启用或禁用构建一个模块来允许服务器使用select()方法。该模块将自动建立，如果平台不支持的kqueue，epoll，rtsig或/dev/poll。
  –with-poll_module –without-poll_module 启用或禁用构建一个模块来允许服务器使用poll()方法。该模块将自动建立，如果平台不支持的kqueue，epoll，rtsig或/dev/poll。
 –without-http_gzip_module — 不编译压缩的HTTP服务器的响应模块。编译并运行此模块需要zlib库。
  –without-http_rewrite_module  不编译重写模块。编译并运行此模块需要PCRE库支持。
 –without-http_proxy_module — 不编译http_proxy模块。
  –with-http_ssl_module — 使用https协议模块。默认情况下，该模块没有被构建。建立并运行此模块的OpenSSL库是必需的。
  –with-pcre=path — 设置PCRE库的源码路径。PCRE库的源码（版本4.4 – 8.30）需要从PCRE网站下载并解压。其余的工作是Nginx的./ configure和make来完成。正则表达式使用在location指令和 ngx_http_rewrite_module 模块中。
  –with-pcre-jit —编译PCRE包含“just-in-time compilation”（1.1.12中， pcre_jit指令）。
  –with-zlib=path —设置的zlib库的源码路径。要下载从 zlib（版本1.1.3 – 1.2.5）的并解压。其余的工作是Nginx的./ configure和make完成。ngx_http_gzip_module模块需要使用zlib 。
  –with-cc-opt=parameters — 设置额外的参数将被添加到CFLAGS变量。例如,当你在FreeBSD上使用PCRE库时需要使用:–with-cc-opt=”-I /usr/local/include。.如需要需要增加 select()支持的文件数量:–with-cc-opt=”-D FD_SETSIZE=2048″.
 –with-ld-opt=parameters —设置附加的参数，将用于在链接期间。例如，当在FreeBSD下使用该系统的PCRE库,应指定:–with-ld-opt=”-L /usr/local/lib”.
  说明: 说明: 若安装时找不到上述依赖模块，使用–with-openssl=<openssl_dir>、–with-pcre=<pcre_dir>、–with-zlib=<zlib_dir>指定依赖的模块目录。如已安装过，此处的路径为安装目录；若未安装，则此路径为编译安装包路径，nginx将执行模块的默认编译安装。s






																							    



																							     


																							     db.createUser({
																							         user: "root",
																								     pwd: "zhumin",
																								         roles: [{role:"root", db: "admin"}]
																									 });
																									 db.createUser( 
																									 { 
																									 user: "mongoRoot",
																									 pwd: "testMongoDB2016",
																									 roles:[{role:"userAdminAnyDatabase", db:"admin"}] 
																									 } 
																									 );
																									 ]

																									 rpm -e --nodeps mysql 


