#### this的指向

除去不常用的 with 和 eval 的情况，具体到实际应用中，this 的指向大致可以分为以下 4 种。

 作为对象的方法调用。

 作为普通函数调用。

 构造器调用。

 Function.prototype.call 或 Function.prototype.apply 调用。

当函数作为对象的方法被调用时this 指向该对象。

当普通函数方式被调用时，此时的 this 总是指
