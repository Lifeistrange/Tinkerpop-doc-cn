# cap步骤

`cap()`步骤(barrier)迭代遍历到他的位置，然后通过所给的键来触发副作用。如果给多个键，那么就会以`Map<String, Object>`的形式来触发副作用。

```groovy
gremlin> g.V().groupCount('a').by(label).cap('a') //1\
==>[software:2,person:4]
gremlin> g.V().groupCount('a').by(label).groupCount('b').by(outE().count()).cap('a','b') //2\
==>[a:[software:2,person:4],b:[0:3,1:1,2:1,3:1]]
```

1. 根据标签分组统计顶点数。触发通过标签统计的被标记为‘a’的副作用。
2. 和第一种情况相同，但是同时触发了统计边的数量的标签为‘b’的副作用。