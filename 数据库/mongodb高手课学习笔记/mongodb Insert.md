# Mongodb insert

* `insertOne`

  > 插入一条数据
  >
  > 格式：
  >
  > db.<集合>.insertOne(<JSON对象>)

  ```shell
  $ db.fruit.insertOne({name: 'apple'})
  ```

  

* `insertMany`

  > 批量出入数据
  >
  > 格式：
  >
  > db.<集合>.insertMany([<JSON对象>, <JSON对象>...])

  ```shell
  $ db.fruit.insertMany([{name: 'apple'}, {name:'pear'}])
  ```

  