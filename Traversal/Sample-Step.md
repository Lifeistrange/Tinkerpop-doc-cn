# sample步骤

`sample()`步骤在遍历器中在遍历前提供样例数值。

```groovy
gremlin> g.V().outE().sample(1).values('weight')
==>0.5
gremlin> g.V().outE().sample(1).by('weight').values('weight')
==>0.5
gremlin> g.V().outE().sample(2).by('weight').values('weight')
==>1.0
==>1.0
```

当我们在`local()`的联合运算中使用`sample()`将会非常有趣。这两个步骤的组合会使遍历的路径随机选择。在下面的例子中，遍历开始于顶点1，然后我们选择了一个这个点的边并对其生成了一个权重。输出是遍历器所选择的边生成的一条路径，然后遍历器会在一条单一的路径上持续遍历。

```groovy
gremlin> g.V(1).repeat(local(
                  bothE().sample(1).by('weight').otherV()
                )).times(5)
==>v[4]
gremlin> g.V(1).repeat(local(
                  bothE().sample(1).by('weight').otherV()
                )).times(5).path()
==>[v[1],e[9][1-created->3],v[3],e[11][4-created->3],v[4],e[10][4-created->5],v[5],e[10][4-created->5],v[4],e[11][4-created->3],v[3]]
gremlin> g.V(1).repeat(local(
                  bothE().sample(1).by('weight').otherV()
                )).times(10).path()
==>[v[1],e[7][1-knows->2],v[2],e[7][1-knows->2],v[1],e[8][1-knows->4],v[4],e[8][1-knows->4],v[1],e[8][1-knows->4],v[4],e[11][4-created->3],v[3],e[9][1-created->3],v[1],e[9][1-created->3],v[3],e[12][6-created->3],v[6],e[12][6-created->3],v[3]]
```

