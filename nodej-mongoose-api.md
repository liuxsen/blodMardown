---
title: nodej__mongoose__api
date: 2016-11-18 15:45:08
tags:
---


# Node+Mongoose常用查询中文文档

> Mongoose 模型提供了 find, findOne, 和 findById 方法用于文档查询。

## Model.find

```js
Model.find(query, fields, options, callback)
// fields 和 options 都是可选参数
```

## 简单查询

```js
Model.find({ 'csser.com': 5 }, function (err, docs) { 
// docs 是查询的结果数组
 });
```

## 只查询指定键的结果
```js

Model.find({}, ['first', 'last'], function (err, docs) {
  // docs 此时只包含文档的部分键值
})
```

## Model.findOne
```js
//与 Model.find 相同，但只返回单个文档

Model.findOne({ age: 5}, function (err, doc){
  // doc 是单个文档
});
```

## Model.findById
> 与 findOne 相同，但它接收文档的 _id 作为参数，返回单个文档。_id 可以是字符串或 ObjectId 对象。

```js
Model.findById(obj._id, function (err, doc){
  // doc 是单个文档
});
```

## Model.count
> 返回符合条件的文档数。

```js
Model.count(conditions, callback);
```

## Model.remove
> 删除符合条件的文档。

```js
Model.remove(conditions, callback);
```

## Model.distinct
> 查询符合条件的文档并返回根据键分组的结果。

```js
Model.distinct(field, conditions, callback);
```

## Model.where
> 当查询比较复杂时，用 where：

```js
Model
.where('age').gte(25)
.where('tags').in(['movie', 'music', 'art'])
.select('name', 'age', 'tags')
.skip(20)
.limit(10)
.asc('age')
.slaveOk()
.hint({ age: 1, name: 1 })
.run(callback);
```

## Model.$where

>有时我们需要在 mongodb 中使用 javascript 表达式进行查询，这时可以用 find({$where : javascript}) 方式，$where 是一种快捷方式，并支持链式调用查询。

```js
Model.$where('this.firstname === this.lastname').exec(callback)
```

## Model.update

> 使用 update 子句更新符合指定条件的文档，更新数据在发送到数据库服务器之前会改变模型的类型。

```js
var conditions = { name: 'borne' }
  , update = { $inc: { visits: 1 }}
  , options = { multi: true };

Model.update(conditions, update, options, callback)
//*注意*：为了向后兼容，所有顶级更新键如果不是原子操作命名的，会统一被按 $set 操作处理，例如：

var query = { name: 'borne' };
Model.update(query, { name: 'jason borne' }, options, callback)

// 会被这样发送到数据库服务器

Model.update(query, { $set: { name: 'jason borne' }}, options, callback)
```

## 查询 API

> 如果不提供回调函数，所有这些方法都返回 Query 对象，它们都可以被再次修改（比如增加选项、键等），直到调用 exec 方法。

```js
var query = Model.find({});

query.where('field', 5);
query.limit(5);
query.skip(100);

query.exec(function (err, docs) {
  // called when the `query.complete` or `query.error` are called
  // internally
});
```

[完]