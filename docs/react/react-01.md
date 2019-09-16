# 一个 React 组件是怎么渲染成 HTML DOM 的。

我们先看看一个最基本的React组件长什么样。
```jsx
const title = <h1 className="title">Hello, world!</h1>;
```
很明显它不是一个合法的js代码，它是`jsx`句法。那么React是怎么把它渲染到页面中的呢？往下看。

## 1.使用babel编译成合法js代码
上面例子中的代码会被编译成下面的样子，编以后已经是合法的js代码了。
```js
const title = React.createElement(
    'h1',
    { className: 'title' },
    'Hello, world!'
);
```
React.createElement函数的三个参数分别是 `tag` `attrs` `childrens`

## 2.将虚拟 DOM 转化成真实 DOM
2.1 创建标签
```js
const dom = document.createElement( vnode.tag );
```
2.2 处理属性
```js
if( vnode.attrs ){
    Object.keys( vnode.attrs ).forEach( key => {
        const value = vnode.attrs[ key ];
        setAttribute( dom, key, value );    // 设置属性
    } );
}
```
2.3 处理子元素
```js
vnode.children.forEach( child => render( child, dom ) );
```

> 以上就是一个 React 组件转化成 DOM 的过程。  
> 以上内容摘抄自：https://www.cnblogs.com/zhenfei-jiang/p/9682430.html  
