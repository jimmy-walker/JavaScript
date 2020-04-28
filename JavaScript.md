# JavaScript

## 快速入门

JavaScript代码可以直接嵌在网页的任何地方，不过通常我们都把JavaScript代码放到`<head>`中：

```html
<html>
<head>
  <script>
    alert('Hello, world');
  </script>
</head>
<body>
  ...
</body>
</html>
```

由`<script>...</script>`包含的代码就是JavaScript代码，它将直接被浏览器执行。

第二种方法是把JavaScript代码放到一个单独的`.js`文件，然后在HTML中通过`<script src="..."></script>`引入这个文件：

```html
<html>
<head>
  <script src="/static/js/abc.js"></script>
</head>
<body>
  ...
</body>
</html>
```

这样，`/static/js/abc.js`就会被浏览器执行。

**把JavaScript代码放入一个单独的`.js`文件中更利于维护代码，并且多个页面可以各自引用同一份`.js`文件。**

### 语法

JavaScript的语法和Java语言类似，每个语句以`;`结束，语句块用`{...}`。但是，JavaScript并不强制要求在每个语句的结尾加`;`，浏览器中负责执行JavaScript代码的引擎会自动在每个语句的结尾补上`;`。

以`//`开头直到行末的字符被视为行注释，注释是给开发人员看到，JavaScript引擎会自动忽略：

 另一种块注释是用`/*...*/`把多行字符包裹起来，把一大“块”视为一个注释：

##数据类型

###Number

JavaScript不区分整数和浮点数，统一用Number表示，以下都是合法的Number类型：

```javascript
123; // 整数123
0.456; // 浮点数0.456
1.2345e3; // 科学计数法表示1.2345x1000，等同于1234.5
-99; // 负数
NaN; // NaN表示Not a Number，当无法计算结果时用NaN表示
Infinity; // Infinity表示无限大，当数值超过了JavaScript的Number所能表示的最大值时，就表示为Infinity
```

### 字符串

字符串是以单引号'或双引号"括起来的任意文本，比如`'abc'`，`"xyz"`等等。

#### 转义\

JavaScript的字符串就是用`''`或`""`括起来的字符表示。

 如果字符串内部既包含`'`又包含`"`怎么办？可以用转义字符`\`来标识，比如：

```javascript
'I\'m \"OK\"!';
```

表示的字符串内容是：`I'm "OK"!`

####多行字符串`

由于多行字符串用`\n`写起来比较费事，所以最新的ES6标准新增了一种多行字符串的表示方法，用反引号`表示：

```JavaScript
`这是一个
多行
字符串`
```

####模板字符串+或${}

要把多个字符串连接起来，可以用`+`号连接：

```javascript
var name = '小明';
var age = 20;
var message = '你好, ' + name + ', 你今年' + age + '岁了!';
alert(message);
```

如果有很多变量需要连接，用`+`号就比较麻烦。ES6新增了一种模板字符串，表示方法和上面的多行字符串一样，但是它会自动替换字符串中的变量：

```javascript
var name = '小明';
var age = 20;
var message = `你好, ${name}, 你今年${age}岁了!`;
alert(message);
```

#### 索引[]

```javascript
var s = 'Hello, world!';

s[0]; // 'H'
s[6]; // ' '
s[7]; // 'w'
s[12]; // '!'
s[13]; // undefined 超出范围的索引不会报错，但一律返回undefined
```

####字符串函数

`toUpperCase()`把一个字符串全部变为大写：

```JavaScript
var s = 'Hello';
s.toUpperCase(); // 返回'HELLO'
```

`toLowerCase()`把一个字符串全部变为小写：

```JavaScript
var s = 'Hello';
var lower = s.toLowerCase(); // 返回'hello'并赋值给变量lower
lower; // 'hello'
```

`indexOf()`会搜索指定字符串出现的位置：

```javascript
var s = 'hello, world';
s.indexOf('world'); // 返回7
s.indexOf('World'); // 没有找到指定的子串，返回-1
```

`substring()`返回指定索引区间的子串：

```javascript
var s = 'hello, world'
s.substring(0, 5); // 从索引0开始到5（不包括5），返回'hello'
s.substring(7); // 从索引7开始到结束，返回'world'
```

###布尔值和运算符

布尔值和布尔代数的表示完全一致，一个布尔值只有`true`、`false`两种值。

与运算符`&&`，`||`，`!`搭配，经常用在条件判断中 。

要特别注意相等运算符`==`。JavaScript在设计时，有两种比较运算符：

第一种是`==`比较，它会自动转换数据类型再比较，很多时候，会得到非常诡异的结果；

第二种是`===`比较，它不会自动转换数据类型，如果数据类型不一致，返回`false`，如果一致，再比较。

由于JavaScript这个设计缺陷，**不要使用`==`比较，始终坚持使用`===`比较**。

另一个例外是`NaN`这个特殊的Number与所有其他值都不相等，包括它自己。唯一能判断`NaN`的方法是通过`isNaN()`函数：

```javascript
NaN === NaN; // false
isNaN(NaN); // true
```

最后要注意浮点数的相等比较：

```javascript
1 / 3 === (1 - 2 / 3); // false
Math.abs(1 / 3 - (1 - 2 / 3)) < 0.0000001; // true
```

这不是JavaScript的设计缺陷。浮点数在运算过程中会产生误差，因为计算机无法精确表示无限循环小数。要比较两个浮点数是否相等，只能计算它们之差的绝对值，看是否小于某个阈值。

###null和undefined

`null`表示一个“空”的值，它和`0`以及空字符串`''`不同，`0`是一个数值，`''`表示长度为0的字符串，而`null`表示“空”。

JavaScript的设计者希望用`null`表示一个空的值，而`undefined`表示值未定义。事实证明，这并没有什么卵用，区分两者的意义不大。**大多数情况下，我们都应该用`null`。`undefined`仅仅在判断函数参数是否传递的情况下有用。**

### 数组

数组是一组按顺序排列的集合，集合的每个值称为元素。**JavaScript的数组可以包括任意数据类型**。例如：

```javascript
new Array(1, 2, 3); // 创建了数组[1, 2, 3]
var arr = [1, 2, 3.14, 'Hello', null, true];
arr[0]; // 返回索引为0的元素，即1
arr[5]; // 返回索引为5的元素，即true
arr[6]; // 索引超出了范围，返回undefined
```

上述数组包含6个元素。数组用`[]`表示，元素之间用`,`分隔。

另一种创建数组的方法是通过`Array()`函数实现。

然而，**出于代码的可读性考虑，强烈建议直接使用`[]`**。

数组的元素可以通过索引来访问。

### 对象（J类似于字典）

JavaScript的对象是一组由键-值组成的无序集合，例如：

```javascript
var person = {
    name: 'Bob',
    age: 20,
    tags: ['js', 'web', 'mobile'],
    city: 'Beijing',
    hasCar: true,
    zipcode: null
};
```

JavaScript对象的键都是字符串类型，值可以是任意数据类型。上述`person`对象一共定义了6个键值对，其中每个键又称为对象的属性，例如，`person`的`name`属性为`'Bob'`，`zipcode`属性为`null`。

要获取一个对象的属性，我们用`对象变量.属性名`的方式：

```javascript
person.name; // 'Bob'
person.zipcode; // null
```

 ### 变量

申明一个变量用`var`语句。

打印使用`console.log(x) `。

使用`console.log()`代替`alert()`的好处是可以避免弹出烦人的对话框。

 ```javascript
var x = 100;
console.log(x);
 ```

### strict模式

不用`var`申明的变量会被视为全局变量，为了避免这一缺陷，所有的JavaScript代码都应该使用strict模式。

启用strict模式的方法是在JavaScript代码的第一行写上：

```javascript
'use strict';
```

 

 


####const常量

const 用于声明一个或多个常量，声明时必须进行初始化，且初始化后值不可再修改：  

```javascript
const Router = require('@koa/router');
```



##Reference

- [JavaScript教程](https://www.liaoxuefeng.com/wiki/1022910821149312 )