# Webpack 怎么做到热刷新和热加载？

## 1.启用热更新
> 启用热更新非常的简单只要在webpack配置中增加下面这些配置就可以了。

**webpack.config.js的配置**
```js
const webpack = require('webpack');
module.exports = {
    devServer: {
        hot: true, // 启用热更新
    },
    plugins: [
        // 当开启 HMR 的时候使用该插件会显示模块的相对路径，建议用于开发环境。
        new webpack.NamedModulesPlugin(),
        // 热替换模块(Hot Module Replacement)，也被称为 HMR。
        new webpack.HotModuleReplacementPlugin(),
    ],
}
```

## 2.CSS热更新

> css热更新只要配置了`style-loader`就可以了。

**webpack.config.js的配置**
```js
module.exports = {
    module: {
        rules: [
            {
                test: /\.css$/,
                use: ['style-loader', 'css-loader']
            },
        ]
    },
}
```


## 3.JS热更新

> JS代码实现热更新相对复杂，需要写一些代码来完成。  
> 首先配置还是`1.启用热更新`中的配置，下面我写一个例子来实现JS热更新。

**文件`buttonClickHandler.js`**
```js
export default function () {
    console.log('您点击了按钮！');
}
```

**文件`index.js`**
```js
import buttonClickHandler from './buttonClickHandler';
function createButton(clickHandler) {
    const button = document.createElement('button');
    button.innerText = '我是一个按钮';
    button.addEventListener('click', clickHandler, false);
    return button;
}
let button = createButton(buttonClickHandler);
document.body.appendChild(button);

// 需要额外写的代码
if (module.hot) {
    module.hot.accept('./buttonClickHandler.js', function () {
        console.log('buttonClickHandler 更新了');
        buttonClickHandler();
    });
}
```

通过上面的例子就实现了js代码的热更新，大家可以修改`buttonClickHandler.js`中的log测试一下。  
也许你会发现，修改完代码后控制台输出的log是改变了，但是点击按钮还是输出了以前的log。为什么呢？因为按钮绑定的监听事件依然是以前的函数，并没有更新成新的函数，我再完善一下。

**文件`index.js`代码修改**
```js
function createButton(clickHandler) {
    const button = document.createElement('button');
    button.innerText = '我是一个按钮';
    button.addEventListener('click', clickHandler, false);
    return button;
}
let button = createButton(buttonClickHandler);
document.body.appendChild(button);

// 需要额外写的代码
if (module.hot) {
    module.hot.accept('./buttonClickHandler.js', function () {
        console.log('buttonClickHandler 更新了');
        // buttonClickHandler();
        // 新增加代码
        document.body.removeChild(button);
        button = createButton(buttonClickHandler);
        document.body.appendChild(button);
    });
}
```
> 现在再测试就会发现再点击按钮已经实现了输出log的热更新。  
> 其实css也是用的同样的处理方式，只不过loader已经帮我们处理好了。  
> 另外第三方框架也为我们处理好了，不需要我们写这些代码来实现。  
