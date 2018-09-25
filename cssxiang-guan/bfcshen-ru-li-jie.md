#### 关于BFC的定义 {#-bfc-}

BFC\(Block formatting context\)直译为"块级格式化上下文"。它**是一个独立的渲染区域**，只有**Block-level box**参与（在下面有解释）， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。

我们常说的文档流其实分为定位流、浮动流和普通流三种。而**普通流其实就是指BFC中的FC**。

**FC**是formatting context的首字母缩写，直译过来是格式化上下文，它**是页面中的一块渲染区域**，有一套渲染规则，决定了其**子元素如何布局，以及和其他元素之间的关系和作用。**

**BFC 可以简单的理解为某个元素的一个 CSS 属性，只不过这个属性不能被开发者显式的修改，拥有这个属性的元素对内部元素和外部元素会表现出一些特性，这就是BFC。**

#### BFC触发条件（满足下列条件之一就可触发BFC）

1. 根元素，即HTML元素
2. float的值不为none
3. overflow的值不为visible
4. display的值为inline-block、table-cell、table-caption
5. position的值为absolute或fixed

#### BFC布局规则

1.内部的Box会在垂直方向，一个接一个地放置。

2.Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠

3.每个元素的margin box的左边， 与包含块border box的左边相接触\(对于从左往右的格式化，否则相反\)。即使存在浮动也是如此。

4.BFC的区域不会与float box重叠。

5.BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。

6.计算BFC的高度时，浮动元素也参与计算

#### BFC有哪些作用

1. 自适应两栏布局
2. 可以阻止元素被浮动元素覆盖
3. 可以包含浮动元素——清除内部浮动
4. 分属于不同的BFC时可以阻止margin重叠

更多内容参考：https://juejin.im/post/5909db2fda2f60005d2093db



