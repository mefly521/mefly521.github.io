### $unwind

将aggregate查询结果进行打散,如果结果中有一条数据,数据中有一个数组,数组中有三结束, unwind 后会变成三条数据,如下

未 unwind前的结果

```json
{ 
    "_id" : 6.0, 
    "title" : "title6", 
    "tab" : [
        "chinese", 
        "us", 
        "uk"
    ]
}
```

执行unwind 命令

```
db.test_mf.aggregate([
    {$project: {"tab":1,"title":1}},
    {$unwind:"$tab"}
])
```

结果

```
{ 
    "_id" : 6.0, 
    "title" : "title6", 
    "tab" : "chinese"
}
{ 
    "_id" : 6.0, 
    "title" : "title6", 
    "tab" : "us"
}
{ 
    "_id" : 6.0, 
    "title" : "title6", 
    "tab" : "uk"
}
```

