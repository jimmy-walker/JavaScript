## jQuery

[TOC]

### 简介（jQuery:JavaScript世界中使用最广泛的一个库，一个js文件）

目前jQuery有1.x和2.x两个主要版本，区别在于2.x移除了对古老的IE 6、7、8的支持，因此2.x的代码更精简。选择哪个版本主要取决于你是否想支持IE 6~8。

从[jQuery官网](http://jquery.com/download/)可以下载最新版本。jQuery只是一个`jquery-xxx.js`文件，但你会看到有compressed（已压缩）和uncompressed（未压缩）两种版本，使用时完全一样，但如果你想深入研究jQuery源码，那就用uncompressed版本。

使用jQuery只需要在页面的`<head>`引入jQuery文件即可：

```html
<html>
<head>
    <script src="//code.jquery.com/jquery-1.11.3.min.js"></script>
	...
</head>
<body>
    ...
</body>
</html>
```

#### $符号（jQuery的别名）

`$`是著名的jQuery符号。实际上，jQuery把所有功能全部封装在一个全局变量`jQuery`中，而`$`也是一个合法的变量名，它是变量`jQuery`的别名： 

#### 选择器（帮助快速定位DOM节点，比之前写法更简洁，类似$('#abc')，返回jQuery对象，类似数组）

jQuery的选择器就是帮助我们快速定位到一个或多个DOM节点。

 ##### 按ID查找

如果某个DOM节点有`id`属性，利用jQuery查找如下：

```javascript
// 查找<div id="abc">:
var div = $('#abc');
```

*注意*，`#abc`以`#`开头。返回的对象是jQuery对象。

什么是jQuery对象？jQuery对象类似数组，它的每个元素都是一个引用了DOM节点的对象。

以上面的查找为例，如果`id`为`abc`的`<div>`存在，返回的jQuery对象如下：

```javascript
[<div id="abc">...</div>]
```

如果`id`为`abc`的`<div>`不存在，返回的jQuery对象如下：

```javascript
[]
```

总之jQuery的选择器不会返回`undefined`或者`null`，这样的好处是你不必在下一行判断`if (div === undefined)`。

jQuery对象和DOM对象之间可以互相转化：

```javascript
var div = $('#abc'); // jQuery对象
var divDom = div.get(0); // 假设存在div，获取第1个DOM元素
var another = $(divDom); // 重新把DOM包装为jQuery对象
```

通常情况下你不需要获取DOM对象，直接使用jQuery对象更加方便。如果你拿到了一个DOM对象，那可以简单地调用`$(aDomObject)`把它变成jQuery对象，这样就可以方便地使用jQuery的API了。

##### 按tag查找

按tag查找只需要写上tag名称就可以了：

```javascript
var ps = $('p'); // 返回所有<p>节点
ps.length; // 数一数页面有多少个<p>节点
```



##### 按class查找

按class查找注意在class名称前加一个`.`：

```javascript
var a = $('.red'); // 所有节点包含`class="red"`都将返回
// 例如:
// <div class="red">...</div>
// <p class="green red">...</p>
```

通常很多节点有多个class，我们可以查找同时包含`red`和`green`的节点：

```javascript
var a = $('.red.green'); // 注意没有空格！
// 符合条件的节点：
// <div class="red green">...</div>
// <div class="blue green red">...</div>
```

##### 按属性查找

一个DOM节点除了`id`和`class`外还可以有很多属性，很多时候按属性查找会非常方便，比如在一个表单中按属性来查找：

```javascript
var email = $('[name=email]'); // 找出<??? name="email">
var passwordInput = $('[type=password]'); // 找出<??? type="password">
var a = $('[items="A B"]'); // 找出<??? items="A B">
```

当属性的值包含空格等特殊字符时，需要用双引号括起来。

按属性查找还可以使用前缀查找或者后缀查找：

```javascript
var icons = $('[name^=icon]'); // 找出所有name属性值以icon开头的DOM
// 例如: name="icon-1", name="icon-2"
var names = $('[name$=with]'); // 找出所有name属性值以with结尾的DOM
// 例如: name="startswith", name="endswith"
```

这个方法尤其适合通过class属性查找，且不受class包含多个名称的影响：

```javascript
var icons = $('[class^="icon-"]'); // 找出所有class包含至少一个以`icon-`开头的DOM
// 例如: class="icon-clock", class="abc icon-home"
```

##### 组合查找

组合查找就是把上述简单选择器组合起来使用。如果我们查找`$('[name=email]')`，很可能把表单外的`<div name="email">`也找出来，但我们只希望查找`<input>`，就可以这么写：

```javascript
var emailInput = $('input[name=email]'); // 不会找出<div name="email">
```

同样的，根据tag和class来组合查找也很常见：

```javascript
var tr = $('tr.red'); // 找出<tr class="red ...">...</tr>
```

##### 多项选择器

多项选择器就是把多个选择器用`,`组合起来一块选：

```javascript
$('p,div'); // 把<p>和<div>都选出来
$('p.red,p.green'); // 把<p class="red">和<p class="green">都选出来
```

要注意的是，选出来的元素是按照它们在HTML中出现的顺序排列的，而且不会有重复元素。例如，`<p class="red green">`不会被上面的`$('p.red,p.green')`选择两次。

####操作DOM（无需再分浏览器，统一操作即可）

回顾一下修改DOM的CSS、文本、设置HTML有多么麻烦，而且有的浏览器只有innerHTML，有的浏览器支持innerText，有了jQuery对象，不需要考虑浏览器差异了，全部统一操作！

J待看。

####事件（用选择器返回jQuery对象，然后调用on或click）

因为JavaScript在浏览器中以单线程模式运行，页面加载后，一旦页面上所有的JavaScript代码被执行完后，就只能依赖触发事件来执行JavaScript代码。

```javascript
/* HTML:
 *
 * <a id="test-link" href="#0">点我试试</a>
 *
 */

// 获取超链接的jQuery对象:
var a = $('#test-link');
a.on('click', function () {
    alert('Hello!');
});
```

`on`方法用来绑定一个事件，我们需要传入事件名称和对应的处理函数。

另一种更简化的写法是直接调用`click()`方法：

```
a.click(function () {
    alert('Hello!');
});
```

两者完全等价。我们通常用后面的写法。

jQuery能够绑定的事件主要包括：

##### 鼠标事件

- click: 鼠标单击时触发；
- dblclick：鼠标双击时触发；
- mouseenter：鼠标进入时触发；
- mouseleave：鼠标移出时触发；
- mousemove：鼠标在DOM内部移动时触发；
- hover：鼠标进入和退出时触发两个函数，相当于mouseenter加上mouseleave。

##### 键盘事件

键盘事件仅作用在当前焦点的DOM上，通常是`<input>`和`<textarea>`。

- keydown：键盘按下时触发；
- keyup：键盘松开时触发；
- keypress：按一次键后触发。

##### 其他事件

- focus：当DOM获得焦点时触发；
- blur：当DOM失去焦点时触发；
- change：当`<input>`、`<select>`或`<textarea>`的内容改变时触发；
- submit：当`<form>`提交时触发；
- ready：当页面被载入并且DOM树完成初始化后触发。

#### ajax($.ajax，返回jqXHR对象，继承了promise)

用JavaScript写AJAX前面已经介绍过了，主要问题就是不同浏览器需要写不同代码，并且状态和错误处理写起来很麻烦。

用jQuery的相关对象来处理AJAX，不但不需要考虑浏览器问题，代码也能大大简化。

jQuery在全局对象`jQuery`（也就是`$`）绑定了`ajax()`函数，可以处理AJAX请求。`ajax(url, settings)`函数需要接收一个URL和一个可选的`settings`对象，常用的选项如下：

- async：是否异步执行AJAX请求，默认为`true`，千万不要指定为`false`；
- method：发送的Method，缺省为`'GET'`，可指定为`'POST'`、`'PUT'`等；
- contentType：发送POST请求的格式，默认值为`'application/x-www-form-urlencoded; charset=UTF-8'`，也可以指定为`text/plain`、`application/json`；
- data：发送的数据，可以是字符串、数组或object。如果是GET请求，data将被转换成query附加到URL上，如果是POST请求，根据contentType把data序列化成合适的格式；
- headers：发送的额外的HTTP头，必须是一个object；
- dataType：接收的数据格式，可以指定为`'html'`、`'xml'`、`'json'`、`'text'`等，缺省情况下根据响应的`Content-Type`猜测。

下面的例子发送一个GET请求，并返回一个JSON格式的数据：

```javascript
var jqxhr = $.ajax('/api/categories', {
    dataType: 'json'
});
// 请求已经发送了
```

不过，如何用回调函数处理返回的数据和出错时的响应呢？

$.ajax() 返回jqXHR对象。jQuery的jqXHR对象类似一个Promise对象，我们可以用链式写法来处理各种回调：

```javascript
'use strict';

function ajaxLog(s) {
    var txt = $('#test-response-text');
    txt.val(txt.val() + '\n' + s);
}

$('#test-response-text').val('');

var jqxhr = $.ajax('/api/categories', {
    dataType: 'json'
}).done(function (data) {
    ajaxLog('成功, 收到的数据: ' + JSON.stringify(data));
}).fail(function (xhr, status) {
    ajaxLog('失败: ' + xhr.status + ', 原因: ' + status);
}).always(function () {
    ajaxLog('请求完成: 无论成功或失败都会调用');
});
```

jqXHR对象 实现了 Promise 接口, 使它拥有了 Promise 的所有属性，方法和行为。

- jqXHR.done(function(data, textStatus, jqXHR) {});
- jqXHR.fail(function(jqXHR, textStatus, errorThrown) {});
- jqXHR.always(function(data|jqXHR, textStatus, jqXHR|errorThrown) { });
- jqXHR.then(function(data, textStatus, jqXHR) {}, function(jqXHR, textStatus, errorThrown) {});

###Reference

- [jquery ajax 之 jqXHR 和 Data Types详解](https://www.jianshu.com/p/c6a0dc9000ef )
- [jQuery-廖雪峰的教程](https://www.liaoxuefeng.com/wiki/1022910821149312/1023022609723552 )