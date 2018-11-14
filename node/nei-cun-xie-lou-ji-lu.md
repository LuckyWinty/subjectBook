## 一些概念

**增量式GC**

表示垃圾回收器在扫描内存空间时是否收集（增加）垃圾并在扫描周期结束时清空垃圾。

**非增量式GC**

使用非增量式垃圾收集器时，一收集到垃圾即将其清空。

垃圾回收器只会针对新生代内存区、老生代指针区以及老生代数据区进行垃圾回收。对象首先进入占用空间较少的新生代内存。大部分对象会很快失效，非增量GC直接回收这些少量内存。假如有些对象一段时间内不能被回收，则进去老生代内存区。这个区域则执行不频繁的增量GC，且耗时较长。



**内存泄漏的发生的原因**

1，缓存；

2，队列消费不及时；

3，作用域未释放

 4，全局变量，即直接挂在global上的变量

 5，闭包

 6，事件监听，例如，Node.js 中 Agent 的 keepAlive 为 true 时，可能造成的内存泄漏。当 Agent keepAlive 为 true 的时候，将会复用之前使用过的 socket，如果在 socket 上添加事件监听，忘记清除的话，因为 socket 的复用，将导致事件重复监听从而产生内存泄漏。

**检测内存泄漏方法**

针对内存泄漏可以采用植入memwatch，或者定时上报process.memoryUsage内存使用率到monitor，并设置告警阀值进行监控。

**内存泄漏问题排查方法**

1、打印内存快照，若允许情况下，可以在本地运行node-heapdump，使用定时生成内存快照。并把快照通过chrome Profiles分析泄漏原因。若无法本地调试，在测试服务器上使用v8-profiler输出内存快照比较分析json（需要代码侵入）。

例子：将 heapdump 引入代码中，使用 heapdump.writeSnapshot 就可以打印内存快照了了。为了减少正常变量的干扰，可以在打印内存快照之前会调用主动释放内存的 gc\(\) 函数（启动时加上 --expose-gc 参数即可开启）。

```
const heapdump = require('heapdump');

const save = function () {
  gc();
  heapdump.writeSnapshot('./' + Date.now() + '.heapsnapshot');
}
```



