## NodeJs

简单的说 Node.js 就是运行在服务端的 JavaScript，基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。 **J理解为nodejs就是JavaScript，只不过因为V8引擎的原因，在服务器端执行起来非常快。**

## 模块

模块是Node.js 应用程序的基本组成部分，文件和模块是一一对应的。换言之，**一个 Node.js 文件就是一个模块，这个文件可能是JavaScript 代码、JSON 或者编译过的C/C++ 扩展**。 

在node的代码中使用`require`加载模块，在模块中使用`exports`或者`module.exports`导出接口，`require`、`module`、`exports`都是node的全局对象，我们不需要在模块中定义它们就可以直接使用 

###require

```javascript
var hello = require('./hello');
```

require用来引入模块，代码 require('./hello') 引入了当前目录下的 hello.js 文件（./ 为当前目录，node.js 默认后缀为 js）。



 ## scoped packages

在 npm 的包管理系统中，有一种 scoped packages 机制，用于将一些 packages 以 `@scope/package` 的命名形式集中在一个命名空间下面。**scopes are preceded by an `@` symbol and followed by a slash**.

位置在node_modules文件夹下。

而还有一个@没有找到明确解释，可能是指代src目录吧：

```javascript
const config = require('@/config').value;
```



## Reference

- [Node.js教程](https://www.runoob.com/nodejs/nodejs-tutorial.html )