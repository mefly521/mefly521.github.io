## 查询

### select by id 

db.collect.find({ "_id" : ObjectId("5326bfc0e6f780b21635248f") })

### 查询某个字段存在

MongoDB查询某个字段存在的语句

```bash
db.getCollection('tableName').find({"RouteInfo":{"$exists":true}})
```

查询某个字段不存在的语句

```bash
db.getCollection('tableName').find({"RouteInfo":{"$exists":false}})		
```

### 指定显示某个字段

```
db.EV_DECLARE.find({isTrappedPerson:true}, { elevatorId: 1})
```

### count(*)

db.spilder_news.find**({})**.count**()**

### order by

db.spilder_news.find**({})**.sort**({**"date"**:**1**})  正序**

db.spilder_news.find**({})**.sort**({**"date"**:**-1**}) 降序**

### 多表联查

详解MongoDB中的多表关联查询（$lookup） - 东山絮柳仔 - 博客园
https://www.cnblogs.com/xuliuzai/p/10055535.html

### $first

(6条消息) mongdb系列之 aggregate展示未被分组的字段_sswltt的博客-CSDN博客
https://blog.csdn.net/sswltt/article/details/105688398

### $lookup 

orders表：

```
{
    "_id" : 1,
    "product_id" : 154,
    "status" : 1
}
 
{
    "_id" : 2,
    "product_id" : 155,
    "status" : 1
}

```

products表：

```
{
    "_id" : 154,
    "name" : "笔记本电脑"
}
{
    "_id" : 155,
    "name" : "耳机"
}
{
    "_id" : 156,
    "name" : "台式电脑"
}
```

lookup

```
{$lookup:
    {
        from:'products', //关联查询表2
        localField:'product_id', //关联表1的商品编号ID
        foreignField:'_id',  //匹配表2中的ID与关联表1商品编号ID对应
        as:'orderLists'  //满足 localField与foreignField的信息加入orderlists集合
    }
}
```

### $unwind

先group 再 lookup 再 unwind

```json
db.EV_DECLARE.aggregate([
    {$group: {_id: "$workGroupId",count:{$sum:1}}},
    {$lookup:
       {
         from: "EV_WORK_GROUP",
         localField: "_id",
         foreignField: "_id",
         as: "group"
       }
    }
    ,{$project:{_id:0,'teamName':'$group.teamName',count:'$count'}},
    {$unwind:"$teamName"}
])
```

### $push 拼成一个数组

```json
db.artwork.aggregate( [
  {
    $bucket: {
      groupBy: "$price", // 对prices字段进行统计查询
      boundaries: [ 0, 200, 400 ], // 数据的边界范围，包左不包右
      default: "Other", // 不在boundaries内的默认名称
      output: {
        "count": { $sum: 1 }, // 统计
        "titles" : { $push: "$title" } // 将改范围内的title组成数组
      }
    }
  }
] )
```

### $out

![image-20210127171255895](http://img.playgame.love/image-20210127171255895.png)





## java

### objectId

```
public DBObject findDocumentById(String id) {
    BasicDBObject query = new BasicDBObject();
    query.put("_id", new ObjectId(id));
    DBObject dbObj = collection.findOne(query);
    return dbObj;
}
```

## 聚合

《MongoDB高手课》学习记录（第四天） 
https://segmentfault.com/a/1190000021364343

<img src="http://img.playgame.love/image-20210126171221323.png" alt="image-20210126171221323" style="zoom: 80%;" />



![image-20210126171731759](http://img.playgame.love/image-20210126171731759.png)

unwind 展开数组

<img src="http://img.playgame.love/image-20210126171829509.png" alt="image-20210126171829509" style="zoom:80%;" />

<img src="http://img.playgame.love/image-20210126172028792.png" alt="image-20210126172028792" style="zoom:80%;" />

<img src="http://img.playgame.love/image-20210126172152434.png" alt="image-20210126172152434" style="zoom: 70%;" />



<img src="http://img.playgame.love/image-20210126172325143.png" alt="image-20210126172325143" style="zoom:80%;" />

<img src="http://img.playgame.love/image-20210126172616387.png" alt="image-20210126172616387" style="zoom:80%;" />

<img src="http://img.playgame.love/image-20210126172844401.png" alt="image-20210126172844401" style="zoom:80%;" />

## 复制集

原理
https://segmentfault.com/a/1190000021390186

演练

https://segmentfault.com/a/1190000021402232



## 读写分离

(6条消息) springboot配置多个mongodb实现动态切换_Mr_Dai的博客-CSDN博客
https://blog.csdn.net/DWL0208/article/details/106467598/



崛起于Springboot2.X+ MongoDB读写分离（25） - 纯粹而又极致的光--木九天 - OSCHINA - 中文开源技术交流社区
https://my.oschina.net/mdxlcj/blog/1859527





## 工具

https://www.mongodb.com/try/download/compass