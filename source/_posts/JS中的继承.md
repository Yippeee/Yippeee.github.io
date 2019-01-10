---
title: JS中的继承
date: 2019-01-10 14:33:09
tags:
---

> 文中方法全部来自<<JavaScript高级程序设计一书>>

先想一个需求, 这里有一个父类方法, 包含了全部的共有属性, 我们想自己利用这个方法来创造继承于其中的子类方法
```js
function SuperClass(id) {
    this.id = id
    this.name = 'fatherFunction'
    this.colors = ['red', 'bule']
}

SueperClass.prototype.say = function () {
    console.log(this.id, this.name, this.colors)
}
```
在上面我们定义了一个父类构造方法, 其中包含属性id,name,colors,还定义了它的一个方法say

### 原型链继承
```js
function Child_chain () {
    this.name = 'Child_Prototype_chain'
}
Child_chain.prototype = new SuperClass('10')
let child_chain1 = new Child_chain()
child_chain1.colors.push('yellow')
child_chain1.say()
let child_chain2 = new Child_chain()
child_chain2.say()
```
原型链继承是直接利用原型链实现的继承方法, 将原型继承父类实例的方法和属性
当子类中调用其本身没有的属性和方法的时候,会在原型链上查找就可以调用父类上的属性和方法了,就实现了继承

### 构造函数继承

```js
function Child_constructor (id) {
    SuperClass.call(this, id) // key
}
let child_constructor = new Child_constructor('20')
let child_constructor2 = new Child_constructor('21')
```
用构造函数实现继承的时候,每一个子类的实例都会调用一次父类的构造函数,保证了数据的独立性
但是这样并不是完整的继承,这样的方法只实现了属性的继承,而对方法没有继承
so,就是组合继承出场的时候了.

### 组合继承

在组合继承中,还是使用构造函数一样的继承方法,但是会在子类的原型上再进行一些操作
```js
Child_constructor.prototype = new SuperClass() // 实现对父类中的方法的继承
Child_constructor.prototype.constructor = Child_constructor // 同时还需要把constructor属性更改回来
```
这样就完成了一个完整的继承,当然还是有不够完美的地方,那就是调用了父类的构造函数两次,这些都是很浪费的

### 寄生组合继承

这个方法的出现原因主要是为了解决父类构造函数被调用了两次的问题,`Super.call()`这个继承属性的构造函数是没有什么办法可以去省略的,但是在原型链中指定子类的原型其实没有必要用父类的构造函数,因为我们其实需要父类的原型的一个副本而已

就衍生了一个创造副本的函数
```js
function object (o) {
    var F = new fucntion()
    F.prototype = o
    return new F()
}
```
>这个函数在ECMAScript5中用Object.create()进行了规范

那么寄生组合继承就可以完成了
```js
function Child_inherit (id) {
    SuperClass.call(this, id)
}

Child_inherit.prototype = object(SuperClass.prototype)
Child_inherit.prototype.constructor  = Child_inherit
```
就OK啦

#### class完成继承

>上面的方法看着就反人类,官方也这么觉得了,自然现在就有官方的解决办法了.类似JAVA,用class来完成继承
```js
class Super {
    constructor (name) {
        this.name = name
    }
    sayName () {
        console.log(this.name)
    }
}

class Child extends Super {
    constructor (name, id) {
        super(name) // 实现继承
        this.id = id
    }
}
```
关于class见[more](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/class)
in 2019-01-10 12:00AM