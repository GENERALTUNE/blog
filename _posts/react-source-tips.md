---
title: react-source-tips
date: 2018-01-19 19:41:55
tags:
---

## 相关文档资源
react IRC https://webchat.freenode.net/?channels=reactjs
react git 地址　https://github.com/facebook/react.git
使用flow进行数据校验　 地址：https://flow.org/en/docs/install/
##  好的源码分析文章
16.x 源码解析：　http://zxc0328.github.io/2017/09/28/react-16-source/
15.x 源码解析：　https://www.cnblogs.com/JhoneLee/p/5886759.html
15.x 源码解析：　https://zhuanlan.zhihu.com/p/28697362
15.x 源码解析：　http://blog.csdn.net/p77ll9l53x/article/details/72675590
15.x react diff算法解析　http://blog.csdn.net/yczz/article/details/49886061
重新实现一个react 的难点与思路　https://www.cnblogs.com/sven36/p/6486860.html

对比es6 extends写法
说了一堆，其实 createClass 主要就做了这么几件事。

生成并返回一个Constructor构造函数，该函数有props，context，refs，updater和state5个属性。
绑定了作用域。
将属性或方法添加到 Constructor.prototype 上。
调用 getDefaultProps 初始化 Constructor.defaultProps 属性。
以上3点通过 class xxx extends React.Component 写法都能办到。


## 源码分析笔记


分析入口：　ReactDOM.render   Component.prototype.setState  这两个函数是react应用开发中经常用到的触发页面渲染函数.


###  ReactDOM.render 

ReactDOM.render --> legacyRenderSubtreeIntoContainer --> legacyCreateRootFromDOMContainer --> legacyCreateRootFromDOMContainer --> new ReactRoot /
--> DOMRenderer.createContainer(container, isAsync, hydrate) --> DOMRenderer.updateContainer
                                                     --> DOMRenderer.unbatchedUpdates  
                                                     --> root.legacy_renderSubtreeIntoContainer
                                                     --> root.render
                                                     --> DOMRenderer.getPublicRootInstance(root._internalRoot)






