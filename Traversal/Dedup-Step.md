# dedup步骤

使用`dedup()`步骤(filter)，重复的对象会被过滤掉。注意，如果一个批量遍历的量大于1，那么他会在传输出之前被设置为1。

```groovy
gremlin> g.V().values('lang')
==>java
==>java
gremlin> g.V().values('lang').dedup()
==>java
gremlin> g.V(1).repeat(bothE('created').dedup().otherV()).emit().path() //1\
==>[v[1],e[9][1-created->3],v[3]]
==>[v[1],e[9][1-created->3],v[3],e[11][4-created->3],v[4]]
==>[v[1],e[9][1-created->3],v[3],e[12][6-created->3],v[6]]
==>[v[1],e[9][1-created->3],v[3],e[11][4-created->3],v[4],e[10][4-created->5],v[5]]
```

1. 遍历所有`created`边，但是不会显示任何边两次。

如果一个`by()`步骤跟在`dedup()`后，那么会在确定是否显示对象前就对其处理。

```groovy
gremlin> g.V().valueMap(true, 'name')
==>[name:[marko],label:person,id:1]
==>[name:[vadas],label:person,id:2]
==>[name:[lop],label:software,id:3]
==>[name:[josh],label:person,id:4]
==>[name:[ripple],label:software,id:5]
==>[name:[peter],label:person,id:6]
gremlin> g.V().dedup().by(label).values('name')
==>marko
==>lop
```

最后，如果`dedup()`给了一列字符串，那么它会确认根据历史路径上的对应的遍历来去重，而不是当前遍历的对象。

```groovy
gremlin> g.V().as('a').out('created').as('b').in('created').as('c').select('a','b','c')
==>[a:v[1],b:v[3],c:v[1]]
==>[a:v[1],b:v[3],c:v[4]]
==>[a:v[1],b:v[3],c:v[6]]
==>[a:v[4],b:v[5],c:v[4]]
==>[a:v[4],b:v[3],c:v[1]]
==>[a:v[4],b:v[3],c:v[4]]
==>[a:v[4],b:v[3],c:v[6]]
==>[a:v[6],b:v[3],c:v[1]]
==>[a:v[6],b:v[3],c:v[4]]
==>[a:v[6],b:v[3],c:v[6]]
gremlin> g.V().as('a').out('created').as('b').in('created').as('c').dedup('a','b').select('a','b','c') //1\
==>[a:v[1],b:v[3],c:v[1]]
==>[a:v[4],b:v[5],c:v[4]]
==>[a:v[4],b:v[3],c:v[1]]
==>[a:v[6],b:v[3],c:v[1]]
```

1. 如果当前`a`和`b`曾出现过，那么就会被过滤。