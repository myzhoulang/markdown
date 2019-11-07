<h1 align="center">mongodb 查询</h1>
## find

* find 简介

> Mongodb 使用`find`进行查询。`find`有两个参数，第一个参数是指定查询条件。默认是 `{}`。

```javascript
// 不指定任何条件，查询所有
db.collections.find()
```



* 指定返回的值

> `find`的第二个参数是指定需要返回哪些字段，这样避免将所有的字段返回，可以节省传输的数据量，也可以节省客户端解码文档的时间和内存消耗。指定字段的值 1 为包含， 0为排除。
>
> **默认情况下 `_id`是总是会返回的，只有当设置为 0才不会返回**
>
> **指定的字段中不能同时又包含和排除**
>
> 格式：`db.collection.find(<查询条件>, <需要返回的字段>)`

返回的文档中只包含 `name` 、`email`和`_id`。

```javascript
db.users.find({ name: "A" }, { "name": 1, "email": 1})
```

返回文档中除 `name`字段外所有的字段。

```javascript
db.users.find({ name: "A"}, {"name": 0})
```

下面的查询=出错**`Error`**

```javascript
db.users.find({ name: "A"}, { "name": 1, "email": 0})
```

## 查询条件

* 查询条件

> `$lt`、`$gt`、`$lte`、`$gte`是比较操作符，可以单独使用，也可以组合使用查询一个范围的值。可以比较`数字型`、`时间型`的值。

查询 18~30岁（含）的用户

```javascript
db.users.find({age: {"$gte": 18, "$lte": 30}})
```

查找2007年1月1日之前注册的用户。

```javascript
db.users.find({registered: { "$lt": new Date("01/01/2007")}})
```



> `$ne`是不等操作符。当查询条件需要正则去匹配的时候，使用`$ne`会出错。正则类型的可以使用 `$not`操作符。

查找 `name!=A` 的用户。

```javascript
db.users.find({name: {"$ne": "A"}})
```



下面查询`name`字段不以`A`开头的用户，报错了。

```javascript
// Error `Can't have regex as arg to $ne.`
db.users.find({name: {"$ne": /^A/}}) 
```



* OR 查询

> 

* $not
* 条件语句

## 特定类型查询

## $where 查询

## 游标

## 数据库命令

