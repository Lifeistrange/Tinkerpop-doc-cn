# label步骤

`label()`步骤(map)用来显示来自元素的标签。

```groovy
gremlin> g.V().label()
==>person
==>person
==>software
==>person
==>software
==>person
gremlin> g.V(1).outE().label()
==>created
==>knows
==>knows
gremlin> g.V(1).properties().label()
==>name
==>age
```

