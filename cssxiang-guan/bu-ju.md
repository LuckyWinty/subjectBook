#### 网页布局有哪几种，有什么区别

1. 静态、自适应、流式、响应式四种网页布局
2. 静态布局：意思就是不管浏览器尺寸具体是多少，网页布局就按照当时写代码的布局来布置；
3. 自适应布局：就是说你看到的页面，里面元素的位置会变化而大小不会变化；
4. 流式布局：你看到的页面，元素的大小会变化而位置不会变化——这就导致如果屏幕太大或者太小都会导致元素无法正常显示。
5. 自适应布局：每个屏幕分辨率下面会有一个布局样式，同时位置会变而且大小也会变。

根据[CSS3规范](https://drafts.csswg.org/css-values-3/#viewport-relative-lengths)，视口单位主要包括以下4个：

* **vw**: 1vw 等于视口宽度的1%
* **vh**: 1vh 等于视口高度的1%
* **vmin**: 选取 vw 和 vh 中最小的那个
* **vmax**: 选取 vw 和 vh 中最大的那个

## 兼容性 {#兼容性}

其兼容性如下图所示，可以知道：**在移动端 iOS 8 以上以及 Android 4.4 以上获得支持，并且在微信 x5 内核中也得到完美的全面支持。**

适应布局方案一：搭配vw和rem，布局更优化

rem 弹性布局的核心在于动态改变根元素大小，那么我们可以通过：

1. 给根元素大小设置随着视口变化而变化的 vw 单位，这样就可以实现动态改变其大小。
2. 限制根元素字体大小的最大最小值，配合 body 加上最大宽度和最小宽度

这样我们就能够实现对布局宽度的最大最小限制。因此，根据以上条件，我们可以得出代码实现如下：

```
// rem 单位换算：定为 75px 只是方便运算，750px-75px、640-64px、1080px-108px，如此类推
$vw_fontsize: 75; // iPhone 6尺寸的根元素大小基准值
@function rem($px) {
     @return ($px / $vw_fontsize ) * 1rem;
}
// 根元素大小使用 vw 单位
$vw_design: 750;
html {
    font-size: ($vw_fontsize / ($vw_design / 2)) * 100vw; 
    // 同时，通过Media Queries 限制根元素最大最小值
    @media screen and (max-width: 320px) {
        font-size: 64px;
    }
    @media screen and (min-width: 540px) {
        font-size: 108px;
    }
}
// body 也增加最大最小宽度限制，避免默认100%宽度的 block 元素跟随 body 而过大过小
body {
    max-width: 540px;
    min-width: 320px;
}
```

适应布局方案二：仅使用vw作为CSS单位

```
//iPhone 6尺寸作为设计稿基准
$vw_base: 375; 
@function vw($px) {
    @return ($px / 375) * 100vw;
}
```

使用：

```
.test {    
    &_list {
        display: flex;
        padding: vw(15) vw(10) vw(10); // 内间距
        &_item {
            flex: 1;
            text-align: center;
            font-size: vw(10); // 字体大小
            &_logo {
                display: block;
                margin: 0 auto;
                width: vw(40); // 宽度
                height: vw(40); // 高
            }
        }
    }
}
```

结论

相对于做法二，个人比较推崇做法一，有以下两点原因：

第一，做法一 相对来说用户视觉体验更好，增加了最大最小宽度的限制；

第二，更重要是，如果选择主流的rem弹性布局方式作为项目开发的适配页面方法，那么做法一更适合于后期项目从 rem 单位过渡到 vw 单位。只需要通过改变根元素大小的计算方式，你就可以不需要其他任何的处理，就无缝过渡到另一种CSS单位，更何况vw单位的使用必然会成为一种更好适配方式，目前它只是碍于兼容性的支持而得不到广泛的应用。

特殊布局，特殊处理

[https://mp.weixin.qq.com/s/PrKDFGmJi8Ln8ZbFRnwNoQ](https://mp.weixin.qq.com/s/PrKDFGmJi8Ln8ZbFRnwNoQ)

