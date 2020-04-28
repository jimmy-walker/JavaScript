## KOA

Koa 就是一种简单好用的 Web 框架。**J简而言之，就是将网页放到网上变成url可以访问**。

只要三行代码，就可以用 Koa 架设一个 HTTP 服务。 

```javascript
const Koa = require('koa');
const app = new Koa();

app.listen(3000);
```




## 路由

在web开发中，“route”是指根据url分配到对应的处理程序。  

传统web开发是每一个请求地址都会请求服务器来进行处理，但是用户有些操作则无需请求服务器，直接页面端修改下逻辑就能达到目的，这种最好使用路由，也许题主会有疑问：直接使用js处理下不就行了。使用js直接处理这些是可以的，事实上以前我们也这么做，但是这样做不便于用户收藏当前页，因为使用js时并不更新url，但是使用路由时，url也是随着改变的，用户浏览到一个网页时可以直接复制或收藏当前页的url给别人，这种方式对于搜索引擎和用户来说都是友好的。

```javascript
const Koa = require('koa');
const app = new Koa();
 
const route = (ctx, next) => {
  console.log(ctx.path);
  switch (ctx.path) {
    case '/':
      ctx.body = '<h1>欢迎光临index page 页面</h1>';
      break;
    case '/home':
      ctx.body = '<h1>欢迎光临home页面</h1>';
      break;
    default:
      // 404页面
      return;
  }
}
 
app.use(route);
 
app.listen(3001, () => {
  console.log('3001 server start....');
});
```

这种方式不是很好，当我们项目变得很大的时候，我们需要编写很多 switch-case 这样的语句，代码变得更加耦合，并且当我需要对某个路由要加一个中间件过滤下的时候，这种方式并不好处理。并且当项目非常大的时候，我们不想把所有的路由编写的一个app.js 页面的时候，我们需要写到routes文件夹下多个js里面去，也就是说对路由进行分层级的时候，这样做的目的就是想让以后项目路由管理更加方便。那么目前的app.js中的switch-case 这种方式不支持了，因此我们这个时候就需要koa-router这样的中间件来做这件事情了。

```javascript
const Koa = require('koa');
const app = new Koa();
 
const router = require('koa-router')();
 
// 添加路由
router.get('/', ctx => {
  ctx.body = '<h1>欢迎光临index page 页面</h1>';
});
 
router.get('/home', ctx => {
  ctx.body = '<h1>欢迎光临home页面</h1>';
});
 
router.get('/404', ctx => {
  ctx.body = '<h1>404...</h1>'
});
 
// 加载路由中间件
app.use(router.routes());
 
app.listen(3001, () => {
  console.log('server is running at http://localhost:3001');
});
```



## Reference

- [理解koa-router 路由一般使用](https://www.cnblogs.com/tugenhua0707/p/10686634.html)