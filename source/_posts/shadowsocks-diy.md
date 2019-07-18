---
title: 自己实现一个shadowsock
date: 2019-07-17 16:54:31
tags:  diy  shadowsocks
---

### shadowsocks


### 什么是socks5
或许你没听说过socks5，但你一定听说过ShadowSocks，ShadowSockS内部使用的正是socks5协议。
socks是”SocketS”的缩写，因此socks5也叫sockets5。
RFC地址：
[rfc1928](https://www.ietf.org/rfc/rfc1928.txt)
[rfc1929](https://www.ietf.org/rfc/rfc1929.txt)

socks是一种网络传输协议，主要用于客户端与外网服务器之间通讯的中间传递。根据OSI七层模型来划分，SOCKS属于会话层协议，位于表示层与传输层之间。

当防火墙后的客户端要访问外部的服务器时，就跟socks代理服务器连接。该协议设计之初是为了让有权限的用户可以穿过过防火墙的限制，使得高权限用户可以访问外部资源。经过10余年的时间，大量的网络应用程序都支持socks5代理。

这个协议最初由David Koblas开发，而后由NEC的Ying-Da Lee将其扩展到版本4，最新协议是版本5，与前一版本相比，socks5做了以下增强：
1. 增加对UDP协议的支持；
2. 支持多种用户身份验证方式和通信加密方式；
3. 修改了socks服务器进行域名解析的方法，使其更加优雅；

socks协议的设计初衷是在保证网络隔离的情况下，提高部分人员的网络访问权限，但是国内似乎很少有组织机构这样使用。一般情况下，大家都会使用更新的网络安全技术来达到相同的目的。

但是由于socksCap32和PSD这类软件，人们找到了socks协议新的用途：突破网络通信限制，这和该协议的设计初衷正好相反。

下面是两个典型的运用场景：

美国某网游的服务器仅允许本国的IP进行连接。非美国玩家为了突破这种限制，可以找一个该地区的socks5代理服务器，然后用PSD接管网游客户端，通过socks5代理服务器连接游戏服务器。这样游戏服务器就会认为该玩家的客户端位于本地区，从而允许该玩家进行游戏（在天朝也叫科学**，属于正向代理）。
![图片sock5](socks5-protocol.png)
### 参考
[socks5 协议简介](https://blog.csdn.net/tianxuhong/article/details/82151020)