---
title: js中的this
date: 2019-03-14 18:19:09
tags: JS
---
### 提纲: 
1. this指向什么? 什么时候 this指向 window? 
2. 箭头函数中的this
3. bind,apply和 call, 模拟实现一个bind,call,apply

## this指向什么

> 刚开始了解this的时候, 看到过一句话, 谁调用函数, 函数中的this就执行谁.我也把这句话奉为圭臬.
> 但是,随着深入的了解, 有些时候就更蒙蔽了.到底是谁调用的这个函数呢..

### 直接调用

直接通过函数名调用, 无论在哪个作用域下返回的都是全局对象, 直接声明的函数就是在全局对象上声明的.
note: 浏览器下和node下的全局对象是不同的

### 方法中 this 指向全局对象的情况

```js
var name = 'Bob'
var obj = {
    name: 'Jack',
    getName: function () {
        console.log(this.name)
    }
}
var getName = obj.getName
getName()
```
我一开始觉得这个`getName`会输出的是 obj对象里面的 name,因为不是用obj.getName定义的嘛.
然而, 并不是的, 这都是属于定义的, 这个时候的调用还是window的,所以`log`出来的应该是Bob
还有一种特殊情况就是数组中的函数在数组中调用的情况,如下题目:
```js
var length = 50
var fn = function (){
    console.log(this.length)
}
var arr = [fn,1,2]
arr[0]()
```
一开始以为这块的fn中的this应该会没有特殊指定会指向window,然而并不是的.
在数组中调用函数的话,this相当于是这个数组,所以返回的length是数组的length,所以返回的是3

### 箭头函数中的this

箭头函数中没有作用域,也就没有this,this使用的是父级的this

## bind,apply和 call, 模拟实现一个bind,call,apply

三者对比:
> call 接受参数 [thisArg, arg1, arg2, ...]
> apply 接受参数 [thisArg, [argArr]]
> bind 接受参数 [thisArg, arg1, arg2, ...]
> 可以看出前两者的区别在于参数的提供方式 call是一个一个指定, 而apply 是用的数组
> 最后一个bind 接受的参数于call 相同,但是bind 会返回一个函数, 并不会直接执行函数, 这也是bind 与前两者的区别所在

### call的实现

先看call 到底做了什么.
```js
var a = {name: 'Jack'}
function fn() {console.log(this.name)}
fn.call(a) // Jack
```
那么其实,上面的代码等同于:
```js
var a = {
    name: 'Jack',
    fn: fn // 这块的fn的属性名任意都行
}
a.fn()
```
有了这样的思路之后,我们发现实现call,就是将调用的方法增加到参数对象的属性中,然后运行就行了.为了不被污染,执行完成之后,还可以删除该属性
```js 
Function.prototype.MyCall = function (thisArg) {
    thisArg.fn = thisArg
    thisArg.fn()
    delete thisArg.fn
}
```
这样的第一版的call就完成了,然后我们再继续优化它

首先,对接受的参数是不是函数要进行判断.

其次,除了接受thisArg参数之外,call 还可以接受函数执行时候的参数,

如果函数有返回值的话,还应该将值返回出去,

还需要保证添加的函数的属性名不会覆盖原有属性

以及, 如果传的 thisArg是 null,应该使用window

```js
// 考虑到多种环境下的使用
function getWindow () {
    return this
}
function generateRandom () {
    return Date.now()
}
Function.prototype.MyCall = function (thisArg, ...args) {
    if (typeof this !=== 'function') throw new Error(this+'must be a function')
    if (thisArg === null) thisArg = getWindow()
    var random = generateRandom()
    thisArg[random] = this
    var result = thisArg[random](...args)
    delete thisArg[random]
    return result
}
```
这块使用了...args 来获取传过来的函数的参数,如果不让使用的话,可以使用`eval`来解析参数

### apply的实现
>实现了call之后 apply就是水到渠成的事情了.
```js
Function.prototype.MyApply = function (thisArg, args) {
    this.MyCall(thisArg,...args)
}
```

### bind的实现

bind的实现需要考虑到一些比较特殊的地方,就是在使用new的时候, 感觉这块的实现有点不懂.等会来补充

待续~
