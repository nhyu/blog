# 搭建 SASS 学习环境

> 看了 SASS 的官方文档，但是我并没能清楚的知道我怎么来搭建一个学习环境。于是又搜索了很多大佬的分享才明白，原来这么简单。  
> 参考：[Sass 中文网](https://www.sass.hk/) 和 [node-sass - npm](https://www.npmjs.com/package/node-sass)

## 准备

- 请自行安装 node 环境
- 我的系统是 mac，其他系统的可能会出现命令不一致的情况，请自行解决。

## 开始安装

直接在控制台运行下面命令，学习环境用到的东西就全了

```shall
mkdir my-sass && cd my-sass && npm init -y && npm i -D node-sass
```

> 命令解释  
> `mkdir my-sass`: 新建一个名字是 my-sass 的文件夹  
> `cd my-sass`: 进入 my-sass 文件夹  
> `npm init -y`: npm 初始化项目  
> `npm i -D node-sass`: 安装 node-sass (最重要的一个命令)

## 配置环境

在 package.json 的 scripts 中增加一下配置

```json
"scripts": {
    "sass": "node-sass",
    "dev" : "node-sass --watch index.scss output.css"
}
```

> 需要改变代码目录的请自行修改命令(例如："node-sass --watch src/index.scss dist/output.css")  
> 更多命令请参考：https://www.npmjs.com/package/node-sass#command-line-interface  
> package.json 完整配置在最下面

## 启动项目

有两种方法进行 sass 编译

### 先准备测试代码

新建 input.scss 文件

```css
.test {
  color: red;
}
```

### 编译方法一

`npm run sass input.scss output.css`

> 这个方法每次要编译都要运行一遍编译命令。

### 编译方法二（自动编译）

`npm run dev`

> 使用方法二代码改变会自动进行编译

### 检查结果

这里你就可以看到项目中多了一个 output.scc 文件，打开应该是这样的

```css
.test {
  color: red;
}
```

-the end-

---

package.json 的完整配置

```json
{
  "name": "my-sass",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "sass": "node-sass",
    "dev": "node-sass --watch index.scss output.css"
  },
  "repository": {
    "type": "git",
    "url": ""
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "node-sass": "^5.0.0"
  }
}
```
