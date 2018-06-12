# not步骤

`not()`步骤(filter)会移除经过遍历参数不返回对象的遍历流上的对象。

```groovy
gremlin> g.V().not(hasLabel('person')).valueMap(true)
==>[name:[lop],label:software,id:3,lang:[java]]
==>[name:[ripple],label:software,id:5,lang:[java]]
gremlin> g.V().hasLabel('person').
           not(out('created').count().is(gt(1))).values('name') //1\
==>marko
==>vadas
==>peter
```

1. josh创建了两个项目而vadas一个都没有。