---
title: webpack初认识
date: 2017-2-28 14:37:04
categories: 前端
tags:
 -构建工具
comments: true
---
### 什么是webpack?
一种模块化的解决方案。模块按需加载。（不算太懂）

### 怎么了解到webpack
去年我在学习echarts时候，深深被echarts的强大打动，然后就看了一下echarts的源码，一看就懵了，完全看不懂，当时还不懂前端模块化处理。于是慢慢接触到的。

### webpack的特点
##### 代码拆分
>Webpack有两种组织模块依赖的方式，同步和异步。异步依赖作为分割点，形成一个新的块。在优化了依赖树之后，每一个异步区都作为一个文件被打包。   

<!-- more -->
##### Loader
Webpack本身只能处理原生的JavaScript模块，但是Loader转换器可以将各种类型的资源转换成为js模块。这样任何资源都可以转换成为js模块。

##### 智能解析
Webpack有一个智能解析器，几乎可以处理任何第三方库，无论他们的形式是CommonJs、AMD、还是普通的js文件。甚至在加载依赖的时候可以用动态的表达式加载:require('.template'+name+'.jade')

##### 插件系统
Webpack还有功能丰富的插件系统。大多数内容功能都是基于这个插件系统运行的，还可以开发和使用开源的Webpack插件，来满足格式各样的要求。

##### 快速运行
Webpack使用异步I/O和多级缓存提高运行效率，这使得Webpack能够以令人难以置信的速度快速增量编译。

### Webpack的安装
```javascript
// 先全局安装
$ npm install webpack -g

// 在项目目录初始化package.json（如果有就不用）
$ npm init 

// 在项目目录安装Webpack依赖
$ npm install webpack --save-dev
或者
$ npm install webpack@2.2.1 --save-dev

// 如果需要使用 Webpack 开发工具，要单独安装
$ npm install webpack-dev-server --save-dev
```
这里的解释一下命令的作用。
npm init ： 在项目目录中创建package.json

npm install xxx --save-dev : 自动把模块加入项目package.json中的devDependencies，也把xxx依赖的模块，安装到本目录下。

### Webpack最简单打包
打包成一个文件。首先应该由入口文件:entry.js,和输出文件：bundle.js
1、首先创建index.html同目录下创建入口文件和输出文件。index.html引入输出文件
```html  
<!-- index.html -->
<html>
<head>
    <meta charset="UTF-8">
    <title></title>
    <script src="bundle.js"></script>
</head>
<body>
</body>
</html>
```

2、输出文件不需代码，webpack会将打包的entry.js输出到bundle.js
```javascript
// 入口文件entry.js
document.write('Hello Webpack')

```

3、打包
```cmd
$ webpack entry.js bundle.js
```
打开index.html就看到hello webpack了。

4、加一个模块module.js,修改入口文件entry.js
```javascript
// module.js
module.exports = function(){
    return "I'm Liyang";
}
```

```javascript
// entry.js
var str = require('./mudule');

document.write(str);
```

### Webpack Loader
>webpack本身只能处理javascript文件，如果要处理其他文件只能用Loader进行处理。
>Loader可以理解为模块和资源的转换器。它本身是个函数，接受源文件为参数，返回转换的结果。

##### Webpack Loader的特点
* 可以管道方式链式调用，可以把任意资源转换成任何格式然后传递给下个一个Loader,但是最后一个Loader必须返回javascript
* 可以同步或者异步执行
* 运行在node.js环境中，所以可以做任何事情
* 可以接受参数，用来传递配置项给Loader
* 可以通过文件扩展名（或者正则表达式）绑定给不同类型的文件
* 除了通过package.json 的main指定，通常的模块也可以导出一个Loader来使用
* 可以访问配置
* 插件可以让Loader拥有更多的特性
* 可以分发出任意的附加文件

Loader 通常使用npm管理，但是也可以在项目中自己写一个Loader模块。
一般情况：Loader以xxx-loader方式命名，xxx表示这个Loader要转的转换功能，比如json-loader,在引用时候，可以使用全名json-loader也可以使用短名：json,这个命名规则和搜索优先级顺序在webpack中的resolveLoader.moduleTemplates api中定义。

默认优先级：
```javascript
// 第一优先级以-webpack-loader结尾
// 第二-web-loader结尾
// 第三-loader结尾
// 第四是自定义名称
 Defaults:[*-webpack-loader,*-web-loader,*-loader,*];
```

##### Webpack Loader 调用方式
1. 可以在require()引用模块时候添加
2. 可以在webpack全局配置中进行绑定
3. 通过命令行方式使用

例子：require的方式
```css
/** style.css**/
body{
    background:yellow;
}
```
```javascript 
require("!style-loader!css-loader!./style.css") // 载入 style.css
document.write('It works.')
document.write(require('./module.js'))
```

安装Loader
```javascript 
$ npm install css-loader style-loader
```

例子：命令方式
如果每次require css文件的时候都要写loader的前缀，很麻烦，我们可以根据模块类型（扩展名）来进行自动绑定需要的Loader。

将 entry.js 中的 require("!style!css!./style.css") 修改为 require("./style.css")
```javascript
$ webpack entry.js bundle.js --module-bind 'css=sytle-loader!css-loader'

// 有些环境下需要用双引号

$ webpack entry.js bundle.js --module-bind "css=style-loader!css-loader"
```


### 配置文件
>Webpack在执行的时候，会搜索当前目录中webpack.config.js文件，这是一个node.js模块，返回一个json格式的配置信息对象，或者通过--config选项来指定配置文件。

先在package.json的文件中加入style-loader和css-loader的依赖。
```javascript 
$ npm install css-loader style-loader --save-dev
```

也可以在package.json 的devDependencies属性中加入你的css-loader style-loader然后运行**npm install**

```json 
{
  "name": "litest",
  "version": "1.0.0",
  "description": "my first project by webpack",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "css-loader": "^0.27.3",
    "style-loader": "^0.14.1",
    "webpack": "^2.2.1",
    "webpack-dev-server": "^2.4.1"
  }
}
```

然后创建一个webpack.config.js配置文件

```javascript 
var webpack = require('webpack')

module.exports = {
    entry:'./entry.js',
    output:{
        path:_dirname,
        filename:'bundle.js'
    },
    module:{
        loaders:[
            {
                test:/\.css$/,loader:'style-loader!css-loader'
            }
        ]
    }
}
```

entry.js中的require调用模块时候用css-loader,style-loader就不需要了。
直接在命令行输入**webpack**就ok

### Webpack插件
>webpack插件可以完成更多Loader不能完成的功能，在webpack.config.js中的plugins属性中指定。
>本身内置了一些插件，也可以通过npm安装。

例子：内置插件BannerPlugin（给输出的文件头部注释信息）
```javascript
var webpack = require('webpack');

module.exports = {
    entry:'./entry.js',
    output:{
        path:__dirname,
        filename:'bundle.js'
    },
    module:{
        loaders:{
            test:'\/.css$\',
            loader:'css-loader!style-loader'
        }
    },
    plugins:[
        new webpack.BannerPlugin('author:LazyYager')
    ]
}
```

