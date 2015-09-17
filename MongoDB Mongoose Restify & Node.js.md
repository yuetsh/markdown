# MongoDB, Mongoose, Restify & Node.js学习笔记
## MongoDB
mongodb的
### Mongoose

## Restify
### REST
Representational State Transfer表述性状态转移 是一种针对网络应用的设计和开发方式，可以降低开得复杂性，提高系统的可伸缩性。

REST中的资源所指的不是数据，而是数据和表现形式，比如“最活跃的10位会员”和“最新的10位会员” 在数据上可能是重叠的或者是完全一致的，但是由于他们的表现形式不同，所以被归类为不同的资源。也就是REST的名字是Representational State Transfer的原因。

### restify
restify是一个基于Nodejs的REST应用框架，支持服务器端和客户端。restify比起express更专注于REST服务，去掉了express中的template, render等功能，同时强化了REST协议使用，版本化支持，HTTP的异常处理。

## Express
### Request
- req.params
ç
这是一个数组对象，命名过的参数会以键值对的形式存放。 比如你有一个路由`/user/:name`, "name"属性会存放在`req.params.name`. 这个对象默认为`{}`.

```javascript
// GET /user/xuyue
req.params.name
// => "xuyue"çç
```

- req.query

这是一个解析过的请求参数对象，默认为`{}`.

```javascript
// GET /search?q=tobi+ferret
req.query.q
// => "tobi ferret"

// GET /shoes?order=desc&shoe[color]=blue&shoe[type]=converse
req.query.order
// => "desc"

req.query.shoe.color
// => "blue"

req.query.shoe.type
// => "converse"
```
- req.body

