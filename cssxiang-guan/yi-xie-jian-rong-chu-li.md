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



