# key步骤

`key()`步骤(map)从属性中获取键。

```groovy
gremlin> g.V(1).properties().key()
==>name
==>location
==>location
==>location
==>location
gremlin> g.V(1).properties().properties().key()
==>startTime
==>endTime
==>startTime
==>endTime
==>startTime
==>endTime
==>startTime
```

