---
title: vue-koa-demo入门练手总结
date: 2018-07-11 16:39:42
categories: 总结
tags: 
  - nodejs 
  - vue 
  - webpack
---

之前看了一些vue和nodejs的教程，但是入门过程中有些问题一直很迷：
1.具体的框架怎么构建起来的
2.前端是怎么和node通信的
3.用vue控制前端路由的目录应该怎么建
等...

后来找到[全栈开发实战：用Vue2+Koa1开发完整的前后端项目（更新Koa2）](https://molunerfinn.com/Vue+Koa/)这篇文章，跟着写了一遍。
不得不说，这位大哥真的太懂我这种初学者需要哪些点了，感谢这位大哥的教程。

步骤就不说了，总结一下跟着做的过程遇到的坑吧，有些可能因为版本更新之类的原因,总之也对版本之间的差别有了更多的了解。

主要版本：
```
"koa": "^2.5.1",
"koa-router": "^7.4.0",
```

#### 安装vue-cli
安装过程报错
```
PS E:\front\learn_project> npm install -g vue-cli
npm WARN deprecated coffee-script@1.12.7: CoffeeScript on NPM has moved to "coffeescript" (no hyphen)
npm ERR! Unexpected end of JSON input while parsing near '...027241519e787343df782'

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\hetutu\AppData\Roaming\npm-cache\_logs\2018-07-04T11_20_18_301Z-debug.log
```
解决方法：
```
npm cache clean --force
```

#### 使用sequelize-auto
在使用sequelize-auto自动生成model的时候，报错
```
npm install --save sequelize mysql
npm install -g sequelize-auto
```
执行
```
sequelize-auto -o "./schema" -d todolist -h 127.0.0.1 -u root -p 3306 -e mysql -x ht19931007
```
主要错误信息：
```
name: 'SequelizeConnectionError',
  message:
   'ER_NOT_SUPPORTED_AUTH_MODE: Client does not support authentication protocol requested by server; consider upgrading MySQL client',
```
解决方法：
```
mysql命令行中执行：
use mysql;
alter USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'ht19931007';
flush privileges;
```

#### async/await vs generator
原文作者用的都是generator函数（中间件和路由），百度到的koa-router也都是可以用generator的，但路由怎么都挂载不上。
看了koa2的文档：
<img src="/images/vue-koa-demo1.png">

所以需要把原文里的的generator用co处理一下，或者用async/await来写。

#### this or ctx
原文的中间件获取上下文都是用的this,比如：
```
const getUserInfo = function* (){
  const id = this.params.id; 
  const result = yield user.getUserById(id);  
  this.body = result 
}
```
但是我的版本里this获取不到数据，需要用ctx
```
const getUserInfo = async function (ctx){
  const id = ctx.params.id; 
  const result = await user.getUserById(id);  
  ctx.body = result 
}
```

#### axios.delete
删除todolist我用的是axios.delete(),比如:
```
axios.delete('/api',{id:1})
```
但是后台就是接收不到数据，原来delete第二个参数是 config ，所以要通过 config 里面的 data 来传参：
```
axios.delete('/api',{data:{id:1}})
```

#### koa-static & koa-router
koa-static会拦截请求路径进行处理 如果不是静态文件将会返回http404，两个中间件同时使用时应该先执行router中间件避免请求被static拦截处理。

### 写在最后
入门教程写完，起码明白了之前实习的时候一直很迷的地方：一个目录下的vue和koa是怎么协作的。
前端由vue实现单页面，用vue-cli构建的项目本身已经有web-dev-server作为开发服务器了，而使用koa也是实现一个服务器的功能，处理api请求，最后生产环境可以挂载前端build之后的静态资源，把Koa作为一个Vue项目的静态资源服务器,请求就都是同域了。