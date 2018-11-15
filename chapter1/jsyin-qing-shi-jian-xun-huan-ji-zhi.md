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
* micro-task大概包括: process.nextTick, Promise, Object.observe\(已废弃\), MutationObserver\(html5新特性\)
* setTimeout/Promise等我们称之为任务源。而进入任务队列的是他们指定的具体执行任务
* 来自不同任务源的任务会进入到不同的任务队列。其中setTimeout与setInterval是同源的。
* 事件循环的顺序，决定了JavaScript代码的执行顺序。它从script\(整体代码\)开始第一次循环。之后全局上下文进入函数调用栈。直到调用栈清空\(只剩全局\)，然后执行所有的micro-task。当所有可执行的micro-task执行完毕之后。循环再次从macro-task开始，找到其中一个任务队列执行完毕，然后再执行所有的micro-task，这样一直循环下去。

* 其中每一个任务的执行，无论是macro-task还是micro-task，都是借助函数调用栈来完成。

参考\(详见\)：[https://www.jianshu.com/p/12b9f73c5a4f](https://www.jianshu.com/p/12b9f73c5a4f)

队列执行规则

1、整体代码执行，遇到宏任务则将宏任务放到队列中，遇到微任务则将微任务放到队列中。

2、整体代码执行完毕后，若宏任务栈为空则直接执行微任务。否则执行宏任务栈的任务，执行宏任务过程中，若遇到微任务则将微任务放到微任务栈中，且在该宏任务执行完毕后，执行一个微任务。若没有遇到微任务，则直接执行下一个宏任务。

