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

### 参考

[看完此文，妈妈还会担心你docker入不了门？](http://www.17coding.info/article/24)
