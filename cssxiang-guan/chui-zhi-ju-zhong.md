实现已知或者未知宽度的垂直水平居中。

```
// 1
.wraper {
  position: relative;
  .box {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 100px;
    height: 100px;
    margin: -50px 0 0 -50px;
  }
}


// 2
.wraper {
  position: relative;
  .box {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
}


// 3
.wraper {
  .box {
    display: flex;
    justify-content:center;
    align-items: center;
    height: 100px;
  }
}


// 4
.wraper {
  display: table;
  .box {
    display: table-cell;
    vertical-align: middle;
  }
}
//5
.container {
  display: grid;
  grid-auto-columns: 1fr;
  grid-auto-rows: 200px;
  background: #eee;
}
.parent {
  background: grey;
  justify-self: center; 
  align-self: center;  
}
.child {
  font-size: 30px;
}
//6、块级元素：calc()
.parent {
    width: 300px;
    height: 300px;
    border: 1px solid red;
    position: relative;
}
.child {
    width: 100px;
    height: 100px;
    background: blue;
    padding: -webkit-calc((100% - 100px) / 2);
    padding: -moz-calc((100% - 100px) / 2);
    padding: -ms-calc((100% - 100px) / 2);
    padding: calc((100% - 100px) / 2);
    background-clip: content-box;
}
//7、margin:auto实现绝对定位元素的居中
.element {
    width: 600px; height: 400px;
    position: absolute; left: 0; top: 0; right: 0; bottom: 0;
    margin: auto;    /* 有了这个就自动居中了 */
}
```

#### margin:auto实现垂直居中，居中的原因分析

margin概念：

margin属性为给定元素设置所有四个（上下左右）方向的外边距属性。这是四个外边距属性设置的简写。四个外边距属性设置分别是： [`margin-top`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-top)，[`margin-right`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-right)，[`margin-bottom`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-bottom)和[`margin-left`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-left)。指定的外边距允许为负数。

margin的top和bottom属性对非替换内联元素无效，例如[`<span>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/span)和 [`<code>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/code)。

