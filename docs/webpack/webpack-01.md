# Webpack 打包时怎么抽离第三方插件呢？
> 因为第三方插件变化较少，单独打包可以直接走浏览器缓存，提高加载速度。

## 1.第三方库单独的入口

```js
module.exports = {
    entry:{
        app: './src/index.js',
        jquery:'jquery', // 要抽离的jquery库
    },
}
```

## 2.可以全局引入公共库（可选）
> 全局引入公共库后代码中不需要再单独引入，可以直接使用`$`  

```js
const webpack = require('webpack');
module.exports = {
    plugins: [
        // 全局引入jquery
        new webpack.ProvidePlugin({
            $:'jquery'
        }),
    ],
}
```

## 3.打包抽离公共库配置（Webpack 3）
> Webpack 3中已经移除了 `webpack.optimize.CommonsChunkPlugin` 插件，Webpack 4直接移步下一项  

```js
const webpack = require('webpack');
module.exports = {
    plugins: [
        // 抽离公共库
        new webpack.optimize.CommonsChunkPlugin({
            //name对应入口文件中的名字，我们起的是jQuery
            name:['jquery'],
            //把文件打包到哪里，是一个路径
            filename:"[name].js",
            //最小打包的文件模块数
            minChunks:2
        }),
    ],
}
```

## 4.打包抽离公共库配置（Webpack 4）

```js
module.exports = {
    optimization: {
        splitChunks: {
            cacheGroups: {
                jquery: {
                    name: "jquery",
                    chunks: "initial",
                    minChunks: 2
                }
            }
        }
    }
}
```

## 最终webpack.config.js全部配置
```js
const path = require('path');
const webpack = require('webpack');

module.exports = {
    entry: {
        app: './src/index.js',
        jquery: 'jquery',
    },
    output: {
        filename: '[name].js',
        path: path.resolve(__dirname, 'dist')
    },
    plugins: [
        // 全局引入jquery
        new webpack.ProvidePlugin({
            $:'jquery'
        }),
        // webpack3.0 单独打包外部库的方法
        // new webpack.optimize.CommonsChunkPlugin({
        //     name:['jquery'], //对应入口文件的jquery(单独抽离)
        //     filename:'[name].js', //抽离到哪里
        //     minChunks: 2, //抽离几个文件
        // }),
    ],
    // webpack4.0 打包外部库的方法
    optimization: {
        splitChunks: {
            cacheGroups: {
                jquery: {
                    name: "jquery",
                    chunks: "initial",
                    minChunks: 2
                }
            }
        }
    }
};
```