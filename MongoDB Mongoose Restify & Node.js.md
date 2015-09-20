# MongoDB, Mongoose, Restify & Node.js学习笔记
## MongoDB
MongoDB是一个跨平台的，面向文档的数据库，具有高性能、高可用性和可扩展性的优点。MongoDB是工作在集合和文档的概念上的：

- 数据库


- 集合

![collections](http://docs.mongoing.com/manual-zh/_images/crud-annotated-collection.png)

- 文档

![documents](http://docs.mongoing.com/manual-zh/_images/crud-annotated-document.png)


### MongoDB的CRUD操作
MongoDB以**文档**的形式存数数据，文档类似与JSON的字段和值对。文档与编程语言中把键值对关联起来的结构很类似，比如dictionaries、 hashes、 maps以及 associative arrays。正式的来说，MongoDB文档是`term:BSON`文档。BSON是有额外类别信息的`term:JSON` 的二进制表现形式。在文档里，字段的值可以是BSON数据类型中的任意一种，包括其他的文档、数组以及文档数组。

#### 数据库操作
##### 查询
在MongoDB中，查询以一个特定的文档集合作为查询目标。查询指定一些条件，这些条件确定MongoDB返回到客户端的文档。查询可以包含一个**映射**，它指定返回的匹配文档的字段。可以使用limits、skips以及sort 命令来有选择的修饰查询。
##### 数据修改
数据修改是指创建、更新或者删除数据操作。在MongoDB里，这些操作修改单个**collection**中的数据。对于更新或者删除操作，你可以为要选择的文档指定条件，然后进行更新或者删除。
![insert](http://docs.mongoing.com/manual-zh/_images/crud-insert-stages.png)
⬆️插入一个新的document到名叫user的collection中
#### 相关名词
##### 索引
为了提高常用的查询和更新操作的性能，MongoDB对辅助索引提供了完全支持。这些索引允许应用使用一个高效的数据结构存储一部分集合的 视图 。大部分索引存储一个或一组字段的所有值的有序表现形式。索引也可以 强制唯一 ，以 地理空间表现形式 存储对象，并且简化 文本搜索。

##### 复制集读选项
对于有复制集组件的复制集和分片索引，应用指定 复制集读选项 。复制集读选项决定客户端从哪个复制集成员上进行读操作的策略。

##### 安全写级别
应用也可以使用 安全写级别 来控制写操作的行为。特别是在复制集部署场景下，客户端程序可以通过安全写级别来指定MongoDB如何确认写操作成功。

##### 聚合
除基本的查询外，MongoDB还提供了几个数据聚合特性。例如MongoDB可以返回匹配一个查询的文档的数量，或者返回一个字段非重复值的数量，或者用一种灵活的多阶段数据处理管道或Map-Reduce来对一个集合的文档进行处理。

#### 读写操作
##### 读操作
##### 写操作（插入 更新 删除）
## Mongoose
Mongoose是基于node-mongodb-native开发的MongoDB nodejs驱动，可以在异步的环境下执行。
### 名词
- `Schema`

一种以文件形式存储的数据库模型骨架，不具备数据库的操作能力。

- `Model`

由`Schema`发布生成的模型，具有抽象属性和行为的数据库操作对。

- `Entity`

由`Model`创建的实体，他的操作也会影响数据库。

- 三者关系

`Schema`生成`Model`，`Model`创造`Entity`，`Model`和`Entity`都可对数据库操作造成影响，但`Model`比`Entity`更具操作性。

### 具体看
- 什么是Schema

我理解Schema仅仅只是一断代码，他书写完成后程序依然无法使用，更无法通往数据库端。
他仅仅只是数据库模型在程序片段中的一种表现，或者是数据属性模型。

- 如何定义Schema

```javascript
var mongooseSchema = new mongoose.Schema({
    username : {type : String, default : '匿名用户'},
    title    : {type : String},
    content  : {type : String},
    time     : {type : Date, default: Date.now},
    age      : {type : Number}
});
```
## Restify
### REST
Representational St4ate Transfer表述性状态转移 是一种针对网络应用的设计和开发方式，可以降低开得复杂性，提高系统的可伸缩性。

REST中的资源所指的不是数据，而是数据和表现形式，比如“最活跃的10位会员”和“最新的10位会员” 在数据上可能是重叠的或者是完全一致的，但是由于他们的表现形式不同，所以被归类为不同的资源。也就是REST的名字是Representational State Transfer的原因。

### restify
restify是一个基于Nodejs的REST应用框架，支持服务器端和客户端。restify比起express更专注于REST服务，去掉了express中的template, render等功能，同时强化了REST协议使用，版本化支持，HTTP的异常处理。

## Express
### Request
- req.params

这是一个数组对象，命名过的参数会以键值对的形式存放。 比如你有一个路由`/user/:name`, "name"属性会存放在`req.params.name`. 这个对象默认为`{}`.

```javascript
// GET /user/xuyue
req.params.name
// => "xuyue"
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

这个对应的是解析过的请求体。这个特性是`bodyParser()` 中间件提供,其它的请求体解析中间件可以放在这个中间件之后。当`bodyParser()`中间件使用后，这个对象默认为 `{}`。

```javascript
// POST user[name]=tobi&user[email]=tobi@learnboost.com
req.body.user.name
// => "tobi"

req.body.user.email
// => "tobi@learnboost.com"

// POST { "name": "tobi" }
req.body.name
// => "tobi"
```
- req.param(name)

返回`name`参数的值。

```javascript
// ?name=tobi
req.param('name')
// => "tobi"

// POST name=tobi
req.param('name')
// => "tobi"

// /user/tobi for /user/:name 
req.param('name')
// => "tobi"
```

查找的优先级如下:

- req.params
- req.body
- req.query

直接访问 `req.body`, `req.params`, 和 `req.query` 应该更合适，除非你真的需要从这几个对象里同时接受输入。