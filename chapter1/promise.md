## Promise原理简析

Promise 必须为以下三种状态之一：等待态（Pending）、执行态（Fulfilled）和拒绝态（Rejected）。一旦Promise被resolve或reject，不能再迁移至其他任何状态（即状态 immutable）。

基本过程：

1. 初始化 Promise 状态（pending）
2. 初始化 then\(..\) 注册回调处理数组（then 方法可被同一个 promise 调用多次）
3. 立即执行传入的 fn 函数，传入Promise 内部 resolve、reject 函数
4. 按事件机制时机处理

Promise里的关键是要保证，then方法传入的参数onFulfilled 和 onRejected，必须在then方法被调用的那一轮事件循环之后的新执行栈中执行。

简版实现：



[https://segmentfault.com/a/1190000013396601](https://segmentfault.com/a/1190000013396601)

