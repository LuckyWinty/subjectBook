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

Babel默认只转换新的JavaScript语法，而不转换新的API。 例如，Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise 等全局对象，以及一些定义在全局对象上的方法（比如 Object.assign）都不会转译。 如果想使用这些新的对象和方法，则需要为当前环境提供一个polyfill。

解决方案一：

引入babel-polyfill，它会”加载整个polyfill库”，针对编译的代码中新的API进行处理，并且在代码中插入一些帮助函数。

解决方案二：

babel-runtime

两种方案区别：

babel-polyfill解决了Babel不转换新API的问题，但是直接在代码中插入帮助函数，会导致污染了全局环境，并且不同的代码文件中包含重复的代码，导致编译后的代码体积变大。babel-runtime配置了之后，打包时会“按需加载”，当用到某个polifill时再引入对应的垫子，这样可以减少体积。但在某些情况下仍然不能被babel-runtime替代， 例如，代码：\[1, 2, 3\].includes\(3\)，Object.assign\({}, {key: 'value'}\)，Array，Object以及其他”实例”下es6的方法，babel-runtime是无法支持的， 因为babel-runtime只支持到static的方法。

因此，babel-runtime适合在组件，类库项目中使用，而babel-polyfill适合在业务项目中使用。

**babel-runtime版本搭配注意：**

```
//安装babel-runtime和babel-plugin-transform-runtime，配置如下：
{
"plugins": [
        "transform-runtime"
    ]
}
//高版本的babel，安装@babel/plugin-transform-runtime、@babel/runtime
{
"plugins": ["@babel/transform-runtime"]
}
```

2.转换css

相关的loder：less-loader、[postcss-loader](https://webpack.js.org/loaders/postcss-loader/)、css-loader、style-loader等。利用[mini-css-extract-plugin](https://github.com/webpack-contrib/mini-css-extract-plugin)来打包到一个文件里面。参考代码：

```
    const MiniCssExtractPlugin = require("mini-css-extract-plugin");
    module.exports = (env, argv) => {
      const devMode = argv.mode !== 'production'
      return {
        module: {
          rules: [
            {
                test: /\.css$/,
                use: [
                    devMode ? 'style-loader' : MiniCssExtractPlugin.loader,
                    'css-loader',
                    'postcss-loader'
                ]
            },
            ]
          },
          plugins: [
            new MiniCssExtractPlugin({
              filename: "[name].css",
              chunkFilename: "[id].css"
            })
          ]
      }
    }
```

3.处理html文件，一般用[html-webpack-plugin](https://webpack.js.org/guides/output-management/#setting-up-htmlwebpackplugin)，参考代码：

```
    const HtmlWebPackPlugin = require("html-webpack-plugin");
    module.exports = {
        module: {
            rules: [
                // ...,
                {
                    test: /\.html$/,
                    use: [{
                        loader: "html-loader",
                        options: {
                            minimize: true
                        }
                    }]
                }
            ]
        },
        plugins: [
            new HtmlWebPackPlugin({
                template: "./src/index.html",
                filename: "./index.html"
            })
        ]
    };
```

更多配置解释，参考[https://segmentfault.com/a/1190000007294861](https://segmentfault.com/a/1190000007294861)

4.清理

每次打包，都会生成项目的静态资源，随着某些文件的增删，我们的 dist 目录下可能产生一些不再使用的静态资源，webpack并不会自动判断哪些是需要的资源，为了不让这些旧文件也部署到生产环境上占用空间，所以在 webpack 打包前最好能清理 dist 目录。

```
  const CleanWebpackPlugin = require('clean-webpack-plugin');
  module.exports = {
    plugins: [
      new CleanWebpackPlugin(['dist']),
    ]
  };
```

5.资源处理

```
{
    test: /\.html$/,
    loader: 'art-template-loader',
}, {
    test: /\.jpg$/,
    loader: 'url-loader?mimetype=image/jpg'
}, {
    test: /\.png$/,
    loader: 'url-loader?mimetype=image/png'
},
{
    test: /\.svg/,
    loader: 'url-loader?mimetype=image/svg+xml'
}
```

6.server的启用

```
    "scripts": {
      "start": "webpack-dev-server --mode development --open",
      "build": "webpack --mode production"
    }
```

7.开启source-map

```
 devtool: "inline-source-map" //详细到打包前的每个没被压缩的文件
or
 devtool: "source-map" //打包后的未压缩文件
```

### production相关的一些配置

1、代码压缩，uglifyjs-webpack-plugin

```
new UglifyJsPlugin({
    test: /.js$|.jsx$/i,
    uglifyOptions: {
        compress: {
            pure_funcs: ['console.log', 'alert']
        },
    },
})
```

2.提取公共模块

```
output: {
    ...
    chunkFilename: '[name].[chunkhash:8].js'
},
...
optimization: {
    splitChunks: {
        cacheGroups: {
            vendor: {
                test: /[\\/]node_modules[\\/]/,
                name: 'common',
                chunks: 'all'
            }
        }
    }
}
```

详细参考：https://www.cnblogs.com/ufex/p/8758792.html



