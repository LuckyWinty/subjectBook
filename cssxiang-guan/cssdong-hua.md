需求中常见的css3动画一般有**补间动画（又叫“关键帧动画”）**和**逐帧动画**两种，下面分别介绍：

1. 补间动画/关键帧动画：  
   常用于实现位移、颜色（透明度）、大小、旋转、倾斜等变化。一般有`Transitions`和`Keyframes animation`两种方法实现补间动画。  
   **Transitions：**用于实现简单的动画，只有起始两帧过渡。多用于页面的交互操作，使交互效果更生动活泼。

   > CSS的`transition`允许CSS的属性值在一定的时间区间内平滑地过渡。  
   > 这种效果可以在鼠标单击、获得焦点、被点击或对元素任何改变中触发，并圆滑地以动画效果改变CSS的属性值。

   **Keyframes animation：**用于实现较为复杂的动画，一般关键帧较多。

   > 设置动画的关键帧规则。  
   > `animation`的`timing-function`设置为`ease`、`linear`或`cubic-bezier`，它会在每个关键帧之间插入补间动画，产生具有连贯性的动画。

2. 逐帧动画：

   > animation的`timing-function`默认值为`ease`，它会在每个关键帧之间插入补间动画，所以动画效果是连贯性的。  
   > 除了`ease`、`linear`、`cubic-bezier`之类的过渡函数都会为其插入补间。  
   > 有些效果不需要补间，只需要关键帧之间的跳跃，这时应该使用`steps`过渡方式。

transition的主要api有

```
    transition-property: height;
    transition-duration: 1s;
    transition-delay: 1s;
    transition-timing-function: ease;
```

transition的优点在于简单易用，但是它有几个很大的局限。

（1）transition需要事件触发，所以没法在网页加载时自动发生。

（2）transition是一次性的，不能重复发生，除非一再触发。

（3）transition只能定义开始状态和结束状态，不能定义中间状态，也就是说只有两个状态。

animation的主要api有

```
div:hover {
  animation-name: rainbow;
  animation-duration: 1s;
  animation-timing-function: linear;
  animation-delay: 1s;
  animation-fill-mode:forwards;
  animation-direction: normal;
  animation-iteration-count: 3;
}
```

keyframes的写法

```
@keyframes rainbow {
  0% { background: #c00 }
  50% { background: orange }
  100% { background: yellowgreen }
}
```

另外一点需要注意的是，浏览器从一个状态向另一个状态过渡，是平滑过渡。steps函数可以实现分步过渡。

steps讲解：https://www.zhangxinxu.com/wordpress/2018/06/css3-animation-steps-step-start-end/

