---
title: cas
date: 2020-05-03 10:32:39
tags:
---

# 搭建cas 

## 克隆项目模板
```shell
git clone https://github.com/apereo/cas-overlay-template

git checkout   5.3

```
### 新建目录
src\main\resources

命令行打开目录cas-server/src/main/resources/etc/cas，执行以下命令生成对应的keystore

keytool -genkey -keyalg RSA -alias thekeystore -keystore thekeystore -storepass 123456 -validity 360 -keysize 2048

回车
秘钥口令：123456
组织 国家  姓名   zhumin
thekeystore秘钥口令 ：123456


接着我们要把这个keystore导成证书给客户端用：
keytool -export -alias thekeystore -file thekeystore.crt  -keystore thekeystore

现在我们要把这个导出来的证书导进去JVM里面
keytool -import -alias thekeystore -storepass changeit -file thekeystore.crt -keystore "C:\Program Files\Java\jdk1.8.0_221\jre\lib\security\cacerts"

keytool -import -alias thekeystore -storepass changeit -file thekeystore.crt -keystore "C:\Program Files\Java\jre1.8.0_221\lib\security\cacerts"




参考：https://blog.csdn.net/makyan/article/details/88907349