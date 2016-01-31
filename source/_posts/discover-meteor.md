---
title: Meteor学习笔记
date: 2015-10-25 20:13:23
tag: [meteor]
---

### 创建项目
meteor create microscope

### 四个子文件夹：

- /client
- /server
- /public
- /lib

**说明**

- 在 /server 文件夹中的代码只会在服务器端运行。
- 在 /client 文件夹中的代码只会在客户端运行。
- 其它代码则将同时运行于服务器端和客户端上。
- 请将所有的静态文件（字体，图片等）放置在 /public 文件夹中
- 在 /lib 文件夹中的文件将被优先载入。
- 所有以 main.* 命名的文件将在其他文件载入后载入。
- 其他文件以文件名的字母顺序载入。

**文件名称和路径不重要**

Meteor 的强大之处在于文件的查找。无论你把代码文件放在 /client 目录下的任何地方，Meteor 都可以找到它并且正确地进行编译。这意味着你永远都不需要手动编写 JavaScript 或 CSS 文件的调用路径。

### 模板系统 Spacebars

- Inclusion
- Expression
- Block Helper

**代码所在的目录既不是 client/ 也不是 server/ 所以 Posts 会共同存在运行在服务器和客户端**

### 存储数据

- 浏览器内存：像 JavaScript 变量的这些数据会保存在浏览器内存中，意味着他们不是永久性的：它们存在于当前浏览器标签中，当标签关闭后它们会消失。
- 浏览器存储：浏览器也可存储较为永久性的数据，使用 cookies 或本地存储 Local Storage。虽然数据会在不同 session 间保持，但是只是针对于当前用户（包括标签之间）但不能轻易地共享给其他用户。
- 服务器端数据库：你想永久保存数据并且提供给多个用户的最好方法是数据库（MongoDB 是 Meteor 应用默认的方案）。

### console，console 与 console

**第1个：**

由操作系统启动
服务器端 console.log() 会输出到这里
有 $ 提示符
通常也被成为外壳程序 Shell，Bash

**第2种：**
在浏览器内启动，执行 Javascript 代码
客户端的 console.log() 会输出到这里
提示符是 ❯
也通常被称作 Javascript 控制台或者开发工具控制台（DevTools Console）

**第3种：**
从终端由 meteor mongo 或者 mrt mongo 来启动
你可以在这里直接操作 App 的数据库
提示符 >
也被称作 Mongo 控制台 (Mongo Console)
客户端集合

客户端的集合更加有趣。当你在客户端申明 Posts = new Mongo.Collection('posts'); 你实际上是创建了一个本地的，在浏览器缓存中的真实的 Mongo 集合。 当我们说客户端集合被"缓存"是指它保存了你数据的一个子集，而且对这些数据提供了十分快速的访问。

有一点我们必须要明白，因为这是 Meteor 工作的一个基础: 通常说来，客户端的集合的数据是你 Mongo 数据库的所有数据的一个子集（毕竟我们不会想把整个数据库的数据全传到客户端来）。

第二，那些数据是被存储在浏览器内存中的，也就是说访问这些数据几乎不需要时间，不像去服务器访问 Posts.find()那样需要等待，因为数据事实上已经载入了。



### 介绍 MiniMongo

Meteor 的客户端 Mongo 的技术实现被成为 MiniMongo。它目前还不是一个完美的实现，而且你会发现偶尔 Mongo 的功能在这里不能实现。不过本书中涉及到的功能都是可以在 Mongo 和 MiniMongo 中实现的。


meteor reset:清空数据库初始化


**查找与提取**

在 Meteor 中，find() 返回值是一个游标。游标是一种从动数据源。如果你想输出内容，你可以对游标使用 fetch()来把游标转换成数组。

Meteor 十分智能地在应用中保持游标状态而避免动不动就把游标变成数组。这就造成了你不会经常在 Meteor 代码中看到 fetch() 被调用（基于同样原因，我们在上述例子中也没有使用 fetch ）。


### autopublish

- autopublish:让我们的客户端可以镜像般地得到数据库中的所有帖子
- meteor remove autopublish


meteor add iron:router


### Loading模板
meteor add sacha:spin
```
<template name="loading"> {{>spinner}} </template>
```

### dataNotFound hook
Router.onBeforeAction('dataNotFound', {only: 'postPage'});
这会告诉 Iron Router 不仅在非法路由情况下，而且在 postPage 路由，每当 data 函数返回“falsy”（比如null、false、undefined 或 空）对象时，显示“无法找到”的页面

动态代码重载技术

手动去重载浏览器窗口，自然就会丢失我们的 Session 变量（因为这将会创建一个新的会话）
引发动态代码重载（即，通过修改并保存我们的源文件）去重新加载页面，Session 变量却仍然存在

### Package
- meteor add twbs:bootstrap
- meteor add underscore

### autorun

- 每一次 autorun 上下文中的响应式数据源发生变化的时候，autorun 函数就会自动运行
- Tracker.autorun( function() { console.log('Value is: ' + Session.get('pageTitle')); } );

**发布时删除辅助包**

meteor remove insecure


### 发布与订阅

```
// 在服务器端
Meteor.publish('posts', function(author) {
  return Posts.find({flagged: false, author: author});
});
```

然后我们在客户端订阅这个发布时定义同一个参数。

// 在客户端
Meteor.subscribe('posts', 'bob-smith');



**让meteor在后台运行**
screen meteor