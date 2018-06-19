# peerPresure步骤

`peerPressure()`步骤(map/sideEffect)会使用`PeerPresureVertexPergram`聚合计算顶点。

> `peerPressure()`步骤是一个顶点计算步骤，因此也只能使用在支持图计算(OLAP)的图中。

```groovy
gremlin> g = graph.traversal().withComputer()
==>graphtraversalsource[tinkergraph[vertices:6 edges:6], graphcomputer]
gremlin> g.V().peerPressure().by('cluster').values('cluster')
==>1
==>1
==>1
==>1
==>1
==>6
gremlin> g.V().hasLabel('person').
           peerPressure().by('cluster').
           group().by('cluster').by('name')
==>[1:[marko,vadas,josh],6:[peter]]
```

