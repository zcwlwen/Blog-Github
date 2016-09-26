---
title: MongoDB的进阶使用
date: 2016-09-20 14:00:00
tags:
---

#MongoDB的进阶使用


##在终端使用shell命令操作数据库

###数据库操作

创建或者切换到当前数据库:

`use DATABASE_NAME`

使用`use`命令，如果当前数据库存在则是切换到对应的数据库，如果数据库不存在则是创建数据库。

查看数据库：

`show dbs`

删除数据库：

使用`use`命令进入到要删除的数据库中，然后执行删除命令：

`db.dropDatabase()`

就可以删除当前数据库了。

###集合操作（表）
查看当前数据库中包含的集合

`show collections`

创建集合

`db.createCollection(集合名) `

删除集合

`db.COLLECTION_NAME.drop()`

###文档操作
插入文档

`db.COLLECTION_NAME.insert(document)`

查询文档

`db.COLLECTION_NAME.find()`

使用pretty()方法格式化返回

`db.COLLECTION_NAME.find().pretty()`

find()中可以添加条件筛选要查询的内容

更新文档

`db.COLLECTION_NAME.update(SELECTIOIN_CRITERIA, UPDATED_DATA)`

例子：

```php
$collection->update(array("endTime"=>""),array('$set'=>array("endTime"=>time())));
$collection->update(array("isTranscoding"=>0),array('$set'=>array("isTranscoding"=>1)));
```
删除文档

`db.COLLECTION_NAME.remove(DELLETION_CRITTERIA)`

如果remove()括号中不写东西默认删除全部内容

-----

##在php中操作MongoDB数据库

###连接和选择一个数据库

```
<?php
$m = new MongoClient(); // 连接默认主机和端口为：mongodb://localhost:27017
$db = $m->test; // 获取名称为 "test" 的数据库
?>
```

###创建集合

```
<?php
$m = new MongoClient(); // 连接
$db = $m->test; // 获取名称为 "test" 的数据库
$collection = $db->createCollection("testCollection");
echo "集合创建成功";
?>

```

###插入文档

```
<?php
$m = new MongoClient();    // 连接到mongodb
$db = $m->test;            // 选择一个数据库
$collection = $db->testCollection; // 选择集合
$document = array( 
	"title" => "MongoDB", 
	"description" => "database", 
	"likes" => 100,
	"url" => "http://www.runoob.com/mongodb/"
);
$collection->insert($document);
echo "数据插入成功";
?>
```

###查找文档

```
<?php
$m = new MongoClient();    // 连接到mongodb
$db = $m->test;            // 选择一个数据库
$collection = $db-> testCollection; // 选择集合

$cursor = $collection->find();
// 迭代显示文档标题
foreach ($cursor as $document) {
	echo $document["title"] . "\n";
}
?>
```

###更新文档

```
<?php
$m = new MongoClient();    // 连接到mongodb
$db = $m->test;            // 选择一个数据库
$collection = $db-> testCollection; // 选择集合
// 更新文档
$collection->update(array("title"=>"MongoDB"), array('$set'=>array("title"=>"MongoDB 教程")));
// 显示更新后的文档
$cursor = $collection->find();
// 循环显示文档标题
foreach ($cursor as $document) {
	echo $document["title"] . "\n";
}
?>
```

###删除文档

```
<?php
$m = new MongoClient();    // 连接到mongodb
$db = $m->test;            // 选择一个数据库
$collection = $db->testCollection; // 选择集合
   
// 移除文档
$collection->remove(array("title"=>"MongoDB 教程"), array("justOne" => true));

// 显示可用文档数据
$cursor = $collection->find();
foreach ($cursor as $document) {
	echo $document["title"] . "\n";
}
?>
```

