## NodeJs( Node.js 就是运行在服务端的 JavaScript)

[TOC]

简单的说 Node.js 就是运行在服务端的 JavaScript，基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。 **J理解为nodejs就是JavaScript，只不过因为V8引擎的原因，在服务器端执行起来非常快。**

### 安装

目前Node.js的最新版本是7.6.x。首先，从[Node.js官网](https://nodejs.org/)下载对应平台的安装程序，网速慢的童鞋请移步[国内镜像](https://pan.baidu.com/s/1kU5OCOB#list/path=%2Fpub%2Fnodejs)。

在Windows上安装时务必选择全部组件，包括勾选`Add to Path`。

安装完成后，在Windows环境下，请打开命令提示符，然后输入`node -v`，如果安装正常，你应该看到`v7.6.0`这样的输出：

```
C:\Users\IEUser>node -v
v7.6.0
```

继续在命令提示符输入`node`，此刻你将进入Node.js的交互环境。在交互环境下，你可以输入任意JavaScript语句，例如`100+200`，回车后将得到输出结果。

要退出Node.js环境，连按两次Ctrl+C。

#### npm（是Node.js的包管理工具（package manager） ）

其实npm已经在Node.js安装的时候顺带装好了。我们在命令提示符或者终端输入`npm -v`，应该看到类似的输出：

```
C:\>npm -v
4.1.2
```

### 模块（一个.js文件就称之为一个模块，用module.exports = variable进行对外暴露变量 ）

模块是Node.js 应用程序的基本组成部分，文件和模块是一一对应的。换言之，**一个 Node.js 文件就是一个模块，这个文件可能是JavaScript 代码、JSON 或者编译过的C/C++ 扩展**。 

在node的代码中使用`require`加载模块，在模块中使用`exports`或者`module.exports`导出接口，`require`、`module`、`exports`都是node的全局对象，我们不需要在模块中定义它们就可以直接使用 

####require（引入模块，作为变量，函数也是变量 ）

```javascript
var hello = require('./hello');
```

require用来引入模块，代码 require('./hello') 引入了当前目录下的 hello.js 文件（./ 为当前目录，node.js 默认后缀为 js）。

要在模块中对外输出变量，用：

```javascript
'use strict';

var s = 'Hello';

function greet(name) {
    console.log(s + ', ' + name + '!');
}

module.exports = greet;
```

输出的变量可以是任意对象、函数、数组等等。

要引入其他模块输出的对象，用：

```javascript
'use strict';

// 引入hello模块:
var greet = require('./hello');

var s = 'Michael';

greet(s); // Hello, Michael!
```

引入的对象具体是什么，取决于引入模块输出的对象。

#### scoped packages

在 npm 的包管理系统中，有一种 scoped packages 机制，用于将一些 packages 以 `@scope/package` 的命名形式集中在一个命名空间下面。**scopes are preceded by an `@` symbol and followed by a slash**.

位置在node_modules文件夹下。

而还有一个@没有找到明确解释，可能是指代src目录吧：

```javascript
const config = require('@/config').value;
```



### 基本模块

#### http

`request`对象封装了HTTP请求，我们调用`request`对象的属性和方法就可以拿到所有HTTP请求的信息；

`response`对象封装了HTTP响应，我们操作`response`对象的方法，就可以把HTTP响应返回给浏览器。

用Node.js实现一个HTTP服务器程序非常简单。我们来实现一个最简单的Web程序`hello.js`，它对于所有请求，都返回`Hello world!`：

```javascript
'use strict';

// 导入http模块:
var http = require('http');

// 创建http server，并传入回调函数:
var server = http.createServer(function (request, response) {
    // 回调函数接收request和response对象,
    // 获得HTTP请求的method和url:
    console.log(request.method + ': ' + request.url);
    // 将HTTP响应200写入response, 同时设置Content-Type: text/html:
    response.writeHead(200, {'Content-Type': 'text/html'});
    // 将HTTP响应的HTML内容写入response:
    response.end('<h1>Hello world!</h1>');
});

// 让服务器监听8080端口:
server.listen(8080);

console.log('Server is running at http://127.0.0.1:8080/');
```

在命令提示符下运行该程序，可以看到以下输出：

```
$ node hello.js 
Server is running at http://127.0.0.1:8080/
```

不要关闭命令提示符，直接打开浏览器输入`http://localhost:8080`，即可看到服务器响应的内容。

同时，在命令提示符窗口，可以看到程序打印的请求信息：

```
GET: /
GET: /favicon.ico
```

这就是我们编写的第一个HTTP服务器程序！

### Web开发相关模块

#### koa（web框架）

koa是Express的下一代基于Node.js的web框架，目前有1.x和2.0两个版本。

## Reference

- [Node.js教程](https://www.runoob.com/nodejs/nodejs-tutorial.html )