
## require.resolve
返回模块的绝对路径，并检查模块是否存在，不存在会报错。

## path.resolve
获取绝对路径，不管路径是否真实存在。  
多个参数的意义就是`cd`，从第一个参数开始cd，直到最后一个参数。然后找到最终绝对路径。

## 路径 `/src` 和 `src` 、 `./src` 有什么区别
`/` 好像是根目录，根目录是电脑的根目录。`/src`是根目录下的src目录（这个windows系统应该没有。）  
`src`和`./src`是相对目录
>  `__dirname`：当前路径的绝对路径
