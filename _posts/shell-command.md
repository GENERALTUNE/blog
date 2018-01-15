---
title: shell_command
date: 2018-01-13 14:30:07
tags:
---

### 监控CPU使用情况　uptime 命令
该命令的描述为：　打印当前时间，系统已经运行了多久，当前登录用户数以及系统平均负载．

```
[root@centos7 ~]# uptime

```
### 监控内存及交换分区使用情况－－　free 命令
该命令的描述为：　显示系统内存及交换分区信息
用法：　feee [-b |-k | -m]

```
[root@centos7 ~]# free -b

```
### 监控磁盘使用情况　－－　df

该命令的描述为：　生成系统磁盘空间的使用量信息．
用法：　df[选项]...
选项：　-h 人性化方式显示容量信息
        -i 显示磁盘inode使用量信息
        -T 显示文件系统类型
```
[root@centos7 ~]# df -hT

[root@centos7 ~]# df -i

```

### 监控网络使用情况　－－　ip 和 netstat命令
ip命令可以查看网卡接口信息，　netstat 命令查看服务器开启的端口信息以及网络连接状态

描述：　netstat 打印网络连接、路由表、网络接口统计等信息．
用法：　netstat[选项]
选项：　-s 显示各种协议数据统计信息
　　　　-n 使用数字形式的IP、端口号、用户ID替代主机、协议、用户等名称信息
　　　　-p 显示今晨该名称及对应进程ID号
　　　　-l 仅显示正在监听的sockets 接口信息
　　　　-u 查看udp连接信息
　　　　-t 查看tcp连接信息

```
[root@centos7 ~]# ip a s
[root@centos7 ~]# ip -s link show enol17773
[root@centos7 ~]# netstat -nutlp
[root@centos7 ~]# netstat -s

```

### 监控进程使用情况　－－　ps 和　top命令
1. ps命令
　描述：　查看当前进程信息
　用法：　ps命令版本众多，有多种语法种类，如UNIX、BSD、以及GNU linux

```
[root@centos7 ~]# ps -e  #查看所有的进程信息
[root@centos7 ~]# ps -ef #全格式显示进程信息

```
2. top命令
描述：　动态查看进程信息
选项：　-d #top刷新间隔　，默认为3秒
        -p #查看指定PID的进程信息


## 网络配置

### 网络接口参数 －－ ifconfig 命令
描述：　显示或设置网络接口信息
用法：　ifconfig interface 选项 | 地址

### 主机名参数　－－　hostnamectl命令

```
[root@centos7 ~]# hostnamectl status #查看主机名称和主机信息
[root@centos7 ~]# hostnamectl 

```
### 路由参数－－　route 命令
描述：　显示或设置静态IP路由表．
用法：　route[选项]      #查看路由信息
        route add 目标网络　gw 网关地址　
        route del 目标网络
```
[root@centos7 ~]# route   
[root@centos7 ~]# route -n #使用数字地址替代主机名称
[root@centos7 ~]# route add default gw 192.168.0.254  #添加默认网关为192.168.0.254
[root@centos7 ~]# route add -net 172.16.0.0/16 gw 192.168.0.254  #添加指定网段的网关
[root@centos7 ~]# route add -net 192.56.76.0 netmask 255.255.255.0 dev eno16777736   # 添加路由记录，指定通过eno16777736 网卡传输到192.56.76.0网段的数据
[root@centos7 ~]# route del default gw 192.168.0.254 #删除默认网关
[root@centos7 ~]# route del -net 172,16,0.0/16 #删除指定网段的网关记录
```
### 网络故障排错
1. ping
2. traceroute 
3. nslookup
4. dig
5. netstat

```
[root@centos7 ~]# ping 127.0.0.1
[root@centos7 ~]# traceroute -I www.google.com
[root@centos7 ~]# nslookup www.google.com
[root@centos7 ~]# dig www.google.com

```
## 内核模块
### 查看已加载的内核模块

```
[root@centos7 ~]# lsmod 

```

## Bash 技巧

| 快捷键 | 功能描述 |
| :-: | :-: |
| Ctrl + a | 光标移动到行首|
| Ctrl + e | 光标移动到行尾|
| Ctrl + f | 光标右移动一个字符|
| Ctrl + b | 光标左移动一个字符|
| Ctrl + l | 清屏 等同与　clear命令|
| Ctrl + u | 删除光标至行首的字符 |
| Ctrl + k | 删除光标至行尾的字符 |
| Ctrl + c | 终止进程 |
| Ctrl + z | 挂起进程(jobs命令查看挂起的进程) |
| Ctrl + w | 删除光标前一个单词(以空格为分隔符) |
| Alt  + d | 删除光标后一个单词 |
| Tab | 自动补齐 |

```
[root@centos7 ~]# id tom >> user 2>> error
[root@centos7 ~]# firefox   # 火狐浏览器通过前端启动，　使当前Shell将暂时无法使用
[root@centos7 ~]# firefox &  # 后台运行浏览器，不影响当前Shell的使用  jobs查看后台运行的程序  fg 调用后台的程序 
[root@centos7 ~]# ls /tmp; ls /root ; ls /home 所有的命令按顺序执行(不管前面的命令是否成功,后面的命令一定正常执行)
[root@centos7 ~]# ls test.txt && cat test.txt #仅当ls执行成功才会执行cat
[root@centos7 ~]# gedit || vim  # 如果有gedit编辑器,则打开该程序,否则打开vim编辑器
[root@centos7 ~]# id tom &>/dev/null && echo "Hi, tom" || echo "No Such user"

```
### 花括号{}的使用技巧

```
[root@centos7 ~]# echo {a, b, c}
a b c

[root@centos7 ~]# echo user{1, 5, 8}
user1 user5 user8

[root@centos7 ~]# echo {1..10}
0 1 2 3 4 5 6 7 8 9 

[root@centos7 ~]# mkdir  /tmp/{dir1, dir2, dir3}
[root@centos7 ~]# chmod  tmp/{dir1, dir2, dir3}

```

### 变量
name=[value]

```
[root@centos7 ~]# NAME=tomcat
[root@centos7 ~]# echo $NAME
[root@centos7 ~]# typeset -r NAME #添加readonly 只读属性
[root@centos7 ~]# set   #查看当前环境下的变量

```

使用name=[value]的形式定义的变量默认值仅在当前的Shell中有效, 子进程不会继承这样的变量,使用export命令会将变量放入环境中,这样新的进程会从父进程哪里继承环境,export 可以直接定义环境变量并赋值,也可以先定义一个普通的变量,再通过export 转换为环境变量

```
[root@centos7 ~]# export TEST #将已有用户变量添加至环境
[root@centos7 ~]# export NAME=tom #直接定义环境变量
[root@centos7 ~]# chmod +x echo.sh #修改文件权限为可执行

```
|变量名称|含义|
| :-: | :-: |
| BASHPID| 当前bash 进程的进程号 |
| GROUPS | 当前用户所属的组的组ID号 |
| HOSTNAME | 当前主机的主机名称 |
| PWD | 当前工作目录 |
| OLDPWD | 前一个工作目录 |
| RANDOM | 0~32767之间的一个随机数 |
| UID | 当前用户的ID号码 |
| HISTSIZE | 命令历史的记录条数 |
| HOME | 当前用户的家目录 |
| PATH | 命令搜索路径 |
| PS1 | 主命令提示符 |




