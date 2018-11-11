终于完全理解了柯里化的一些思想...

**柯里化（英语：Currying）**，又称为部分求值，是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回一个新的函数的技术，新函数接受余下参数并返回运算结果。

柯里化的特点：

* 接收单一参数，将更多的参数通过回调函数来搞定？
* 返回一个新函数，用于处理所有的想要传入的参数；
* 需要利用call/apply与arguments对象收集参数；
* 返回的这个函数正是用来处理收集起来的参数。

基于柯里化，解决js中的常见题目：

```
实现一个add方法，使计算结果能够满足如下预期：
add(1)(2)(3) = 6
add(1, 2, 3)(4) = 10
add(1)(2)(3)(4)(5) = 15
```

了解一些扩展知识：

1、toString\(\) 和 valueOf\(\) 是对象的两个方法，toString\( \)就是将其他东西用字符串表示，比较特殊的地方就是，表示对象的时候，变成"\[object Object\]",表示数组的时候，就变成数组内容以逗号连接的字符串，相当于Array.join\(','\)。 而valueOf\( \)就返回它自身。

2、一般用操作符单独对对象进行转换的时候，如果对象存在valueOf或toString改写的话，就先调用改写的方法，valueOf更高级，如果没有被改写，则直接调用对象原型的valueOf方法。如果是alert弹窗的话，直接调用toString方法。

3、当使用console.log，或者进行运算时，隐式转换就会发生。

因此，解题的关键就是，每次调用都返回一个函数，这个函数负责收集参数每次调用传进来的参数。然后在隐式调用的时候，求和输出。代码如下：

```
function add(){
  var args = [].slice.call(arguments);

  var _add = function(){
    var list = [].slice.call(arguments);
    args = args.concat(list);
    return _add;
  }
  _add.toString = function(){
    return args.reduce(function(pre,next){
      return pre+next;
    })
  }

  return _add;
}
```

简洁式：

```
function mul(x) {
	const result = (y) => mul(x + y); 
	result.valueOf = () => x;
	return result;
}
```



