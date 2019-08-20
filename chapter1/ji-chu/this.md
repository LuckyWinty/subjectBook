#### this的指向

除去不常用的 with 和 eval 的情况，具体到实际应用中，this 的指向大致可以分为以下 4 种。

 作为对象的方法调用。

 作为普通函数调用。

 构造器调用。

 Function.prototype.call 或 Function.prototype.apply 调用。

当函数作为对象的方法被调用时this 指向该对象。

当普通函数方式被调用时，此时的 this 总是指上下文全局对象。在浏览器的 JavaScript 里，全局对象是 window 对象。

当用 new 运算符调用函数时，该函数总会返回一个对象，通常情况下，构造器里的 this 就指向返回的这个对象。

用 Function.prototype.call 或 Function.prototype.apply 可以动态地改变传入函数的 this

