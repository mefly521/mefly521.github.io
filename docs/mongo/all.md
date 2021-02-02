## 查询

### 查询集群

查看集群中各节点的状态：

```
rs0:PRIMARY> rs.status()
```

2、查看集群中各节点配置情况：

```
rs0:PRIMARY> rs.conf()
```





### 多表联查

详解MongoDB中的多表关联查询（$lookup） - 东山絮柳仔 - 博客园
https://www.cnblogs.com/xuliuzai/p/10055535.html

### $first

(6条消息) mongdb系列之 aggregate展示未被分组的字段_sswltt的博客-CSDN博客
https://blog.csdn.net/sswltt/article/details/105688398

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

### 操作符

用mongoTemplate的andExpression表达式来表示

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

### 对 group by 后的结果再求合

```
db.EV_DECLARE.aggregate([
    {$group: {_id: "$workGroupId",count:{$sum:1}}},
    {$group:
         {
          	_id : "total",
           totalAmount: { $sum: 1 }
     }}
])
```

MongoDB 中聚合统计计算--$SUM表达式 - 东山絮柳仔 - 博客园
https://www.cnblogs.com/xuliuzai/p/11400546.html





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

MongoDB是原生支持高可用的, 主结点故障时，自动切换替换结点

原理
https://segmentfault.com/a/1190000021390186

演练

https://segmentfault.com/a/1190000021402232



## 读写分离

(6条消息) springboot配置多个mongodb实现动态切换_Mr_Dai的博客-CSDN博客
https://blog.csdn.net/DWL0208/article/details/106467598/



崛起于Springboot2.X+ MongoDB读写分离（25） 
https://my.oschina.net/mdxlcj/blog/1859527





## 工具

https://www.mongodb.com/try/download/compass

## 索引

aggregate explain 分析

1. 
   db.doctorinfo.aggregate(
2. [ { "$match" : {"provinceId":13, "doctorService_phone" : "1" , "displayStatus" : 1}},
3. { "$group" : { "_id" : { "standardDeptId" : "$standardDeptId"} , "standardDeptId" : { "$first" : "$standardDeptId"}}},
4. { "$project" : { "standardDeptId" : 1}}],
5. {explain:true}
6. )