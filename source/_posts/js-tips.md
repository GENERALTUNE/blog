---
title: jquery&原生js问题总结
date: 2019-07-11 09:57:12
tags: javascirpt 
---

###  常见类型判断
1. js如何判断数组是Array类型
在说明如何判断一个对象为数组类型前，我们先巩固下js的数据类型，js一共有六大数据类型：number、string、object、Boolean、null、undefined。

```
     var str="string";
     console.log(typeof str); //string
     var num=1;
     console.log(typeof num); //number
     var bn=false;
     console.log(typeof bn); //boolean
     var a;
     console.log(typeof a); //undfined
     var obj = null;
     console.log(typeof obj); //object
     var doc = document;
     console.log(typeof doc);//object
     var arr = [];
     console.log(arr); //object
     var fn = function(){};
     console.log(typeof fn); //function     
````
除了前四个类型外，null、对象、数组返回的都是object类型；对于函数类型返回的则是function，再比如typeof(Date)，typeof(eval)等。接下来进入正题，js判断数组类型的方法。

#### 方法一： 使用instanceof方法
instanceof 用于判断一个变量是否某个对象的实例，左边操作数是一个对象，右边操作数是一个函数对象或者函数构造器。原理是通过判断左操作数的对象的原型链上是否具有右操作数的构造函数的prototype属性。
a instanceof b?alert("true"):alert("false")  //注意b值是你想要判断的那种数据类型，不是一个字符串，比如Array。
举一个例子：
```
var arr=[];
console.log(arr instanceof Array) //返回true
```

#### 方法二： 使用constructor方法
在W3C定义中的定义：constructor 属性返回对创建此对象的数组函数的引用，就是返回对象相对应的构造函数。从定义上来说跟instanceof不太一致，但效果都是一样的。
那么判断各种类型的方法：
```
console.log([].constructor == Array);  //true
console.log({}.constructor == Object);  //true
console.log("string".constructor == String); //true
console.log((123).constructor == Number);  //true
console.log(true.constructor == Boolean);  //true
```
注意：
使用instaceof和construcor,被判断的array必须是在当前页面声明的！比如，一个页面（父页面）有一个框架，框架中引用了一个页面（子页面），在子页面中声明了一个array，并将其赋值给父页面的一个变量，这时判断该变量，Array ==object.constructor;会返回false；
原因：
1、array属于引用型数据，在传递过程中，仅仅是引用地址的传递。
2、每个页面的Array原生对象所引用的地址是不一样的，在子页面声明的array，所对应的构造函数，是子页面的Array对象；父页面来进行判断，使用的Array并不等于子页面的Array。
#### 方法三： 使用Object.prototype.toString.call(arr) === '[object Array]'方法

```
function isArray(o) {
　　return Object.prototype.toString.call(o);
}
var arr=[2,5,6,8];
var obj={name:'zhangsan',age:25};
var fn = function () {}
console.log(isArray(arr)); //[object Array]
console.log(isArray(obj)); //[object Object]
console.log(isArray(fn));  //[object function]
```

#### 方法四：ES5定义了Array.isArray:
```
     Array.isArray([]) //true
```

### es5 继承实现方式(六种)

首先定义一个父类
```
// 定义一个动物类
function Animal (name) {
  // 属性
  this.name = name || 'Animal';
  // 实例方法
  this.sleep = function(){
    console.log(this.name + '正在睡觉！');
  }
}
// 原型方法
Animal.prototype.eat = function(food) {
  console.log(this.name + '正在吃：' + food);
};

```

#### 原型链继承
#### 构造继承
#### 实例继承
#### 拷贝继承
#### 组合继承
#### 寄生组合继承
核心：通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类的构造的时候，就不会初始化两次实例方法/属性，避免的组合继承的缺点
```
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
(function(){
  // 创建一个没有实例方法的类
  var Super = function(){};
  Super.prototype = Animal.prototype;
  //将实例作为子类的原型
  Cat.prototype = new Super();
})();

// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // true
console.log(cat instanceof Cat); //true

Cat.prototype.constructor = Cat; // 需要修复下构造函数
```
或者
```
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
 
Cat.prototype = Object.create(Animal.prototype, {
  constructor: {
    value: Cat,
    enumerable: false,
    writable: true,
    configurable: true
  }
})
 
// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // true
console.log(cat instanceof Cat); //true<br>以上继承实现的核心就是将父类的原型赋值给了子类，并且将构造函数设置为子类，这样既解决了无用的父类属性问题，还能正确的找到子类的构造函数。
```
特点：

堪称完美
缺点：

实现较为复杂

[参考](https://www.cnblogs.com/gwf93/p/10384352.html)


### 防抖 截流
#### 防抖（debounce）
所谓防抖，就是指触发事件后在 n 秒内函数只能执行一次，如果在 n 秒内又触发了事件，则会重新计算函数执行时间。


#### 节流（throttle）
所谓节流，就是指连续触发事件但是在 n 秒中只执行一次函数。节流会稀释函数的执行频率。

### Promise 实现
[参考](https://www.jianshu.com/p/b4f0425b22a1)