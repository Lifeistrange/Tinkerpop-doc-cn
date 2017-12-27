# addV步骤

`addV()`步骤用来向图中添加节点(map/sideEffect)。每一个进入的对象，都会创建一个节点。此外，`GraphTraversalSource`主要也是一个`addV()`方法。

```groovy
gremlin> g.addV('person').property('name','stephen')
==>v[13]
gremlin> g.V().values('name')
==>marko
==>vadas
==>lop
==>josh
==>ripple
==>peter
==>stephen
gremlin> g.V().outE('knows').addV().property('name','nothing')
==>v[15]
==>v[17]
gremlin> g.V().has('name','nothing')
==>v[17]
==>v[15]
gremlin> g.V().has('name','nothing').bothE()
```

