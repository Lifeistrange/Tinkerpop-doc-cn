# order步骤

当遍历流中的数据需要排序时，`order()`步骤(map)就可以派上用场了。

```groovy
gremlin> g.V().values('name').order()
==>josh
==>lop
==>marko
==>peter
==>ripple
==>vadas
gremlin> g.V().values('name').order().by(decr)
==>vadas
==>ripple
==>peter
==>marko
==>lop
==>josh
gremlin> g.V().hasLabel('person').order().by('age', incr).values('name')
==>vadas
==>marko
==>josh
==>peter
```

遍历中的大多数对象都是`Elememt`。一个元素可以有多个属性。在一些情况中国，我们会想要通过比较他们的属性来排序。

```groovy
gremlin> g.V().values('name')
==>marko
==>vadas
==>lop
==>josh
==>ripple
==>peter
gremlin> g.V().order().by('name',incr).values('name')
==>josh
==>lop
==>marko
==>peter
==>ripple
==>vadas
gremlin> g.V().order().by('name',decr).values('name')
==>vadas
==>ripple
==>peter
==>marko
==>lop
==>josh
```

`order()`步骤允许用户提供多个比较器来排序，比较器间有顺序区别。在下面的例子中，我们会先用向外的created边计数排序，再用人的年龄排序。

```groovy
gremlin> g.V().hasLabel('person').order().by(outE('created').count(), incr).
                                          by('age', incr).values('name')
==>vadas
==>marko
==>peter
==>josh
gremlin> g.V().hasLabel('person').order().by(outE('created').count(), incr).
                                          by('age', decr).values('name')
==>vadas
==>peter
==>marko
==>josh
```

我们也可以使用`Order.shuffle`来随机打乱遍历结果。

```groovy
gremlin> g.V().hasLabel('person').order().by(shuffle)
==>v[1]
==>v[4]
==>v[2]
==>v[6]
gremlin> g.V().hasLabel('person').order().by(shuffle)
==>v[6]
==>v[4]
==>v[2]
==>v[1]
```

我们可以使用`order(local)`来排序本地对象而不是整个遍历流。这个操作支持`Collection`和`Map`对象。对于其他对象，返回值不会有任何变化。

```groovy
gremlin> g.V().values('age').fold().order(local).by(decr) //1\
==>[35,32,29,27]
gremlin> g.V().values('age').order(local).by(decr) //2\
==>29
==>27
==>32
==>35
gremlin> g.V().groupCount().by(inE().count()).order(local).by(values, decr) //3\
==>[1:3,0:2,3:1]
gremlin> g.V().groupCount().by(inE().count()).order(local).by(keys, incr) //4\
==>[0:2,1:3,3:1]
```

1. 年龄被收集到列表中并使用降序排列。
2. 年龄没有收集到列表中，导致`order(local)`排序了单个数字，整体未被排序。
3. 将`groupCount()`字典根据值降序排序。
4. 将`groupCount()`字典根据键升序排序。

> 要枚举字段中的键值需要将其从Map,Map.Entry,Path中先取出。