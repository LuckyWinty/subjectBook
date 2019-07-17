事件循环机制相关概念

* 执行上下文\(Execution context\)

* 函数调用栈\(call stack\)

* 队列数据结构\(queue\)

* Promise

##### JavaScript代码的执行

JavaScript代码的执行过程中，除了依靠函数调用栈来搞定函数的执行顺序外，还依靠任务队列\(task queue\)来搞定另外一些代码的执行。

![](/assets/import.png)

JavaScrip中队列数据结构

* 一个线程中，事件循环是唯一的，但是任务队列可以拥有多个。
* 任务队列又分为macro-task（宏任务）与micro-task（微任务），在最新标准中，它们被分别称为task与jobs。
* macro-task大概包括：script\(整体代码\), setTimeout, setInterval, setImmediate, I/O, UI rendering。
* micro-task大概包括: process.nextTick, Promise, async/await\(实际就是promise\),Object.observe\(已废弃\), MutationObserver\(html5新特性\)
* setTimeout/Promise等我们称之为任务源。而进入任务队列的是他们指定的具体执行任务
* 来自不同任务源的任务会进入到不同的任务队列。其中setTimeout与setInterval是同源的。
* 事件循环的顺序，决定了JavaScript代码的执行顺序。它从script\(整体代码\)开始第一次循环。之后全局上下文进入函数调用栈。直到调用栈清空\(只剩全局\)，然后执行所有的micro-task。当所有可执行的micro-task执行完毕之后。循环再次从macro-task开始，找到其中一个任务队列执行完毕，然后再执行所有的micro-task，这样一直循环下去。

* 其中每一个任务的执行，无论是macro-task还是micro-task，都是借助函数调用栈来完成。

参考\(详见\)：[https://www.jianshu.com/p/12b9f73c5a4f](https://www.jianshu.com/p/12b9f73c5a4f)

队列执行规则

1、整体代码执行，遇到宏任务则将宏任务放到队列中，遇到微任务则将微任务放到队列中。

2、整体代码执行完毕后，第一个宏任务script执行完毕之后，就开始执行所有的可执行的微任务。

含有async/await的情况

```
console.log('script start')

async function async1() {
  await async2()
  console.log('async1 end')
}
async function async2() {
  console.log('async2 end')
}
async1()

setTimeout(function() {
  console.log('setTimeout')
}, 0)

new Promise(resolve => {
  console.log('Promise')
  resolve()
})
  .then(function() {
    console.log('promise1')
  })
  .then(function() {
    console.log('promise2')
  })

console.log('script end')
// script start => async2 end => Promise => script end => promise1 => promise2 => async1 end => setTimeout
```

思路基本一致，需要注意：

* 可以简单理解为，await后面的函数执行完毕时，await才产生一个微任务\(promise.then\(xxx\)\)

* 如果await 后面直接跟的为一个变量，比如：await 1；这种情况的话相当于直接把await后面的代码注册为一个微任务，可以简单理解为promise.then\(await 下面的代码\)。然后跳出async函数，执行其他代码，当遇到promise函数的时候，会注册promise.then\(\)函数到微任务队列，注意此时微任务队列里面已经存在await后面的微任务。所以这种情况会先执行await后面的代码（async end），再执行async函数后面注册的微任务代码\(promise1,promise2\)。 
* 如果await后面跟的是一个异步函数的调用，就是作者举得那个例子的形式，此时执行完awit并不先把await后面的代码注册到微任务队列中去，而是执行完await之后，直接跳出async函数，执行其他代码。然后遇到promise的时候，把promise.then注册为微任务。其他代码执行完毕后，需要回到async函数去执行剩下的代码，然后把await后面的代码注册到微任务队列当中，注意此时微任务队列中是有一个之前注册的微任务的。所以这种情况会先执行async函数之外的微任务\(promise1,promise2\)，然后才执行async内注册的微任务\(async end\)



## Node 中的 Event Loop

Node 中的 Event Loop 和浏览器中的是完全不相同的东西。

Node 的 Event Loop 分为 6 个阶段，它们会按照**顺序**反复运行。每当进入某一个阶段的时候，都会从对应的回调队列中取出函数去执行。当队列为空或者执行的回调函数数量到达系统设定的阈值，就会进入下一阶段。

![](/assets/nodeEventLoop.png)

### timer

timers 阶段会执行`setTimeout`和`setInterval`回调，并且是由 poll 阶段控制的。

同样，在 Node 中定时器指定的时间也不是准确时间，只能是**尽快**执行。

### I/O

I/O 阶段会处理一些上一轮循环中的**少数未执行**的 I/O 回调

### idle, prepare

idle, prepare 阶段内部实现，这里就忽略不讲了。

### poll

poll 是一个至关重要的阶段，这一阶段中，系统会做两件事情

1. 回到 timer 阶段执行回调
2. 执行 I/O 回调

并且在进入该阶段时如果没有设定了 timer 的话，会发生以下两件事情

* 如果 poll 队列不为空，会遍历回调队列并同步执行，直到队列为空或者达到系统限制
* 如果 poll 队列为空时，会有两件事发生
  * 如果有
    `setImmediate`
    回调需要执行，poll 阶段会停止并且进入到 check 阶段执行回调
  * 如果没有
    `setImmediate`
    回调需要执行，会等待回调被加入到队列中并立即执行回调，这里同样会有个超时时间设置防止一直等待下去

当然设定了 timer 的话且 poll 队列为空，则会判断是否有 timer 超时，如果有的话会回到 timer 阶段执行回调。

### check

check 阶段执行`setImmediate`

### close callbacks

close callbacks 阶段执行 close 事件

