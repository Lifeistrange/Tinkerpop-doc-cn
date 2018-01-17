# drop步骤

`drop()`步骤(filter/sideEffect)用来删除图中的元素与属性。它也是一个过滤步骤因为它不会返回任何对象。

```groovy
gremlin> g.V().outE().drop()
gremlin> g.E()
gremlin> g.V().properties('name').drop()
gremlin> g.V().valueMap()
==>[age:[29]]
==>[age:[27]]
==>[lang:[java]]
==>[age:[32]]
==>[lang:[java]]
==>[age:[35]]
gremlin> g.V().drop()
gremlin> g.V()
```

