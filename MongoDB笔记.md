# 一.安装

在Windows环境下：
```
1、运行 Win + R
2、输入 services.msc 命令便可以查看到 MongoDB Server (MongoDB) 
3、mongosh表示启动MongoDB数据库
```

```
mongod -version
// 该命令会显示MongoDB数据库的相关信息，如果能显示信息，就表示已安装成功了！！！
```

# 二.MongoDB数据库中的常用述语:
1. 在MongoDB中，数据库是以文件形式存储的，数据库目录中存储了相应的数据库！
2. 在MongoDB中，把传统数据库中的 "表" 叫作：Collections "集合"！
3. 在MongoDB中，向集合存储数据时，直接以JSON格式，进行存取操作！
4. 在MongoDB中，集合中的数据叫作：Documents "文档"！

# 三.创建数据

## 1.查看所有数据库

```bash
show dbs
```
![图 1](images/f0697cde1f413411c859496f776e43ea2a4ce09dc7873847de0767c9362a30c7.png)  

## 2.获取当前数据库名称

```bash
db
```
![图 2](images/4b6031b97c80850574154494c0879bdc49f09d604fa1305b9ae8a8510c03c048.png)  

## 3.创建数据库mydab2,并插入一条doc数据

* document类似于关系型数据库中的行
```bash
test> db
test
test> use mydb2
switched to db mydb2
mydb2> db
mydb2
mydb2> doc={"title":"A","desc":"One"};
{ title: 'A', desc: 'One' }
mydb2> db.some.insertOne(doc);
{
  acknowledged: true,
  insertedId: ObjectId("641a6b4522768edff936b426")
}
```


* collection类似于关系型数据库中的表，字段类似于关系型数据库中的列。
```bash
db.cool_new_stuff.insertOne({});//创建一个collection
```
![图 3](images/33ced5f3f10adbf0bc55d49008497913bf5a6b6024aa47df044045841cd88a87.png)  

```bash
db.cool.new.stuff.insertOne({});
```
![图 4](images/f3daafb7b68fa437c31b16c26d1050d4f3422f7b9889e8b8efaa99997666c8ea.png)  

* 查看所有的collection
```
mydb2> show collections
cool_new_stuff
cool.new.stuff
some
```

# 四.查找

## 1.清屏

```bash
cls
```

## 2.查看数据
* 查找某个collection中的所有数据
```bash
mydb1> db
mydb1
mydb1> show collections
mongo
mydb1> db.mongo.find();
[
  {
    _id: 'ac3',
    name: 'AC3 Phone',
    brand: 'ACME',
    type: 'phone',
    price: 200,
    rating: 3.8,
    warranty_years: 1,
    available: true
  },
  {
    _id: 'ac7',
    name: 'AC7 Phone',
    brand: 'ACME',
    type: 'phone',
    price: 320,
    rating: 4,
    warranty_years: 1,
    available: false
  },
...
```

* db.mongo.find({"type":"service"})
![图 5](images/bdc8b1ec171037e39bbab469706bbc3cb0e9976859ce52734a71dab415a62281.png)  

* 只显示单一字段type
```bash
db.mongo.find({},{"type":1});
```
![图 6](images/b236ec073fef9457c5229ef59614b9a36cc07668c7ad145921b57948d9809879.png)  

```bash
db.mongo.find({"type":"service"},{"type":1});
```
![图 7](images/05886659303e95d1a6dd6ff7c738d2a106797e384c52da407c045307e66c9cf8.png)  

* 正则
```bash
db.mongo.find({"name":{$regex: /phone/i}},{"name":1,"type":1});//查找name中包含phone的数据，i表示不区分大小写
```
![图 8](images/c9191a52b95e1e24cdd3660513beaa1db411f812a47c471cc54c7cd68c620e11.png)  

# 五.Query
## 1.limit与count
* limit返回数组
```bash
db.mongo.find({"name":{$regex: /phone/i}}).limit(2);
```
![图 9](images/083bf6fab13c8f00ab7765e8191aabd29e593f3c835abec7200927f9edb4aa63.png)  

* count返回数字
```bash
db.mongo.find({"name":{$regex: /phone/i}}).count();
```
![图 10](images/f3ba8d80969d6e109548ccf9dd400ef8b122913e76634934e1343200025b36ae.png)  

## 2.sort与skip
* sort排序
  * 顺序排序sort({"some":1})
  * 逆序排序sort({"some":-1})

```bash
db.mongo.find({},{"name":1,"type":1}).sort({"name":1});
```
![图 11](images/049b79ef296f79c5b1c7b28d5a18175ddd5aee022bc2c7ad7331bdfe43ed16df.png)  

* skip跳过几个数据
```bash
db.mongo.find({},{"name":1,"type":1}).sort({"name":1}).skip(2);
```
![图 12](images/1693d1e378e4fcdd0758ae9f0d62300770ea8fab4a3f55bfda061d5789e48536.png)  

## 3.$gt与$lt
* $gt大于
db.mongo.find({"price":{$gt:29}},{"name":1,"type":1});
```
![图 13](images/da3d798ed14a2466771f38f0e549e17606e85212bc7e8de2573a589f2fd12e2f.png)  
```

## 4.$in、$or、$and、$all
* $or
```bash
db.mongo.find({$or:[{"price":{$lt:30}},{"rating":{$gt:3}}]},{"price":1,"rating":1});
```
![图 14](images/d706232795a93dcf618a4c7fd135a41345cdd15c0c4e1983e4769e3f15cd849a.png)  

* $and
```bash
db.mongo.find({$and:[{"price":{$lt:30}},{"rating":{$gt:3}}]},{"price":1,"rating":1});
```
![图 15](images/6741cb14997ff04be79dcbebb0c7246a961607806417d1f6a32b258ac5a6714c.png)  

# 六.修改数据
## 1.字段操作 $set、$unset、$inc

updateOne()修改一条数据

* $set修改数据
```bash
db.mongo.updateOne({"name":"AC3 Case Red"},{$set:{"price":11}});
```
![图 16](images/ff987fa643a20f0fa9f2fe20378c9969c315166936cfb7706aa563ab30294f7f.png)  

查询修改结果是否正确
![图 17](images/6b8f7530bf6e87d87f175d8b5d717f7206382571e95dd7d0e35fdc5f4bb789d4.png)  

* $unset删除数据
```bash
db.mongo.updateOne({"name":"AC3 Case Red"},{$unset:{"color":1}});
```
![图 18](images/8e91b48d00d00251bd4f27a56ac14d468337d1e10790e3e84477bc1042717c6c.png)  

查询修改结果是否正确
![图 19](images/c968cc447ec510d646f12028e836d7ee748cede600b142b13d2b03f99075a06d.png)  

* $inc增加数据
```bash
db.mongo.updateOne({"name":"AC3 Case Red"},{$inc:{"price":10}});
```
![图 20](images/ddced5b694eea84bbac55b22301531d1bda2d05cfcb9b084a5b70baa127d489f.png)  

查询修改结果是否正确
![图 21](images/afd0a727acd85f79f51e67383bfe6c2ee24f24c7dbefa485ef1a43ee7bebefa0.png)  

## 2.数组操作 $push、$pull
* $push添加数据
```bash
db.mongo.updateOne({"name":"AC3 Case Red"},{$push:{"type":"new_type"}});
```
![图 22](images/ffd7070a6c21ba6b11a0b6ad6521918743eb49afe2ac9698352e0adee7fe9d36.png)  

查询修改结果是否正确
![图 23](images/fc6ee854744bc98450bbae59b9e5a274bbf137f9806974ee9d69efe04db1d803.png)  

* $pull删除数据
将上述$pull的部分改为$push，即可删除数据

## 3.delete
* deleteOne()删除一条数据
```bash
db.mongo.delete({"_id":"ac7"});
```
![图 24](images/228e29daaa394318f5eec13a5d2539f5bbb978dc408064c439bfa10842bb6a75.png)  

# 七.mongoDB与nodejs的结合
## 1.新建项目
```bash
mkdir mongo_pro1
```
![图 1](images/5a3eb0b7e9ae0c72c9fc576202ade43594f19a021673f374836ab9f6379670db.png)  
## 2.切换到项目目录
```bash
cd mongo_pro1
```
## 3.初始化项目
```bash
npm init -y
```
## 4.打开vscode
```bash
code .
```
## 5.安装依赖
```bash
npm i mongodb
```
