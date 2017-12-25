---
layout: post
title: "Intro Hapi.js "
description: "hapi.js入门介绍"
category: front
tags: [js]
---
{% include JB/setup %}

Hapi.js是Node生态里的一款优秀服务器端框架，类似于Express.js。

## Install hapi

- 创建一个工程目录`hapi`,进入该目录；
- 在终端运行 `npm init`,默认即可,会在当前目录生成*package.json*；
- 运行`npm intall --save hapi`,它会自动安装*hapi*，并修改*package.json*，加入依赖

完成上述步骤，就可以开始使用！

## 创建服务器

基本的服务器代码像下面：

```js
'use strict';

const Hapi = require('hapi'); (1)

const server = new Hapi.Server(); (2)

server.connection({port:3000,host:'localhost'}); (3)
server.start((err) =>{   (4)
    if(err){
        throw err;
    }
    console.log(`Server running at: ${server.info.url}`);
});

```
1. 导入hapi
2. 创建一个hapi 服务器对象
3. 对服务器添加一个连接，并设置监听端口
4. 启动服务器

## 添加路由

现在我们需要给服务器添加一些路由，让它做一些实际的工作，看起来是这样的：

```js
'use strict';

const Hapi = require('hapi');

const server = new Hapi.Server();
server.connection({ port: 3000, host: 'localhost' });

server.route({
    method: 'GET',
    path: '/',
    handler: function (request, reply) {
        reply('Hello, world!');
    }
});

server.route({
    method: 'GET',
    path: '/{name}',
    handler: function (request, reply) {
        reply('Hello, ' + encodeURIComponent(request.params.name) + '!');
    }
});

server.start((err) => {

    if (err) {
        throw err;
    }
    console.log(`Server running at: ${server.info.uri}`);
});
```
将上述代码保存到`server.js`，然后启动`node server.js`，现在可以在浏览器输入[http://localhost:3000](http://localhost:3000),浏览器将会出现 *Hello, World!* ，访问[http://localhost:3000/stimpy](http://localhost:3000/stimpy)将返回*Hello, stimpy*。

注意上面对用户的*name*参数进行了编码，这是为了防止内容注入攻击，

## 创建静态页面和内容

我们可以通过使用*insert*插件来支持静态页面。

首先停止原先程序，在终端运行 `npm install --save insert`,它会安装插件到本地。

添加下面的代码到*server.js* ：

```js
server.register(require('inert'), (err) => {

    if (err) {
        throw err;
    }

    server.route({
        method: 'GET',
        path: '/hello',
        handler: function (request, reply) {
            reply.file('./public/hello.html');
        }
    });
});

```

`server.register`命令将*insert*插件注册到Hapi应用当中。

[参考](https://hapijs.com/tutorials)