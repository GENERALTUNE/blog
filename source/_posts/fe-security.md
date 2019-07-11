---
title: web安全总结
date: 2019-07-09 20:33:33
tags: 安全 web
---

web安全总结 xss csrf

### xss
#### 一： XSS介绍
  XSS攻击全称跨站脚本攻击，是为不和层叠样式表(Cascading Style Sheets, CSS)的缩写混淆，故将跨站脚本攻击缩写为XSS，XSS是一种在web应用中的计算机安全漏洞，它允许恶意web用户将代码植入到提供给其它用户使用的页面中。
  XSS分类
XSS根据效果的不同，可以分为存储型XSS、反射型XSS，DOM型XSS。

存储型
    攻击代码在服务器端(数据库)，输出在http响应中。 
    比较常见的场景是，黑客写下一篇包含有恶意JavaScript代码的博客文章，文章发表后，所有访问该博客的用户，都会在他们的浏览器中执行这段恶意js代码
反射型
    攻击代码在URL里，输出在http响应中。黑客往往需要诱使用户“点击”一个恶意链接，才能攻击成功。

DOM型
    从效果上来说，也属于反射型XSS。 
    攻击代码在URL里，输出在DOM节点中。

#### 二：防范

[如何防止XSS攻击](https://www.freebuf.com/articles/web/185654.html)




DVWA(Damn Vulnerable Web Application)是一个用PHP编写的用来进行安全脆弱性鉴定的PHP/MySQL Web应用，旨在为安全专业人员测试自己的专业技能和工具提供合法的环境，帮助web开发者更好的理解web应用安全防范的过程。

[DVWA](https://blog.csdn.net/extremebingo/article/details/81176394)
	DVWA共有十个模块，分别是
	Brute Force(暴力破解)
	Command Injection(命令行注入)
	CSRF(跨站请求伪造)
	File Inclusion(文件包含)
	File Upload(文件上传)
	Insecure CAPTCHA(不安全的验证码)
	SQL Injection(SQL注入)
	SQL Injection(Blind)(SQL盲注)
	XSS(Reflected)(反射型跨站脚本)
	XSS(Stored)(存储型跨站脚本)


### csrf
#### 一、CSRF介绍

CSRF（Cross-site request forgery）

跨站请求伪造，也被称为“OneClick Attack”或者Session Riding，通常缩写为CSRF或者XSRF，是一种对网站的恶意利用。

CSRF攻击与防御，web安全的第一防线。

攻击流程：
    用户访问恶意网站B，恶意网站B返回给用户的HTTP信息中要求用户访问网站A，而由于用户和网站A之间可能已经有信任关系导致这个请求就像用户真实发送的一样会被执行。

#### 二、CSRF攻击的危害

攻击者盗用了你的身份，以你的名义发送恶意请求，对服务器来说这个请求是完全合法的，但是却完成了攻击者所期望的一个操作，比如以你的名义发送邮件、发消息，盗取你的账号，添加系统管理员，甚至于购买商品、虚拟货币转账等。

如果CSRF发送的垃圾信息还带有蠕虫链接的话，那些接收到这些有害信息的好友万一打开私信中的连接就也成为了有害信息的散播着，这样数以万计的用户被窃取了资料种植了木马。整个网站的应用就可能在瞬间奔溃，用户投诉，用户流失，公司声誉一落千丈甚至面临倒闭。曾经在MSN上，一个美国的19岁的小伙子Samy利用css的background漏洞几小时内让100多万用户成功的感染了他的蠕虫，虽然这个蠕虫并没有破坏整个应用，只是在每一个用户的签名后面都增加了一句“Samy 是我的偶像”，但是一旦这些漏洞被恶意用户利用，后果将不堪设想，同样的事情也曾经发生在新浪微博上面。

#### 三、CSRF漏洞防御

目前防御 CSRF 攻击主要有三种策略：验证 HTTP Referer 字段；在请求地址中添加 token 并验证（表单令牌验证）；在 HTTP 头中自定义属性并验证。

1、尽量使用POST，限制GET

GET接口太容易被拿来做CSRF攻击，看第一个示例就知道，只要构造一个img标签，而img标签又是不能过滤的数据。

接口最好限制为POST使用，GET则无效，降低攻击风险。

当然POST并不是万无一失，攻击者只要构造一个form就可以，但需要在第三方页面做，这样就增加暴露的可能性。

2、加验证码

验证码，强制用户必须与应用进行交互，才能完成最终请求。在通常情况下，验证码能很好遏制CSRF攻击。

但是出于用户体验考虑，网站不能给所有的操作都加上验证码。

因此验证码只能作为一种辅助手段，不能作为主要解决方案。

3、Referer Check

Referer Check在Web最常见的应用就是“防止图片盗链”。

同理，Referer Check也可以被用于检查请求是否来自合法的“源”（Referer值是否是指定页面，或者网站的域），如果都不是，那么就极可能是CSRF攻击。

但是因为服务器并不是什么时候都能取到Referer，所以也无法作为CSRF防御的主要手段。

但是用Referer Check来监控CSRF攻击的发生，倒是一种可行的方法。

4 、Anti CSRF Token

现在业界对CSRF的防御，一致的做法是使用一个Token（Anti CSRF Token）。

例子：

用户访问某个表单页面。

服务端生成一个Token，放在用户的Session中，或者浏览器的Cookie中。

在页面表单附带上Token参数。

用户提交请求后， 服务端验证表单中的Token是否与用户Session（或Cookies）中的Token一致，一致为合法请求，不是则非法请求。

这个Token的值必须是随机的，不可预测的。由于Token的存在，攻击者无法再构造一个带有合法Token的请求实施CSRF攻击。另外使用Token时应注意Token的保密性，尽量把敏感操作由GET改为POST，以form或AJAX形式提交，避免Token泄露。

注意：

CSRF的Token仅仅用于对抗CSRF攻击。当网站同时存在XSS漏洞时候，那这个方案也是空谈。

所以XSS带来的问题，应该使用XSS的防御方案予以解决。

特别是在一些论坛之类支持用户自己发表内容的网站，黑客可以在上面发布自己个人网站的地址。由于系统也会在这个地址后面加上 token，黑客可以在自己的网站上得到这个 token，并马上就可以发动 CSRF 攻击。为了避免这一点，系统可以在添加 token 的时候增加一个判断，如果这个链接是链到自己本站的，就在后面添加 token，如果是通向外网则不加。不过，即使这个 csrftoken 不以参数的形式附加在请求之中，黑客的网站也同样可以通过 Referer 来得到这个 token 值以发动 CSRF 攻击。这也是一些用户喜欢手动关闭浏览器 Referer 功能的原因。

在 HTTP 头中自定义属性并验证

这种方法也是使用 token 并进行验证，和上一种方法不同的是，这里并不是把 token 以参数的形式置于 HTTP 请求之中，而是把它放到 HTTP 头中自定义的属性里。通过 xmlHttpRequest 这个类，可以一次性给所有该类请求加上 csrftoken 这个 HTTP 头属性，并把 token 值放入其中。这样解决了上种方法在请求中加入 token 的不便，同时，通过 xmlHttpRequest 请求的地址不会被记录到浏览器的地址栏，也不用担心 token 会透过 Referer 泄露到其他网站中去。

然而这种方法的局限性非常大。xmlHttpRequest 请求通常用于 Ajax 方法中对于页面局部的异步刷新，并非所有的请求都适合用这个类来发起，而且通过该类请求得到的页面不能被浏览器所记录下，从而进行前进，后退，刷新，收藏等操作，给用户带来不便。另外，对于没有进行 CSRF 防护的遗留系统来说，要采用这种方法来进行防护，要把所有请求都改为 xmlHttpRequest 请求，这样几乎是要重写整个网站，这代价无疑是不能接受的。



