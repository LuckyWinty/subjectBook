### 概念

Webpack是一个**模块打包机**，它可以将我们项目中的所有js、图片、css等资源，根据其入口文件的依赖关系，打包成一个能被浏览器识别的js文件。能够帮助前端开发将打包的过程更智能化和自动化。

#### WebPack和Grunt以及Gulp相比有什么特性

Webpack和另外两个并没有太多的可比性，Gulp/Grunt是一种能够优化前端的开发流程的工具，而WebPack是一种模块化的解决方案，不过Webpack的优点使得Webpack在很多场景下可以替代Gulp/Grunt类的工具。

### 开始使用

安装webpack，目前webpack已更新到4.x版本，且大力度宣传的是 cli 方式启动，更推崇的是让开发者使用高度封装的 cli。基于此，我们应该安装`webpack^4.1.1、webpack-cli^2.0.10`\(想要执行webpack的命令必须有这个包\)`、webpack-dev-server^3.1.0。`

### webpack的mode

webpack4 的`mode`给出了两种配置：`development`和`production`。生产模式下，启用了 代码压缩、作用域提升（scope hoisting）、 tree-shaking，使代码最精简；开发模式下，相较于更小体积的代码，提供的是打包速度上的优化。所以，我们一般配置2个文件，webpack.dev.conf.js和webpack.prod.conf.js。修改package.json里面的scripts：

```
"scripts": {
  "dev": "webpack --mode development",
  "build": "webpack --mode production"
}
```

### 按项目搭建诉求加入配置

一般诉求：

1. js的处理：转换 ES6 代码，解决浏览器兼容问题
2. css的处理：编译css，自动添加前缀，抽取css到独立文件
3. html的处理：复制并压缩html文件
4. dist的清理：打包前清理源目录文件
5. assets的处理：静态资源处理
6. server的启用：development 模式下启动服务器并实时刷新

1.转换js，解决兼容性问题，用 babel 转换 ES6 代码，用 babel 转换 ES6 代码需要使用到**babel-loader**，我们需要安装一系列的依赖：

```
    npm i babel-core babel-loader babel-preset-env --save-dev
```

然后在根目录新建一个babel配置文件`.babelrc`：

```
    {
        "presets": [
            "env"
        ]
    }
```



