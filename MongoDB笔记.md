MongoDB笔记
==========

[视频](https://www.youtube.com/watch?v=1uFY60CESlM)
## install
brew install mongodb

CLI

启动MongoDB

```
mongod
```
输入命令

```
mongo
```
## webstorm mongodb plug-in

mongodb plug

mongodb path > /usr/local/Cellar/mongodb/3.0.6/bin/mongo

## create database, install data
```sh
use xuyue // 新建或切换到xuyue这个数据库
```
```sh
db // 查看当前数据库，显示：xuyue
```
```sh
xuyue // 这就是数据库的名字
```
```sh
show xuyue
```
```sh
db.friends.insert(
	{
		name: "haha",
		age: "23"
	}
)
```
## 10月21日更新
### 查找并更新某一文档的某一属性
```sh
mongo dev.farseer.com

use liaoyuan

db.User.find({}).pretty()

db.User.find({"email": "yue@alscope.com"}).pretty()

db.User.update({"email": "yue@alscope.com"}, {$set: {"role": 100}})
```