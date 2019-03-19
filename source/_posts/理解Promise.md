---
title: 理解Promise
date: 2019-03-18 09:03:13
tags:
---
> 看到很多对博客的探讨的说法，认为博客应该是对网上没有的知识技能的补充。我不能认同，每个人记录的目的是不一样的，我就是为了记录自己学习的过程，很多文章，现在看来对其他人的作用可能很小，甚至没有什么用。但是这是我记录，加深印象的过程。暂时地步还没有到达补充的看法。

### 从promise的实现说起

既然我们想实现一个promise，至少要明白promise是什么。
一句话解释promise，对异步事件的最终状态的描述。

一个简单的，可以代表实现历程的promise：

```js
const PENDING = 'PENDING'
const FULFILLED = 'FULFILLED'
const REJECTED = 'REJECTED'

const isFunction = fn => typeof fn === 'function'

class MyPromise {
    constructor(handle) {
        if (!isFunction(handle)) {
            throw new Error('must be a function')
        }
        // 初始化状态
        this._status = PENDING
        // 初始化返回值
        this._value = undefined
        // 执行队列
        this._fulfilledQueues = []
        this._rejectedQueues = []

        try {
            handle(this._resolved.bind(this), this._rejected.bind(this))
        } catch (error) {
            this._rejected(error)
        }
    }
    _resolved(val) {
        if (this._status !== PENDING) return
        // 还需要处理如果resolve 接受的还是 promise的情况， 接受就是promise的话， 状态会由接受的promise的状态来决定
        // 上述的解决方法，和then中的设计思路是一样的
        const run = (value) => {
            const fulfilled = (value) => {
                let cb
                while (cb = this._fulfilledQueues.shift()) {
                    cb(value)
                }
            }
            const rejected = (err) => {
                let cb
                while (cb = this._rejectedQueues.shift()) {
                    cb(err)
                }
            }
            if (val instanceof MyPromise) {
                val.then(value => {
                    this._status = FULFILLED
                    this._value = value
                    fulfilled(value)
                }, err => {
                    this._status = REJECTED
                    this._value = err
                    rejected(err)
                })
            } else {
                this._status = FULFILLED
                this._value = value
                fulfilled(value)
            }

        }
        setTimeout(() => run(val), 0)
    }
    _rejected(err) {
        if (this._status !== PENDING) return
        const run = (error) => {
            this._status = REJECTED
            this._value = error
            let cb
            while (cb = this._rejectedQueues.shift()) {
                cb(error)
            }
        }
        setTimeout(() => run(err), 0)
    }
    then(onFulfilled, onRejected) {
        const { _status, _value } = this
        return new MyPromise((onFulfilledNext, onRejectedNext) => {
            const fulfilled = (val) => {
                if (!isFunction(onFulfilled)) { //值穿透 ，如果 then中的值不是函数，会忽略这个值，将上一个的value继续传下去
                    onFulfilledNext(val)
                }
                let res = onFulfilled(val)
                if (res instanceof MyPromise) { // 如果返回的还是一个promise的话，需要等待这个promise的结束，所以把 handle 函数直接then 在其后面
                    res.then(onFulfilledNext, onRejectedNext)
                } else {
                    onFulfilledNext(res) // 一般的情况
                }
            }
            // 失败的执行函数
            const rejected = (err) => {
                if (!isFunction(onRejected)) {
                    onRejectedNext(err)
                }
                console.trace()
                let res = onRejected(err)
                if (res instanceof MyPromise) {
                    res.then(onFulfilledNext, onRejectedNext)
                } else {
                    onFulfilledNext(res) //  在失败队列中，没有错误的执行完了，进行的还是下一个的成功回调
                }
            }
            switch (_status) {
                case PENDING:
                    this._fulfilledQueues.push(fulfilled)
                    this._rejectedQueues.push(rejected)
                    break
                case FULFILLED:
                    fulfilled(_value)
                    break
                case REJECTED:
                    rejected(_value)
                    break
            }
        })
    }
    catch(fn) {
        return this.then(undefined, fn)
    }
    static resolve(value) { // resolve reject 返回的是一个具有成功处理函数（失败处理函数）的promise
        if (value instanceof MyPromise) return value
        return new MyPromise(resolve => resolve(value))
    }
    static rejected(val) {
        return new MyPromise((resolve, rejected) => rejected(val))
    }
    static all(lists) {
        return new MyPromise((resolve, rejected) => {
            let result = []
            for (let p of lists) {
                MyPromise.resolve(p).then(val => {
                    result.push(val)
                    console.log('val: ', val);
                    if (result.length === lists.length) {
                        resolve(result)
                    }
                }, err => {
                    rejected(err)
                })
            }
        })
    }
    static race(lists) {
        return new MyPromise((resolve, rejected) => {
            for (let p in lists) {
                p.then(val => {
                    resolve(val)
                }, err => {
                    rejected(err)
                })
            }
        })
    }
}
```