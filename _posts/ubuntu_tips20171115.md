---
title: ubuntu 16.04 装机攻略
date: 2017-11-15 19:13:35
tags:
---

下面分别介绍一下所用的软件：
## 桌面美化类

至于桌面美化只是简单的做了一些处理，可以参考博客ubuntu16.04主题美化和软件推荐,这里不再赘述，毕竟Ubuntu原始桌面实在是不好看。至于Docker发现装了作用不大而且使屏幕复杂多了就不再用了。

### BingWallpaper

 bing壁纸使用每天可以更换壁纸而且壁纸质量比较好，不同每天看着一样的壁纸。

### Redshift

屏幕防蓝光工具，毕竟要天天看着电脑要保护眼睛。以前在笔记本上使用f.lux，但是在台式机上安装f.lux发现并不能调节屏幕蓝光，找到Redshift，发现还蛮好用的。

## 开发工具类

### eclipse

eclipse作为Java集成开发环境中最好的的存在，当然要接着使用。

### Atom

Github出品的文本编辑器，本人感觉比sublime好用多了，sublime在Ubuntu中最大的缺陷在于对中文支持实在不咋地，遂放弃sublime。Vim就不用说了，超级牛只是用不习惯。

### Pycharm

尝试Pycharm写Python也比较爽，但是本人习惯使用Eclipse的界面风格，所以一般写Python都是使用Eclipse+PyDev的，偶尔也用一下Pycharm。

### Code Blocks

虽然Linux Shell已经提供了Vim+Gcc环境开发C，但是还是习惯集成的开发环境，就装了一个Code Blocks，反正C现在也一般不常用了，装着只是以备不时之需。

### TexStudio

平常写论文少不了LeTex排版工具，本人一直觉得TexStudio+Texlive比Ctex好用多了，就一直用到现在。

### MySQL

至于数据库一直用开源的MySQL+MySQL Workbench，虽然现在已经被Oracle收购了，但是现在还能用。微软的SQLServer实在是太大了，本科学数据库的时候一直用SQLServer。

### Gephi

由于研究生方向是社团检测，Gephi作为复杂网络可视化中比较好用的工具就少不了它罗 。

### Typora

以前在Windows上一直用小书匠，Ubuntu中也能使用小书匠但是感觉真的没有Typora好用。强烈推荐这一款Markdown编辑器。

## 系统工具类

### Google Chrome

Google浏览器一直在用，我觉得这是最好用的浏览器，没有之一。。。。插件尤为强悍

### Firefox

在Google浏览器不能使用的时候偶尔用用Firefox

### ShadowSocks-Qt5

Ubuntu中常用的翻墙软件，当然首先是要有一个账号，并且还要在浏览器中设置代理。

## 新立得软件包管理器

平常用来删除一下不用的软件包。主要是Ubuntu的文件系统有点不熟悉，有时候命令行安装的包不知道安到哪去了。

### Unity Tweak Tool 系统桌面管理工具

添加包命令：sudo add-apt-repository ppa:freyja-dev/unity-tweak-tool-daily
更新包管理器：sudo apt-get update
安装命令：sudo apt-get install unity-tweak-tool
安装完成，但失败无法启用
参考：http://www.linuxidc.com/Linux/2013-02/79830.htm
貌似后来通过软件中心安装成功，可以使用了

### 安装替换系统默认字体
安装文泉字体，通过tweak替换
参考：http://www.linuxdiyf.com/linux/11653.html

### 安装Adobe flash
安装命令：sudo apt-get install flashplugin-installer
### 安装压缩软件RAR
安装命令：sudo apt-get install rar
提高电池的寿命并且减少过热
笔记本过热是一个普遍的问题，它不仅仅存在于ubuntu中，也存在与其他的操作系统中，过热会影响电池的寿命，从 ubuntu12.10开始，tupiter就是解决过热的最好的工具。可惜的是这个项目已经停止开发了，你可以使用TLP或者CPUFREQ来代替 jupiter,安装TLP通过使用下面的命令：
sudo add-apt-repository ppa:linrunner/tlp
sudo apt-get update
sudo apt-get install tlp tlp-rdw
sudo tlp start
使用TPL是不需要进行配置的。
参考：http://www.zhihu.com/question/20509148
安装云盘Dropbox（需VPN）
sudo apt-get install nautilus-dropbox
sudo apt-get install libappindicator1
下载的是一个下载器，完成后可能需要VPN来下载Dropbox主程序。
启动器albert
sudo add-apt-repository ppa:nilarimogard/webupd8
sudo apt-get update
sudo apt-get install albert
安装完成后，在启动器里输入albert打开并设置热键（我这里设置的热键是`+Ctrl）
参考：
http://www.zhihu.com/question/20509148
http://www.webupd8.org/2015/01/albert-fast-lightweight-quick-launcher.html
### 笔记本Zim Desktop Wiki
摘抄知乎：Zim的所有笔记都以文本格式存储，以文件夹方式管理；同时支持HTML、LaTeX、Markdown、rst等多种格式，可在模板中选择(虽然我没用过)；支持全文搜索、标签检索。默认是一种类似Markdown的wiki格式，支持列表、任务列表，可以插入图片、附件、LaTeX公式(需先安装latex相关软件)，插入的图片可单独指定一个附件目录，可输出为HTML、LaTeX、Markdown、rst。
安装命令：sudo apt-get install zim
参考：www.zhihu.com/question/20509148
### 思维导图 Xmind
下载安装包：http://www.xmind.net/download/linux/
安装：sudo dpkg -i xmind-7-update1-linux_amd64.deb
安装失败，缺少依赖包，安装依赖包：sudo apt-get install -f
重新安装：sudo dpkg -i xmind-7-update1-linux_amd64.deb
### 自动调节屏幕色温Redshift
根据日出日落时间(设定经纬度)自动调节电脑屏幕的亮度、色彩(色温)，保护眼睛
没用之前我一直觉得这种东西有必要吗，一直在后台耗费系统资源？用过了之后，我才发现真的很有必要呀！开启软件时，眼睛是一种很柔和的感觉，关闭则是对比很明显的刺眼蓝光。这才发现我的搓本屏幕颜色严重偏蓝，而之前我笔记本的亮度也因为刺眼的蓝光调的过低了。
安装命令：sudo apt-get install redshift
启动命令：redshift
可以加到系统启动里面：vim /etc/rc.local
将启动命令添加进去，保存并推出
参考：http://www.zhihu.com/question/20509148
最近，也找到一个win下的屏幕色温调节工具：flux
### Linux下的完美帅气终端Guake
安装命令：sudo apt-get install guake
     常用快捷键
     调用：键盘F12
     新建终端选项卡：Ctrl+Shift+T
     关闭当前选项卡：Ctrl+Shift+W
     切换到当前选项卡右边：Ctrl+Page Down
     切换到当前选项卡左边：Ctrl+Page Up


### 浮动菜单
下方的浮动图标工具栏：docky
安装方法：sudo apt-get install docky

### 便签Xpad
不是和docky集成的便签
安装方法：sudo apt-get install xpad
参考：http://article.yeeyan.org/view/140496/124205/
### WPS
首先去下载：http://linux.wps.cn/
之后安装：sudo dpkg -i wps-office_10.1.0.5444~a20_amd64.deb


### Pinta

简单好用的图片编辑器，虽然Gimp非常强悍，但是操作觉得比较复杂。

### Catfish

文件搜索，有时文件太多而且不知道放在哪，这个工具可以帮你快速找到，系统自带的文件搜索也好用。

### sougou input 输入法

搜狗输入法，比系统自带的输入法好用多了。

以后发现了什么好用的软件再更新博客。
之前安装的是搜狗拼音，这次安装16.04选择的是汉语各种中文，已经把拼音集成了。所以这次没有安装输入法。不过麻烦的就是去个我的文件夹，原本是cd Downloads的命令要改成cd 下载．
后来发现还是搜狗好用，比方说你要当前的时间，直接输入sj就会出现，但是其他的输入法比方说Ubuntu自带的就没有这个功能，好吧，装搜狗．
安装包下载地址：http://pinyin.sogou.com/linux/
执行安装命令：sudo dpkg -i sogoupinyin_2.0.0.0072_amd64.deb
缺少依赖包，安装失败，安装依赖包：sudo apt-get install -f
依赖包安装完成，重新安装：sudo dpkg -i sogoupinyin_2.0.0.0072_amd64.deb
安装成功
在启动里直接搜索Fcitx Configuration，打开后在Input Method里删除除了Sougou拼音以外的其它中文输入法（可能略有延迟，Sougou拼音没出来），确保除了Sougou拼音外，只有一个Keyboard – English(US)，若没有这一项就在下方点击加号＂＋＂添加这一项．

## 开发工具类

### R+RStudio 
R语言作为统计学习中重要的编程语言。最近装着用着试试。。。可以也不太用的到。
系统工具

### Remmina远程桌面 
最近老师给了一台服务器用用，感觉还是蛮爽的，服务器是Windows Server系统，因此发现了这个比较好用的Linux->Windows远程工具。

### VirtualBox虚拟机 
最近需要做实验需要用到Windows和Kali，于是找了一个虚拟机软件。Linux下VirtualBox和VmWare都可以使用，但是本人觉得轻量级的VirtualBox还是好用一点。但是被其中的界面全屏折腾了一下，后来解决了。


