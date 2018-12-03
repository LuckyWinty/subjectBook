### Safari 3D变换会忽略z-index的层级

在Safari浏览器下，此Safari浏览器包括iOS的Safari，iPhone上的微信浏览器，以及Mac OS X系统的Safari浏览器，当我们使用3D transform变换的时候，如果祖先元素没有overflow:hidden/scroll/auto等限制，则会直接忽略自身和其他元素的z-index层叠顺序设置，而直接使用真实世界的3D视角进行渲染。

例如：

![](/assets/css.png)

解决方法：

**方法1：**  
父级，任意父级，非body级别，设置`overflow:hidden`可恢复和其他浏览器一样的渲染。

**方法2：**  
以毒攻毒。有时候，页面复杂，我们不能给父级设置`overflow:hidden`，怎么办呢？将被影响的元素设置一个足够大的

`translateZ`值就可以，如`translateZ(100px).`

## 文字居中兼容

正常处理文字上下居中的手段是让元素height和line-height相等，但是安卓环境下当字体大小&lt;14px/0.7rem的时候会出现居中失效的情况

处理方式主要有两个：

1）判断系统环境（安卓/IOS）分别作微调；

2）font-size、height、width全部放大为2倍，利用transform进行缩放  
**height: 1rem;  
width: 2rem;  
font-size: 0.5rem;**

变成：**  
height: 2rem;  
width: 4rem;  
font-size: 1rem;  
transform: scale\(0.5\);**

但由于放大之后占据空间，左右会留白，需要利用margin负值【**margin: -0.35rem -0.45rem 0; 】**调整

3）有解决方案是将rem改为px。

## 旋转bug兼容

这里是2个a标签，做90度的旋转效果使得两个a可以切换展示。这里2个的基本样式是一致的，宽高也一样。但是在安卓下（ios正常）只有打开页面能看到的第一个a标签能正常跳转，能正常绑定事件。第二个a不能跳转，我就想那我通过点击事件来跳转可以不，结果绑定任何事件都不生效。然后测试发现，在旋转过程中（只要未完全旋转90度）点击还是能一切正常的。于是把旋转角度改为了89.99度，一切正常。后来查找了一下stackoverflow\([https://stackoverflow.com/questions/23548612/cant-click-on-buttons-after-css-transform](https://stackoverflow.com/questions/23548612/cant-click-on-buttons-after-css-transform)\)。

## css3蒙版巧用

 1.语法，-webkit-mask : url\("images/mask.png"\);

2.效果如下，注意mask.png中黑色代表是不透明的（alpha：1）,其他部分为透明的（alpha：0），将它盖在背景图上，注意：背景图对应mask.png中透明的位置也会变成透明，留下非透明的形状，即背景图的可见形状与mask.png的可见形状相同。 意为:‘蒙版’。

![](/assets/mask.png)

