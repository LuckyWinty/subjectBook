Node程序运行中，此进程占用的所有内存称为**常驻内存**（Resident Set）。

* 常驻内存由以下部分组成：
  1. 代码区（Code Segment）：存放即将执行的代码片段
  2. 栈（Stack）：存放局部变量
  3. 堆（Heap）：存放对象、闭包上下文
  4. 堆外内存：不通过V8分配，也不受V8管理。Buffer对象的数据就存放于此。

除堆外内存，其余部分均由V8管理。

* 栈（Stack）的分配与回收非常直接，当程序离开某作用域后，其栈指针下移（回退），整个作用域的局部变量都会出栈，内存收回。
* 最复杂的部分是堆（Heap）的管理，V8使用**垃圾回收机制**进行堆的内存管理，也是开发中可能造成内存泄漏的部分。

通过`process.memoryUsage()`可以查看此Node进程的内存使用状况。

### 垃圾回收机制

**1、引用计数**

含义是跟踪记录每个值被引用的次数。当声明了一个变量并将一个引用类型值赋给该变量时，则这个值的引用次数就是1。

如果同一个值又被赋给另一个变量，则该值的引用次数加1。相反，如果包含对这个值引用的变量改变了引用对象，则该值引用次数减1。容易引起循环引用。

**2、代回收的策略\(v8\)**

#### v8的待回收策略：

##### 1.1 **新生代的垃圾回收**

新生代中的对象主要通过Scavenge算法进行垃圾回收，这是一种采用复制的方式实现内存回收的算法。Scavenge算法将新生代的总空间一分为二，只使用其中一个，另一个处于闲置，等待垃圾回收时使用。使用中的那块空间称为**From**，闲置的空间称为**To**。

##### 1.2**老年代的垃圾回收**

老年代垃圾回收主要采用**标记清除\(Mark-Sweep\)**和**标记整理\(Mark-Compact\)**。

1.3**增量标记（Incremental Marking）**

早期V8在垃圾回收阶段，采用全停顿（stop the world），也就是垃圾回收时程序运行会被暂停。这在JavaScript还仅被用于浏览器端开发时，并没有什么明显的缺点，前端开发使用的内存少，大多数时候仅触发新生代垃圾回收，速度快，卡顿几乎感觉不到。但是对于Node程序，使用内存更多，在老年代垃圾回收时，全停顿很容易带来明显的程序迟滞，标记阶段很容易就会超过100ms，因此V8引入了**增量标记**，将标记阶段分为若干小步骤，每个步骤控制在5ms内，每运行一段时间标记动作，就让JavaScript程序执行一会儿，如此交替，明显地提高了程序流畅性，一定程度上避免了长时间卡顿。
