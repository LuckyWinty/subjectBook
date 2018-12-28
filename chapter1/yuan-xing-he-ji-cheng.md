[https://juejin.im/post/58f94c9bb123db411953691b](https://juejin.im/post/58f94c9bb123db411953691b)

补充：

new操作符

以 new 操作符调用构造函数的时候，函数内部实际上发生以下变化：

1、创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型。

2、属性和方法被加入到 this 引用的对象中。

3、新创建的对象由 this 所引用，并且最后隐式的返回 this.



