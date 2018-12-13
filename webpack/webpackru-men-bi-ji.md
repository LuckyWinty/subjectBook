### 概念

Webpack是一个**模块打包机**，它可以将我们项目中的所有js、图片、css等资源，根据其入口文件的依赖关系，打包成一个能被浏览器识别的js文件。能够帮助前端开发将打包的过程更智能化和自动化。

#### WebPack和Grunt以及Gulp相比有什么特性

Webpack和另外两个并没有太多的可比性，Gulp/Grunt是一种能够优化前端的开发流程的工具，而WebPack是一种模块化的解决方案，不过Webpack的优点使得Webpack在很多场景下可以替代Gulp/Grunt类的工具。

### 开始使用

安装webpack，目前webpack已更新到4.x版本，且大力度宣传的是 cli 方式启动，更推崇的是让开发者使用高度封装的 cli。基于此，我们应该安装`webpack^4.1.1、webpack-cli^2.0.10、webpack-dev-server^3.1.0`，以及创建一个公共配置文件`webpack.config.js。`

