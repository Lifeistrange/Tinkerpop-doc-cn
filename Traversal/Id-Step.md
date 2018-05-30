# id步骤

`id()`步骤(map)用来操作元素，获取其标识符。

```groovy
gremlin> g.V().id()
==>1
==>2
==>3
==>4
==>5
==>6
gremlin> g.V(1).out().id().is(2)
==>2
gremlin> g.V(1).outE().id()
==>9
==>7
==>8
gremlin> g.V(1).properties().id()
==>0
==>1
```

