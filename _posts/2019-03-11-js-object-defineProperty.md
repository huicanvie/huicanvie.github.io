---
title: 使用Object.defineProperty的几个注意点
anthor: huicanvie
layout: post
---
```text
我们都知道这个Object.defineProperty用于定义一个对象上的新属性或者修改对象上的已有属性。Vue.js的响应式原理核心也是使用此方法。
当然Vue.js使用Es6的proxy来做数据监控。因此有必要去深入探讨下此方法，在使用此方法的过程，需要注意以下几点。

```
> 注意数据描述符和存取描述符的区别

- 一个描述符只能取其一，不能两者具有。就是说，在一个对象上定义个属性，要么是数据描述符，要么是存取描述符，不能对属性同时定义2种描述符。
- 它们共有的属性有：  

  1.configurable（决定该属性的描述符（注意：是描述符改变，不是值改变）能否被改变，该属性能否被删除，默认false）

  2.enumerable （可枚举性，默认false，定义该属性是否可以在 for...in 循环和 Object.keys() 中被枚举）
- 数据描述符特有的属性： 

  1.value （数值，对象，函数等，默认undefined）  

  2.writable （是否可写，就是是否能被改变，默认false）
- 存取描述符特有的属性：

  1.get （getter 函数，当访问该属性的时候，会被调用，返回值就被当做属性的值，这里需要反复理解。
        比如：有个对象A，A = {pro: 'a'},执行到console.log(A.pro)的时候，会去执行getter函数）

  2.set  (setter 函数，当被赋值的时候，会调用此函数，接收一个新值作为参数，比如 A.pro = 'b')

```javascript
var obj = {}
var descriptor  = Object.create(null) // 为了防止继承来的属性干扰
descriptor.value = 'static'
// "static"
descriptor.value = 'static2'
// "static2"  此处可以修改value属性的值
Object.defineProperty(obj,'key',descriptor)
//{key: "static2"} 给obj定义了一个新的属性key,值是static2
console.log(obj.key)
//"static2"
obj.key = 's'  //赋一个新值

console.log(obj.key)
//"static2"   还是之前的值，是因为没有定义writable= true,所以是不可以写的

```
> 举个存取描述符栗子

```javascript
var obj = {}
var bValue
Object.defineProperty(obj, "b", {
  get() { return bValue; },
  set(newValue) { bValue = newValue; },
  enumerable : true,
  configurable : true
});
console.log(obj.b)
// undefined
obj.b = 'b'
console.log(obj.b)
// 'b'
```
> 看看属性不可以修改描述符什么

```javascript
var obj = {}
var bValue
Object.defineProperty(obj, "b", {
  value : 'bb',
  writable : true,
  enumerable : true,
  configurable : false  // 注意此处设置，
});

Object.defineProperty(obj, "b", {
  get() { return bValue; },
  set(newValue) { bValue = newValue; },
  enumerable : true,
  configurable : true
});
console.log(obj.b)

// Uncaught TypeError: Cannot redefine property: b   
// 报错啦，configurable = true就不会报错了
```
> 再看一个反栗

```javascript
var obj = {name: 'zjh'}
Object.defineProperty(obj,'name',{
  enumerable : true,
  configurable : true,
  get () { 
    return obj.name
  }, 
  set (newValue) { 
    obj.name = newValue
  }
})

//Uncaught RangeError: Maximum call stack size exceeded
//因为在get方法里面，又重复调用了.运算，无限循环调用get方法，导致调用栈溢出了

```
> 正确写法

```javascript
var obj = {name: 'zjh'}
function define () { 
  var name = 'inner'
  return function() { 
      Object.defineProperty(obj,'name',{
      enumerable : true,
      configurable : true,
      get () { 
        return name
      }, 
      set (newValue) { 
        name = newValue
      }
    })
  }()
}

// 我们采用闭包，将变量name保存起来，
// name作为中间变量，调用get，set，都只和name相关
```
```text
虽然只是一个方法，但是里面有许多内容，需要我们仔细查看api，以免会出现意想不到的情况

```