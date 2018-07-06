# store步骤

当我们需要延迟聚合操作时，`store()`步骤(sideEffect)可以代替`aggregate()`来达成该目的。这两个步骤的区别在于`store()`不会阻塞并且会将对象存入他的副作用集合中。

```groovy
gremlin> g.V().aggregate('x').limit(1).cap('x')
==>[v[1],v[2],v[3],v[4],v[5],v[6]]
gremlin> g.V().store('x').limit(1).cap('x')
==>[v[1],v[2]]
```

我们注意到，即使我们限制了结果只取1个对象`store()`依然返回了两个结果。当第二个对象通过了了`range()`过滤器，他也通过了`store()`，这说明存储是先于过滤的。

```groovy
gremlin> g.E().store('x').by('weight').cap('x')
==>[0.5,1.0,1.0,0.4,0.4,0.2]
```

