---
title: Promise的实现
url: 57.html
id: 57
categories:
  - 笔记
tags:
---

> 一点预热 console.log的使用技巧

    //console.log在函数中直接使用, 就会默认打印出函数的所有返回值
    let p = new Promise((resolve, reject) => {
        resolve('hello')
    })
    p.then((res) => {
        console.log(res)
    })
    // equals
    p.then(console.log)
    //还是可以节约很多的代码的😂😂
    

然而 今天我才发现这根本不是啥console.log的特性..