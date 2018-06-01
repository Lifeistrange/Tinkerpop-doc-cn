# limit步骤

`limit()`步骤类似于`range()`步骤设置了最低边界为0。

```groovy
gremlin> g.V().limit(2)
==>v[1]
==>v[2]
gremlin> g.V().range(0, 2)
==>v[1]
==>v[2]
gremlin> g.V().limit(2).toString()
==>[GraphStep(vertex,[]), RangeGlobalStep(0,2)]
```

`limit()`步骤也可以用`Scope.local`调用，来操作集合内部。

```groovy
gremlin> g.V().valueMap().select('location').limit(local,2) //1\
==>[san diego,santa cruz]
==>[centreville,dulles]
==>[bremen,baltimore]
==>[spremberg,kaiserslautern]
gremlin> g.V().valueMap().limit(local, 1) //2\
==>[name:[marko]]
==>[name:[stephen]]
==>[name:[matthias]]
==>[name:[daniel]]
==>[name:[gremlin]]
==>[name:[tinkergraph]]
```

1. `List<String>`遍历每个节点所包含的前两个地址。
2. `Map<String, Object>`遍历每个节点，展示他所包含的第一个属性。