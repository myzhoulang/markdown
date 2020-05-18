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

```javascript
// 返回的文档中只包含 `name` 、`email`和`_id`。
db.users.find({ name: "A" }, { "name": 1, "email": 1})
```

```javascript
// 返回文档中除 `name`字段外所有的字段。
db.users.find({ name: "A"}, {"name": 0})
```

```javascript
// 下面的查询出错**`Error`**
db.users.find({ name: "A"}, { "name": 1, "email": 0})
```

## 查询条件

* 查询条件

> `$lt`、`$gt`、`$lte`、`$gte`是比较操作符，可以单独使用，也可以组合使用查询一个范围的值。可以比较`数字型`、`时间型`的值。

```javascript
// 查询 18~30岁（含）的用户
db.users.find({age: {"$gte": 18, "$lte": 30}})
```

```javascript
// 查找2007年1月1日之前注册的用户。
db.users.find({registered: { "$lt": new Date("01/01/2007")}})
```



> `$ne`是不等操作符。当查询条件需要正则去匹配的时候，使用`$ne`会出错。正则类型的可以使用 `$not`操作符。

```javascript
// 查找 `name!=A` 的用户。
db.users.find({name: {"$ne": "A"}})
```

```javascript
// 下面查询`name`字段不以`A`开头的用户，报错了。
// Error `Can't have regex as arg to $ne.`
db.users.find({name: {"$ne": /^A/}}) 
```



* OR 查询

> 有两种方式进行 OR 查询。一种是使用`$in`用来查询一个键的多个值，一种是使用`$or` 可以在多个键中查询任意给定的值。如果需要对`$in`的匹配条件后取反可以使用`$nin`。

```javascript
// 查询 `status`字段的值是 `1`|`2`|`3`的用户 
db.users.find({status: {"$in": [1,2,3]}})
```

```javascript
// 查询`status`字段的不是  `1`|`2`|`3`的用户 
db.users.find({status: {"$nin": [1,2,3]}})
```

```javascript
// 查询 `status`字段是`1` 或者 `age`字段大于`18`的用户
db.users.find({$or: [{status: 1}, {age: {$gt: 18}}]})
```



* $not

> `$not`操作符是匹配那些不符合条件的文档。相对`$ne` ，`$not`可以结合正则表达式一起使用。

```javascript
// 查询`name`字段不是 A 的文档
db.users.find({name: {$not: "A"}})

// 查询`age`字段不小于1的文档
db.users.find({age: {$not: {$lt: 1}}})

// 结合正则表达式查询
// 查询 `name`字段不以字符串 a 开头的
db.users.find({name: { $not: /^a/i}})
```



* 条件语句

> 查询操作符一般是内层文档的键。更新操作符则是外层文档的键。

## 特定类型查询

* null

> `null`类型可以匹配自身，还会匹配那些没有这个键的文档。如果要排除掉没有这个键的文档 需要结合`$exists`这个操作符。

```javascript
// 下面这样会查询出 字段 x 为 null 和那些没有 x 字段的文档
db.users.find({x: null})

// 下面只会查询出 字段 x 为 null
db.users.find({x: {$in: [null], $exists: true}})
```



* 正则表达式

> 正则表达式可以灵活有效的匹配字符串。像对一个字段的模糊查询，忽略大小写等待非常有用。正则表达式也可以匹配到自身，如果字段中的值就是一个正则表达式，也能正确匹配。

```javascript
// 查询字段 name 中包含 (a|A) 的文档
db.users.find({ name: /a/i })

// 查询字段 name 中包含 (a|A) 开头的文档
db.users.find({ name: /^a/i})
```



* 查询数组

  1. 单个元素匹配数组

     ```javascript
     // 假设 fruit 的值一个数组
     // 下面的匹配会匹配到  fruit 数组中包含 apple 一项的文档
     db.foods.find({fruit: "apple"})
     ```

  2. 多个元素匹配数组

     ```javascript
     // 假设 fruit 的值一个数组
     // 下面的匹配会匹配到  fruit 数组中包含 apple 和 banana 的文档
     db.foods.find({fruit:{$all: ["apple", "banana"]} })
     ```

  3. 数组精确匹配

     > 数组的精确匹配必须 **查询条件** 和 **文档中的值个数和顺序**都必须保持一致。要查询数组中特定位置的元素，可加上数组下标进行查询。

     ```javascript
     // 假设 fruit 的值一个数组
     // 下面只会匹配字段 fruit 为 ["apple", "banana"]的文档
     db.foods.find({fruit:["apple", "banana"] })
     
     // 匹配数组中第一个元素的值
     // 下面会匹配到 fruit 字段中第一个元素的值是 apple 的文档。
     db.foods.find({"fruit.0": "apple"})
     ```

     

  4. `$size`

     > `$size` 对查询字段数组长度很有效。像查询某个字段的数组值只有指定的个数的文档。
     >
     > **注意**： `$size` 不能与其他查询条件组合使用。

     ```javascript
     // 假设 fruit 的值一个数组
     // 查询字段 fruit 的个数为 2 个的文档
     db.foods.find({"fruit": {$size: 2}})
     ```

     

  5. `$slice`

     > `$slice`操作符可以返回某个键匹配数组元素的一个子集。放在 `find`的第二个参数中。这个操作符不会影响文档其他键的返回。如果指定返回的键，需要显示声明。

     ```javascript
     // 返回某一个博客的前 10 条评论
     db.find({_id: ObjectId(x)}, {comment: {$slice: 10}})
     
     // 返回某一个博客的最后 10 条评论
     db.find({_id: ObjectId(x)}, {comment: {$slice: -10}})
     
     // 返回某一个博客的中评论
     // 跳过前 10 条评论后，取10条
     db.find({_id: ObjectId(x)}, {comment: {$slice: [10, 10]}})
     
     ```

     

  6. `$elemMatch`

     > 对数组中每一个元素和传入的所有查询条件进行比较。

     ```javascript
     // 假设 x 的值是一个数组
     // 会对字段 x 进行循环，依次和 $elemMatch 中的条件进行匹配
     // 只有元素 大于10 并且小于20 才会被命中
     db.tests.find({x: {$elemMatch: {$gt: 10, $lt: 20}}})
     ```

     

* 查询内嵌文档

> 查询内嵌文档有两种方式： 1. 查询整个文档， 2.针对 key/value 进行查询。

 1. 精确操作 和 数组的精确查找一样。 内嵌文档的 字段个数、字段顺序、字段名称都需要一致。

 2. key/value 查找

    > Key/value 查找使用点表示法。当两个查询条件使用了点表示法，他们的关系是 `or` 的关系。如果需要对两个条件使用 `and`关系，可以结合 `$elemMatch`操作符使用。

    ```javascript
    // 假设需要查询 有 Joe 发表的 5 分以上的评论
    
    // 下面的查询是可能失败的，如果内嵌文档只有这两个字段的时候是OK，
    // 如果内嵌文档中有额外的字段，就会失败。
    // 下面的查询是精确匹配
    db.blogs.find({"comments": {"author": "joe", "score": {"$gte": 5}}})
    
    // 下面的查询会失败
    // 下面的查询会匹配到 评论中 author=joe 或者 score >= 5 的文档。
    db.blogs.find({"comments.author": "joe", "comments.score": {"$gte": 5}})
    
    // 可以使用 $elemMatch
    db.blogs.find({"comments": { $elemMatch: { author: "joe", score: {"$gte": 5 } }}})
    ```

## $where 查询

> `$where`可以在查询中指定任意的 `JavaScript`。为了安全和性能考虑，应尽可能避免使用。

```javascript
db.foo.find({"$where": function(){
  // 查询逻辑
  // 返回 true 文档就作为结果集的一部分返回，返回false 就不返回
}})
```

## 游标

* `limit`、`skip`、`sort`

> `limit`在`find`后使用来限制结果数量，`skip`和`limit`结合用于分页查询，指定跳过指定条数。`sort`用于排序，1 为升序，-1位降序。

```javascript
// 下面的查询跳过前10条，选择10,以 age 升序排序。
db.users.find().skip(10).limit(10).sort({ age: 1})
```

> - [ ]  `skip`性能问题

## 参考

[mongodb 官方文档](https://docs.mongodb.com/)

[MongoDB 权威指南第二版书籍](https://item.jd.com/11384782.html)