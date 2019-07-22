---
title: ubuntu 下react-native开发环境
date: 2019-07-20 12:19:40
tags:  ubuntu react-native
---

### 官网
官方: https://facebook.github.io/react-native/
中文网: https://reactnative.cn/
react natvie UI 库: https://nativebase.io/

### 安装React Native CLI
```
npm install -g react-native-cli
```

#### 问题
1.[npm WARN checkPermissions Missing write access to /usr/local/lib/node_modules](https://www.jianshu.com/p/976810138d35)
解决方法：修改npm包所安装目录的权限：sudo chown -R $USER /usr/local   然后输入密码就可以了

### 安装Jdk 1.8
[OpenJDK ](http://openjdk.java.net/)
### andriod studio 下载地址
https://developer.android.google.cn/studio/
https://www.androiddevtools.cn/
Android SDK在线更新镜像服务器
1、南阳理工学院镜像服务器地址:
mirror.nyist.edu.cn 端口：80
2.中国科学院开源协会镜像站地址:
◦IPV4/IPV6: mirrors.opencas.cn 端口：80
◦IPV4/IPV6: mirrors.opencas.org 端口：80
◦IPV4/IPV6: mirrors.opencas.ac.cn 端口：80
3.上海GDG镜像服务器地址:
sdk.gdgshanghai.com 端口：8000
4.北京化工大学镜像服务器地址:
◦IPv4: ubuntu.buct.edu.cn/ 端口：80
◦IPv4: ubuntu.buct.cn/ 端口：80
◦IPv6: ubuntu.buct6.edu.cn/ 端口：80
5.大连东软信息学院镜像服务器地址:
mirrors.neusoft.edu.cn 端口：80
6.腾讯Bugly 镜像:
android-mirror.bugly.qq.com 端口：8080
腾讯镜像使用方法:
http://android-mirror.bugly.qq.com:8080/include/usage.html

### gradle简单使用

[gradle超详细解析](https://www.jianshu.com/p/822e44a5ea97)

### 安装Watchman
```
git clone https://github.com/facebook/watchman.git
cd watchman
git checkout v4.9.0  # the latest stable release
./autogen.sh
./configure
make
sudo make install
```

### 准备android 设备
#### 使用虚拟设备(Using a virtual device)
用android studio 打开 ./AwesomeProject/android
#### [android api](https://www.android-doc.com/reference/packages.html)

### 启动React Native 项目

```
cd AwesomeProject
react-native run-android
```
使用 react-native 命令行是一种方式,你也可以直接用android studio启动项目

#### 问题
1. [KVM is required to run this AVD /dev/kvm permission denied Ubuntu Android Studio](https://blog.csdn.net/weixin_43760383/article/details/84954126)
```
sudo chown $User -R /dev/kvm
```
2. [/dev/kvm is not found](https://askubuntu.com/questions/564910/kvm-is-not-installed-on-this-machine-dev-kvm-is-missing)
Either your CPU does not support virtualization, or it is disabled in the bios. Go into your bios and see if you can find a setting to enable it.
F1 进入bios设置virtualization enabled


### 升级node
利用n来管理版本
```
sudo npm install -g n
```
n来下载node版本
```
sudo n lts 长期支持
sudo n stable 稳定版
sudo n latest 最新版
sudo n 8.4.0 直接指定版本下载
```
切换版本
```
sudo n
直接键盘上下移动选择你要的版本，回车确认
```
或者直接指定版本
```
sudo n 8.10.0
```
查看node版本
```
node -v 注意本终端下查看有可能没有变化，选择新建一个终端查看
```
升级npm
```
sudo npm i -g npm
```
### 问题

[sudo npm command not found 问题解决](https://blog.csdn.net/StillCity/article/details/84106167)
这种情况通常是使用 npm 命令可以正常使用，但使用sudo npm 命令便会报 command not found

这是什么原因呢？
输入which npm可以得到/usr/local/bin/npm，
这个是普通用户的bin目录
而sudo执行的是/usr/bin目录，这是root用户的目录
所以使用sudo命令是识别不到这个命令的，我们可以使用以下方法来处理这个问题
```
sudo ln -s /usr/local/bin/node /usr/bin/node
sudo ln -s /usr/local/lib/node /usr/lib/node
sudo ln -s /usr/local/bin/npm /usr/bin/npm
sudo ln -s /usr/local/bin/node-waf /usr/bin/node-waf
```
如果你的其他程序也是这样的问题,只要将xxx替换成你的可执行程序就可以了
```
sudo ln -s /usr/local/bin/xxx /usr/bin/xxx
```
ln命令用来为文件创件连接，连接类型分为硬连接和符号连接两种，默认的连接类型是硬连接。如果要创建符号连接必须使用"-s"选项。

[安装Android Studio出现SDK tools directory is missing问题](https://xiezuan.github.io/2019/03/09/%E5%AE%89%E8%A3%85Android-Studio%E5%87%BA%E7%8E%B0SDK-tools-directory-is-missing%E9%97%AE%E9%A2%98/)