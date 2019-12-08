---
title: docker学习笔记
date: 2018-03-01 13:59:47
update:  
tags:  docker devops
---


### docker
Docker是什么呢？百度百科是这样跟我说的：Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的镜像中，然后发布到任何流行的 Linux或Windows 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口。
### 安装
去除之前安装的docker ,老版本的docker 可能叫 docker docker.io docker-engine
```
sudo apt-get remove docker docker-engine docker.io containerd runc
```
Update the apt package index:
```
$ sudo apt-get update
```
安装最新版本docker
```
sudo apt-get install docker-ce docker-ce-cli containerd.io
```
安装特定版本docker
```
apt-cache madison docker-ce //查询版本
sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
```
### 常用命令
上面说了那么多，下面就到了咱们的实操环节啦！这一节的内容会通过一些常用的命令让大家更进一步的了解docker，注意！！这里只是一些常用的命令来加深理解，而不是命令大全！如果没有安装docker的小伙伴可以自己按照官网的文档进行安装，本文不会讲到这部分的内容！所以我假设你在自己的服务器上已经装好了docker！
docker 官网地址：　https://docs.docker.com/install/linux/docker-ce/ubuntu/#set-up-the-repository

#### 帮助命令
  1、docker version 查看docker客户端和服务的版本。

  2、docker info 查看docker的基本信息，如有多少容器、多少镜像、docker根目录等等。

  3、docker --help 查看docker的帮助信息，这个命令可以查看所有docker支持的命令~

  这几个命令非常简单，有过一点linux基础的小伙伴应该很容易理解！
#### 镜像命令
1、docker images 查看本地主机上所有的镜像。注意是本地主机的！这里能看到镜像的名称、版本、id、大小等基本信息，注意这里的image ID是镜像的唯一标识！还可以通过docker images tomcat指定某个具体的镜像查看对应信息。这里还要注意的是centos的镜像才200MB的大小，比我们物理机器上装的centos要小得多的多，这是因为centos的镜像只保留了linux核心部分，这也是为什么docker虚拟化技术比虚拟机运行效率更高的原因！那为什么tomcat的镜像这么大呢？那是因为我们之前说过我们的镜像就像一个洋葱一样，是一层套一层的！tomcat的运行需要基于centos、jdk等等镜像，tomcat在上层所以体积比较大啦！
2、docker rmi 删除本地的镜像，如下图所示，可以加上-f参数进行强制删除。这里的rmi命令跟linux中的删除命令就很像啦，只是这里加了一个i代表image！
3、docker search 根据镜像名称搜索远程仓库中的镜像！
4、docker pull 搜索到某个镜像之后就可以从远程拉取镜像啦，有点类似咱们git中的pull命令，当然对应的还有个docker push的命令。如图，如果我们没有指定tag，默认就会拉取latest版本，也可以通过docker pull tomcat:1.7的方式拉取指定版本！注意这里在拉取镜像的时候打印出来的信息有很多，这也是前面说到的镜像是一层套一层，拉取一个镜像也是一层一层的拉取！

#### 容器命令
  通过镜像命令我们就能获取镜像、删除镜像等操作啦！镜像有了下面自然就需要通过镜像创建对应的实例啦，也就是我们的容器。下面我们以tomcat为例：

  1、docker run [OPTIONS] IMAGE [COMMAND] [ARG...] 可以基于某个镜像运行一个容器，如果本地有指定的镜像则使用本地镜像，如果没有则从远程拉取对应的镜像然后启动！由于这个命令非常重要，所以下面列出几个比较重要的参数：

-d:启动容器，并且后台运行（docker容器后台运行，就必须要有一个前台进程，容器运行的命令如果不是一直挂起的命令，容器启动后就会自动退出）；
-i:以交互模式运行容器，通常与-t同时使用；
-t:为容器重新分配一个伪输入终端，通常与-i同时使用（容器启动后进入到容器内部的命令窗口）；
-P:随机端口映射，容器内部端口随机映射到主机的高端口；
-p:指定端口映射，格式为：主机(宿主)端口:容器端口；
-v:建立宿主机与容器目录的同步；
--name="myTomcat": 为容器指定一个名称（如果不指定，则有个随机的名字）；

2、进入到容器后可以通过exit命令退出容器，也可以通过ctrl+P+Q快捷键退出容器，这两种方式的不同之处是exit会退出并且关闭容器，而ctrl+P+Q快捷键只是单纯的退出，容器还在运行，并且还能再次进入！

  3、docker ps我们可以通过该命令查看正在运行的容器的信息，这里能看到容器的唯一id，启动时间等等...这里跟linux的ps命令类似，所以也可以把容器理解为一个运行在docker上的进程！docker ps -a可以查看运行中与停止的所有容器。
4、docker attach [OPTIONS] CONTAINER上面说过通过ctrl+P+Q快捷键退出容器后容器还在后台运行，那如果想再次进入容器怎么办呢？我们就可以通过attach命令+容器的id再次进入容器！

5、docker exec [OPTIONS] CONTAINER这个命令与attach一样都可以再次进入后台运行的容器，但是该命令可以不进入容器而在运行的容器中执行命令！比attach更加强大！

6、docker stop docker kill docker restart这三个命令分别用来停止容器、强制停止容器和重启容器，就跟我们在linux上停止、强制停止和重启某个进程一样的啦，这里就不做演示了！

7、docker rm 使用这个命令就可以删除某个容器，这里跟删除镜像的区别是这里少了一个 i 啦！需要注意的是通过stop和kill停止的容器还存在于docker中，而使用 rm 命令操作后的容器将不再存在！
 8、docker inspect 查看容器的详情（也能查看镜像详情）。




Dockerfile
  前面我们对docker以及相关概念、常用命令有了基本的了解，我们也知道了可以从远程pull一个镜像，那远程的镜像是怎么来的呢？如果我们想自己创建一个镜像又该怎么做呢？
对，Dockerfile！Dockerfile是一个包含用户能够构建镜像的所有命令的文本文档，它有自己的语法以及命令，docker能够从dockerfile中读取指令自动的构建镜像！
  我们要想编写自己的Dockerfiler并构建镜像，那对Dockerfile的语法和命令的了解就是必须的，了解规则才好办事嘛！

相关指令
FROM
  FROM <image> [AS <name>]
  FROM <image>[:<tag>] [AS <name>]
  FROM <image>[@<digest>] [AS <name>]
  指定基础镜像，当前镜像是基于哪个镜像创建的，有点类似java中的类继承。FROM指令必是Dockerfile文件中的首条命令。

MAINTAINER
  MAINTAINER <name>
  镜像维护者的信息，该命令已经被标记为不推荐使用了。

LABEL
  LABEL <key>=<value> <key>=<value> <key>=<value> ...
  给镜像添加元数据，可以用LABEL命令替换MAINTAINER命令。指定一些作者、邮箱等信息。

ENV
  ENV <key> <value>
  ENV <key>=<value> ...
  设置环境变量，设置的变量可供后面指令使用。跟java中定义变量差不多的意思！

WORKDIR
  WORKDIR /path/to/workdir
  设置工作目录，在该指令后的RUN、CMD、ENTRYPOINT, COPY、ADD指令都会在该目录执行。如果该目录不存在，则会创建！

RUN
  RUN <command>
  RUN ["executable", "param1", "param2"]
  RUN会在当前镜像的最上面创建一个新层，并且能执行任何的命令，然后对执行的结果进行提交。提交后的结果镜像在dockerfile的后续步骤中可以使用。

ADD
  ADD [--chown=<user>:<group>] <src>... <dest>
  ADD [--chown=<user>:<group>] ["<src>",... "<dest>"]
  从宿主机拷贝文件或者文件夹到镜像，也可以复制一个网络文件！如果拷贝的文件是一个压缩包，会自动解压缩！

COPY
  COPY [--chown=<user>:<group>] <src>... <dest>
  COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]
  从宿主机拷贝文件或者文件夹到镜像，不能复制网络文件也不会自动解压缩！

VOLUME
  VOLUME ["/data"]
  VOLUME用于创建挂载点，一般配合run命令的-v参数使用。

EXPOSE
  EXPOSE <port> [<port>/<protocol>...]
  指定容器运行时对外暴露的端口，但是该指定实际上不会发布该端口，它的功能是镜像构建者和容器运行者之间的记录文件。
  回到容器命令中的run命令部分，run命令有-p和-P两个参数，如果是-P就是随机端口映射，容器内会随机映射到EXPOSE指定的端口，如果是-p就是指定端口映射，告诉运维人员容器内需要映射的端口号。

CMD
  CMD ["executable","param1","param2"]
  CMD ["param1","param2"]
  CMD command param1 param2
  指定容器启动时默认运行的命令,在一个Dockerfile文件中，如果有多个CMD命令，只有一个最后一个会生效！同样是可以执行命令，可能你会觉得跟上面的RUN指令很相似，RUN指令是在构建镜像时候执行的，而CMD指令是在每次容器运行的时候执行的！docker run命令会覆盖CMD的命令！

ENTRYPOINT
  ENTRYPOINT ["executable", "param1", "param2"]
  ENTRYPOINT command param1 param2
  这个指令与CMD指令类似，都是指定启动容器时要运行的命令，如果指定了ENTRYPOINT，则CMD指定的命令不会执行！在一个Dockerfile文件中，如果有多个ENTRYPOINT命令，也只有一个最后一个会生效！不同的是通过docker run command命令会覆盖CMD的命令！执行的命令不会覆盖ENTRYPOINT，docker run命令中指定的任何参数都会被当做参数传递给ENTRYPOINT！

RUN、CMD、ENTRYPOINT区别
1、RUN指令是在镜像构建时运行，而后两个是在容器启动时执行！
2、CMD指令设置的命令是容器启动时默认运行的命令,如果docker run没有指定任何的命令，并且Dockerfile中没有指定ENTRYPOINT，那容器启动的时候就会执行CMD指定的命令！有点类似代码中的缺省参数！
3、如果设置了ENTRYPOINT指令，则优先使用！并且可以通过docker run给该指令设置的命令传参！
4、CMD有点类似代码中的缺省参数

USER
  USER <user>[:<group>]
  USER <UID>[:<GID>]
  用于指定运行镜像所使用的用户。

ARG
  ARG <name>[=<default value>]
  指定在镜像构建时可传递的变量，定义的变量可以通过docker build --build-arg =的方式在构建时设置。

ONBUILD
  ONBUILD [INSTRUCTION]
  当所构建的镜像被当做其他镜像的基础镜像时，ONBUILD指定的命令会被触发！

STOPSIGNAL
  STOPSIGNAL signal
  设置当容器停止时所要发送的系统调用信号！

HEALTHCHECK
  HEALTHCHECK [OPTIONS] CMD command （在容器内运行运行命令检测容器的运行情况）
  HEALTHCHECK NONE （禁止从父镜像继承检查）
  该指令可以告诉Docker怎么去检测一个容器的运行状况！

SHELL
  SHELL ["executable", "parameters"]
  用于设置执行命令所使用的默认的shell类型！该指令在windows操作系统下比较有用，因为windows下通常会有cmd和powershell两种shell，甚至还有sh。

构建
  Dockerfile执行顺序是从上到下，顺序执行！每条指令都会创建一个新的镜像层，并对镜像进行提交。编写好Dockerfile文件后，就需要使用docker build命令对镜像进行构建了。
  docker build的格式：docker build [OPTIONS] PATH | URL | -

-f :指定要使用的Dockerfile路径，如果不指定，则在当前工作目录寻找Dockerfile文件！
-t: 镜像的名字及标签，通常 name:tag 或者 name 格式；可以在一次构建中为一个镜像设置多个标签。

  例如我们可以docker build -t myApp:1.0.1 .这样来构建自己的镜像，注意后面的 . ,用于指定镜像构建过程中的上下文环境的目录。如果大家想了解那些官方镜像的Dockerfile文件都是怎么样写的，可以上https://hub.docker.com/ 进行搜索，以tomcat镜像为例

增加docker用户组

1. Create the docker group.

```
$ sudo groupadd docker

```

2. Add your user to the docker group.

```
$ sudo usermod -aG docker $USER

```


### docker 安装mysql

#### 首先安装docker服务
1、Docker 要求 CentOS 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的CentOS 版本是否支持 Docker 。
通过 uname -r 命令查看你当前的内核版本
 ```
 uname -r  
 ```
 2、使用 root 权限登录 Centos。确保 yum 包更新到最新。
 ```shell
 sudo yum update
 ```
 3、卸载旧版本(如果安装过旧版本的话)
 ```shell
sudo yum remove docker  docker-common docker-selinux docker-engine
 ```
 4、安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的
```shell
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
 ```
5、设置yum源
 ```shell
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
 ```
 6、可以查看所有仓库中所有docker版本，并选择特定版本安装
  ```shell
yum list docker-ce --showduplicates | sort -r
 ```
 7、安装docker
```shell
 sudo yum install docker-ce  #由于repo中默认只开启stable仓库，故这里安装的是最新稳定版17.12.0
 sudo yum install <FQPN>  # 例如：sudo yum install docker-ce-17.12.0.ce
 ```
8、启动并加入开机启动 
```shell
 sudo systemctl start docker  //验证docker安装成功：
 sudo systemctl enable docker
 ```

 9、验证安装是否成功(有client和service两部分表示docker安装启动都成功了)
 ```shell
 docker version
 ```


使用Docker安装mysql，挂载外部配置和数据
```shell
mkdir /data
mkdir /data/mysql
mkdir /data/mysql/conf.d
mkdir /data/mysql/data/
```
 
创建my.cnf配置文件
touch /data/mysql/my.cnf

my.cnf添加如下内容：
[mysqld]
user=mysql
character-set-server=utf8
default_authentication_plugin=mysql_native_password
secure_file_priv=/var/lib/mysql
expire_logs_days=7
sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
max_connections=1000

[client]
default-character-set=utf8

[mysql]
default-character-set=utf8

2、创建容器，并后台启动
```shell
docker run --restart=always --privileged=true -d -v /data/mysql/data/:/var/lib/mysql -v /data/mysql/conf.d:/etc/mysql/conf.d -v /data/mysql/my.cnf:/etc/mysql/my.cnf -p 33060:3306 --name my-mysql -e MYSQL_ROOT_PASSWORD=123456 mysql
```
参数说明：
--restart=always： 当Docker 重启时，容器会自动启动。
--privileged=true：容器内的root拥有真正root权限，否则容器内root只是外部普通用户权限
-v /data/mysql/conf.d/my.cnf:/etc/my.cnf：映射配置文件
-v /data/mysql/data/:/var/lib/mysql：映射数据目录

进入容器，修改root用户允许远程访问，如下图所示

进入容器命令：docker exec -it 7681b85e73a1 /bin/sh 修改远程权限：alter user 'root'@'%' identified with mysql_native_password by 'root';

alter user 'root'@'%' identified with mysql_native_password by 'root'

修改密码
```shell
mysql> use mysql;
mysql> alter user 'root'@'%' identified with mysql_native_password by '123';
mysql> flush privileges;
mysql> select host,user,plugin,authentication_string from mysql.user;
```
#### docker中搜索可用镜像
```
docker search mysql
```
#### 拉取MySQL镜像
```
docker pull mysql:5.6
```
#### 查看MySQL镜像
```
 docker images
 //或者
 docker image ls
 
```
#### 列出正在运行的容器
```
docker ps
```
#### 运行MySQL
```
docker run --name mysql -e MYSQL_ROOT_PASSWORD=123456 -d -i -p 3306:3306 --restart=always  mysql:5.6
docker run -d -p 3306:3306 --name mymysql -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7.18
```
以上参数的含义：
--name mysql  将容器命名为mysql，后面可以用这个name进行容器的启动暂停等操作
-e MYSQL_ROOT_PASSWORD=123456 设置MySQL密码为123456
-d 此容器在后台运行,并且返回容器的ID
-i 以交互模式运行容器
-p 进行端口映射，格式为主机(宿主)端口:容器端口
--restart=always 当docker重启时，该容器自动重启

#### 进入MySQL容器
```
docker exec -ti mysql bash

```
#### 创建用户

```
CREATE USER 'usernamexxx'@'hostxxx' IDENTIFIED BY 'passwordxxx';

```
- hostxxx：指定该用户在哪个主机上可以登陆，如果是本地用户可用localhost，如果想让该用户可以从任意远程主机登陆，可以使用通配符%
- passwordxxx：该用户的登陆密码，密码可以为空，如果为空则该用户可以不需要密码登陆服务器
示例：
```
CREATE USER 'jack'@'localhost' IDENTIFIED BY '123456';
CREATE USER 'rose'@'192.168.38.110_' IDENDIFIED BY '123456';
CREATE USER 'rose'@'%' IDENTIFIED BY '123456';
CREATE USER 'rose'@'%' IDENTIFIED BY '';
CREATE USER 'rose'@'%';
```

授权
```
GRANT privilegesxxx ON databasenamexxx.tablenamexxx TO 'usernamexxx'@'hostxxx'
```
说明：

- privilegesxxx：用户的操作权限，如SELECT，INSERT，UPDATE等，如果要授予所的权限则使用ALL
- databasenamexxx：数据库名
- tablenamexxx：表名，如果要授予该用户对所有数据库和表的相应操作权限则可用*表示，如*.*
示例：
```
GRANT SELECT, INSERT ON DbXXX.user TO 'jack'@'%';
GRANT ALL ON *.* TO 'jack'@'%';
GRANT ALL ON DbXXX.* TO 'jack'@'%';
```
注意：

1.  授权之后需要用户重连MySQL，才能获取相应的权限。

2. 用以上命令授权的用户不能给其它用户授权，如果想让该用户可以授权，用以下命令:
```
GRANT privilegesxxx ON databasenamexxx.tablenamexxx TO 'usernamexxx'@'hostxxx' WITH GRANT OPTION;


SET PASSWORD FOR 'usernamexxx'@'hostxxx' = PASSWORD('newpasswordxxx');
```
如果是当前登陆用户用:
```
SET PASSWORD = PASSWORD("newpasswordxxx");
```
示例：
```
SET PASSWORD FOR 'jack'@'%' = PASSWORD("123456");
```
撤销用户权限
```
REVOKE privilegexxx ON databasenamexxx.tablenamexxx FROM 'usernamexxx'@'hostxxx';
```
示例：
```
REVOKE SELECT ON *.* FROM 'jack'@'%';
```
注意：

假如你在给用户'jack'@'%'授权的时候是这样的（或类似的）：
GRANT SELECT ON test.user TO 'jack'@'%'，则在使用REVOKE SELECT ON *.* FROM 'jack'@'%';命令并不能撤销该用户对test数据库中user表的SELECT 操作。相反，如果授权使用的是GRANT SELECT ON *.* TO 'jack'@'%';则REVOKE SELECT ON test.user FROM 'jack'@'%';命令也不能撤销该用户对test数据库中user表的Select权限。


具体信息可以用命令SHOW GRANTS FOR 'jack'@'%'; 查看。

删除用户
```
DROP USER 'usernamexxx'@'hostxxx';
```




#### 开放端口

使用方法如下：

>关闭防火墙
```
systemctl stop firewalld.service             #停止firewall
systemctl disable firewalld.service        #禁止firewall开机启动
```
> 开启端口
firewall-cmd --zone=public --add-port=80/tcp --permanent
 命令含义：
--zone #作用域
--add-port=80/tcp #添加端口，格式为：端口/通讯协议
--permanent #永久生效，没有此参数重启后失效
> 重启防火墙
```
firewall-cmd --reload
```

```
 firewall-cmd --zone=public --add-port=3306/tcp --permanent

```
常用命令介绍
firewall-cmd --state                           ##查看防火墙状态，是否是running
firewall-cmd --reload                          ##重新载入配置，比如添加规则之后，需要执行此命令
firewall-cmd --get-zones                       ##列出支持的zone
firewall-cmd --get-services                    ##列出支持的服务，在列表中的服务是放行的
firewall-cmd --query-service ftp               ##查看ftp服务是否支持，返回yes或者no
firewall-cmd --add-service=ftp                 ##临时开放ftp服务
firewall-cmd --add-service=ftp --permanent     ##永久开放ftp服务
firewall-cmd --remove-service=ftp --permanent  ##永久移除ftp服务
firewall-cmd --add-port=80/tcp --permanent     ##永久添加80端口 
iptables -L -n                                 ##查看规则，这个命令是和iptables的相同的
man firewall-cmd                               ##查看帮助

更多命令，使用  firewall-cmd --help 查看帮助文件

重新开启防火墙：Failed to start firewalld.service: Unit firewalld.service is masked 问题解决：

https://blog.csdn.net/Joe68227597/article/details/75207859

> CentOS 7.0默认使用的是firewall作为防火墙，使用iptables必须重新设置一下
1、直接关闭防火墙
systemctl stop firewalld.service           #停止firewall
systemctl disable firewalld.service     #禁止firewall开机启动

2、设置 iptables service
yum -y install iptables-services
如果要修改防火墙配置，如增加防火墙端口3306
vi /etc/sysconfig/iptables 
增加规则
-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT

保存退出后
systemctl restart iptables.service #重启防火墙使配置生效
systemctl enable iptables.service #设置防火墙开机启动
最后重启系统使设置生效即可。

#### 指定配置文件和存储文件
1. 列出正在运行的容器  docker ps
2. 进入容器 `docker exec -it e1066fe2db35 /bin/bash`
3. 查看配置文件  `/etc/mysql/mysql.conf.d/mysqld.cnf`
4. 配置文件内容：
```
[mysqld]
pid-file    = /var/run/mysqld/mysqld.pid
socket      = /var/run/mysqld/mysqld.sock
datadir     = /var/lib/mysql
#log-error  = /var/log/mysql/error.log
# By default we only accept connections from localhost
#bind-address   = 127.0.0.1
# Disabling symbolic-links is recommended to prevent assorted security risks
#symbolic-links=0

```
5. 停止并删除容器
```
docker stop e1066fe2db35
docker rm e1066fe2db35
```
6. 重新启动容器，指定数据目录和配置文件
```
docker run -d -p 3306:3306 -v /soft/mysql/my.cnf:/etc/mysql/mysql.conf.d/mysqld.cnf -v /soft/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mymysql mysql:5.7.18

```
7. 查看sql_mode
```
mysql> SELECT @@GLOBAL.sql_mode;
```




### 参考

[看完此文，妈妈还会担心你docker入不了门？](http://www.17coding.info/article/24)
