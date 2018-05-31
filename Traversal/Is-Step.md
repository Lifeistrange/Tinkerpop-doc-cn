# is步骤

`is()`步骤(filter)用来筛选标量值。

```groovy
gremlin> g.V().values('age').is(32)
==>32
gremlin> g.V().values('age').is(lte(30))
==>29
==>27
gremlin> g.V().values('age').is(inside(30, 40))
==>32
==>35
gremlin> g.V().where(__.in('created').count().is(1)).values('name') //1\
==>ripple
gremlin> g.V().where(__.in('created').count().is(gte(2))).values('name') //2\
==>lop
gremlin> g.V().where(__.in('created').values('age').
                                    mean().is(inside(30d, 35d))).values('name') //3\
==>lop
==>ripple
```

1. 找到只用一个作者的项目。
2. 找到有两个以上作者的项目。
3. 找到作者平均年龄在30到35之间的项目。