### $lookup 

主要功能是 实现多表的联查,像mysql 的join 一样

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

### 